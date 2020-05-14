---
title: "Create OPA Constraints"
draft: false
weight: 20
---

Now that we have our `ContraintsTemplate` configured and deployed into the cluster, we can now start creating the constraints.

Going back to our templates, we defined a crd called `K8sRequiredLabels` with a set of fields and values we could use.

Here's an example of what we could do with this:

```yaml
apiVersion: constraints.gatekeeper.sh/v1beta1
kind: K8sRequiredLabels
metadata:
  name: all-must-have-owner
spec:
  match:
    kinds:
      - apiGroups: [""]
        kinds: ["Namespace"]
  parameters:
    message: "All namespaces must have an `owner` label that points to your company username"
    labels:
      - key: owner
        allowedRegex: "^[a-zA-Z]+.agilebank.demo$"
```

Looking at `spec.match` We are saying only apply this constraint to `Namespace` objects.

{{% notice info %}}
Note as well the fields we defined previously `kind: K8sRequiredLabels`, `parameters`. This is where we are able to pass unique values in to here.
{{% notice info %}}

In this case, we've added a sepcific message for this contraint as well as the labels we require whenver a namespace is created or updated and the regex of that label.


lets take a look at anoother example :

```yaml
apiVersion: constraints.gatekeeper.sh/v1beta1
kind: K8sAllowedRepos
metadata:
  name: prod-repo-is-openpolicyagent
spec:
  match:
    kinds:
      - apiGroups: [""]
        kinds: ["Pod"]
    namespaces:
      - "production"
  parameters:
    repos:
      - "only-this-repo"
```

Remember we created a template to only allow certain repos based on a given set of rules.

The above shows an exampe of how we can use this in our cluster.

In this case we have set this to apply to all **pods** in the **production** namespace and we have added a `parameter.repos` of "only this repo" - feel free to changes this to test it out.

Let's grab these example templates and check them in to git!


```bash
mkdir opa/contraints
```

```
curl https://weaveworks-gitops.awsworkshop.io/30_workshop_03_grc/130_create_policy_contraints/deploy.files/alowed-repos.yaml -o opa/constraints/alowed-repos.yaml
```
```
curl https://weaveworks-gitops.awsworkshop.io/30_workshop_03_grc/130_create_policy_contraints/deploy.files/require-labels.yaml -o opa/constraints/require-labels.yaml
```
Once you ar happy these have downloaded, lets check them in to git:

```
git add "opa/contraints/"
```
```
git commit -m "adding test constraints"
```
```
git push
```



