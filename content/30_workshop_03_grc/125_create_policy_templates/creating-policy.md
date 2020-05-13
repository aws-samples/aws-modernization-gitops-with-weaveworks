---
title: "Create OPA Policy"
date: 2020-04-12T18:00:00-00:00
draft: false
weight: 20
---

We are going to create 3 roles:

- a **allowed-repos** this will determine what image repos can ben used in production
- a **container-resource-limits** this wil restrict max resource limits for cpu/memory on containers. It wil also enforce that a limit must be set
- a **read-only-root-file-syste** this will demo that we can achieve pod security policies using opa instead of k8s built in admission controller. This can be more granular and means we can just target certain pods/namespaces/labels etc
- **require-labels** - this policy requires that namespaces must have labels that match a regex value

# Let's download some samples for the above


Run the following commands to download some policy examples to your local git repo:

```bash

curl xxxx -o allowed-repos.yaml

curl xxxx -o container-resource-limits.yaml

curl xxxx -o read-only-root-filesystem.yaml

curl xxxx -o require-labels.yaml

```

# Explore the template

Lets understand what's happening and what each part of this template is doing.

If we look at **require-labels.yaml**

We can see this is a **ConstraintTemplate** object. If you remember this crd was created when we first deplopyed opa-gatekeeper.


Okay, te metadata has the name of the templae which is standard stuff, let's look at the next part, we can see inside the **spec** that this template is going to create another crd!!

```
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

Remember, this is only a template, so that means that on its own, its not going to do anything, it needs to be consumed by something else. So every constraint template needs to create constraint objects so it can be used (and it does this by creating crd's). Hopefully that makes sense but don't worry we will show some examples shortly.

The above example shows the outline for the crd, in this case it's called `K8sRequiredLabels` it also contains the parameters of the constraint, in this case its `labels` which is an array of items you can add (`key` + `allowedRegex`).

There is also a `message` string which can be used add different messages to the template.

This allows us to create simple constraints with just a few parameters and re-use this template.

The next part of the constraints template is `targets`. This is where the rego code (policy rules) are added..

### Targets


**You can only have 1 target per constraint**

This contains the `rego` policy you should use to define what rules are applied.

Let's take a look:

```
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


Okay, as we discussed earlier, this is just a **contraint template**, its not a constraint and it does nothing when deployed on it's own.

Lets now look at constraints and start using this template.


