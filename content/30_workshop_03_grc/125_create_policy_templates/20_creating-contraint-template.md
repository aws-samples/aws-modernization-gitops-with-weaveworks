---
title: "Create Contraint Templates"
date: 2020-04-12T18:00:00-00:00
draft: false
weight: 20
---

We are going to create 2 templates:

- **allowed-repos** this will determine what image repos can ben used in production
- **require-labels** - this policy requires that namespaces must have labels that match a regex value

# Let's download some samples for the above


Run the following commands to download some policy template examples to your local git repo:

```bash

mkdir opa/templates

curl -o https://weaveworks-gitops.awsworkshop.io/30_workshop_03_grc/125_create_policy_templates/deploy.files/allowed-repos.yaml > opa/templates/allowed-repos.yaml

curl https://weaveworks-gitops.awsworkshop.io/30_workshop_03_grc/125_create_policy_templates/deploy.files/require-labels.yaml > opa/templates/require-labels.yaml

```

# Explore the require-labels template

Lets understand what's happening and what each part of this template is doing.

If we look at [require-labels.yaml](.opa/templates/require-labels.yaml)

We can see this is a **ConstraintTemplate** object. If you remember this crd was created when we first deplopyed opa-gatekeeper.


Okay, the metadata has the name of the template which is standard stuff, let's look at the next part, we can see inside the **spec** that this template is going to create another crd!!

```yaml
spec:
  crd:
    spec:
      names:
        kind: K8sRequiredLabels
      validation:
        # Schema for the `parameters` field
        openAPIV3Schema:
          properties:
            message:
              type: string
            labels:
              type: array
              items:
                type: object
                properties:
                  key:
                    type: string
                  allowedRegex:
                    type: string
```

{{% notice info %}}
Remember, this is only a template, so that means that on its own, its not going to do anything, it needs to be consumed by something else. So every constraint template needs to create constraint objects so it can be used (and it does this by creating crd's). Hopefully that makes sense but don't worry we will show some examples shortly.
{{% notice info %}}

The above example shows the outline for the crd, in this case it's called `K8sRequiredLabels` it also contains the parameters of the constraint, in this case its `labels` which is an array of items you can add (`key` + `allowedRegex`).

There is also a `message` string which can be used add different messages to the template.

This allows us to create simple constraints with just a few parameters and re-use this template.

The next part of the constraints template is `targets`. This is where the rego code (policy rules) are added..

### Targets


**You can only have 1 target per constraint**

This contains the `rego` policy you should use to define what rules are applied.

Let's take a look:

```yaml
  targets:
    - target: admission.k8s.gatekeeper.sh
      rego: |
        package k8srequiredlabels
        get_message(parameters, _default) = msg {
          not parameters.message
          msg := _default
        }
        get_message(parameters, _default) = msg {
          msg := parameters.message
        }
        violation[{"msg": msg, "details": {"missing_labels": missing}}] {
          provided := {label | input.review.object.metadata.labels[label]}
          required := {label | label := input.parameters.labels[_].key}
          missing := required - provided
          count(missing) > 0
          def_msg := sprintf("you must provide labels: %v", [missing])
          msg := get_message(input.parameters, def_msg)
        }
        violation[{"msg": msg}] {
          value := input.review.object.metadata.labels[key]
          expected := input.parameters.labels[_]
          expected.key == key
          # do not match if allowedRegex is not defined, or is an empty string
          expected.allowedRegex != ""
          not re_match(expected.allowedRegex, value)
          def_msg := sprintf("Label <%v: %v> does not satisfy allowed regex: %v", [key, value, expected.allowedRegex])
          msg := get_message(input.parameters, def_msg)
        }
```

As we can see in the rego policy, we've set 2 `violations`, one flags if required labels are missing, the other flags up if the label exists **but** does not match the regex we specified in the contraint.

{{% notice info %}}
**Things to note** - when configuring rules you can add as many and configure them how you want but you must have the following format on at least one of them for this to work and flag as a violation:
{{% notice info %}}

```
violation[{"msg": msg, "details": {}}] {
  # rule body
}
```


As we discussed earlier, this is just a **contraint template**, its not a constraint and it does nothing when deployed on it's own.

# Explore the allowed-repos template

The format for this look svery similar, we have `metadata` and `name`, then inside the `spec` we are defining a crd called `K8sAllowedRepos`.

The accepted parameters are an array of `repos` which include item values.

```yaml
apiVersion: templates.gatekeeper.sh/v1beta1
kind: ConstraintTemplate
metadata:
  name: k8sallowedrepos
spec:
  crd:
    spec:
      names:
        kind: K8sAllowedRepos
      validation:
        # Schema for the `parameters` field
        openAPIV3Schema:
          properties:
            repos:
              type: array
              items:
                type: string
```

Next let's look at the targets:

```yaml  
targets:
    - target: admission.k8s.gatekeeper.sh
      rego: |
        package k8sallowedrepos
        violation[{"msg": msg}] {
          container := input.review.object.spec.containers[_]
          satisfied := [good | repo = input.parameters.repos[_] ; good = startswith(container.image, repo)]
          not any(satisfied)
          msg := sprintf("container <%v> has an invalid image repo <%v>, allowed repos are %v", [container.name, container.image, input.parameters.repos])
        }
        violation[{"msg": msg}] {
          container := input.review.object.spec.initContainers[_]
          satisfied := [good | repo = input.parameters.repos[_] ; good = startswith(container.image, repo)]
          not any(satisfied)
          msg := sprintf("container <%v> has an invalid image repo <%v>, allowed repos are %v", [container.name, container.image, input.parameters.repos])
        }
```

For this we've created 2 rule violations. Bot of which checks the provided container image and compares the beginning of it to the repos in our parameters.

If the image we use does not match what we have in our contraint, then it will trigger a violation. 

Lets check this in to git and next we can look at constraints and start using these templates.

```bash

git add opa/templates

git commit -m "adding opa templates"

git push
```


