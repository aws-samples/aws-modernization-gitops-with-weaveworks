---
title: "Deploy Test Apps"
date: 2020-04-12T18:00:00-00:00
draft: false
weight: 20
---

Now that we havwe defined our contraint templates and deployed some contraints, let's see if they work!


### Test image repos in prod namespace


Earlier we define that all pods in the production namespace must only use images from `xxxx` repo. Let's deploy a pod using a different unauthorized repo.

Here's an example:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: opa
  namespace: production
  labels:
    owner: me.agilebank.demo
spec:
  containers:
    - name: opa
      image: openpolicyagent/opa:0.9.2
      args:
        - "run"
        - "--server"
        - "--addr=localhost:8080"
      resources:
        limits:
          cpu: "100m"
          memory: "30Mi"
```

Let's add this to our git repo

```bash
mkdir example-apps

curl https://weaveworks-gitops.awsworkshop.io/30_workshop_03_grc/deploy.files/pod-unauthorized-repo-example.yaml -o example-apps/pod-unauthorized-repo.yaml

git add example-apps/pod-unauthorized-repo.yaml

git commit -m "adding pod to test allowed repos"

git push
```

Check what happened to the pod, run the following:

```bash

kubectl get pods opa -n production

```
You should see it does not get deployed.

If we explore flux logs we should be able to see what happened and get the following error:

<pre>
Error from server ([denied by prod-repo-is-openpolicyagent] container <opa> has an invalid image repo <openpolicyagent/opa:0.9.2>, allowed repos are ["only-this-repo"]): error when creating "pod-unauthorized-repo.yaml": admission webhook "validation.gatekeeper.sh" denied the request: [denied by prod-repo-is-openpolicyagent] container <opa> has an invalid image repo <openpolicyagent/opa:0.9.2>, allowed repos are ["only-this-repo"]
</pre>

To get the logs run:

```bash
kubectl logs <flux-pod> -n fluxcd
```

If you run the following command you will get the same error:

```bash
kubectl create -f ./example-apps/pod-unauthorized-repo.yaml
```

### Lets test namespaces

Download this manifest and try to create this namespace:

```bash
mkdir namespaces
curl https://weaveworks-gitops.awsworkshop.io/30_workshop_03_grc/deploy.files/bad-namespace-example.yaml -o namespaces/bad-namespace.yaml
```

It should look like this:

```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: test-namespace
```

Lets run this manually for now so we can see it fail:

```bash
kubectl create -f ./namespaces/bad-namespace.yaml
```

<pre>
Error from server ([denied by all-must-have-owner] All namespaces must have an `owner` label that points to your company username): error when creating "bad-namespace.yaml": admission webhook "validation.gatekeeper.sh" denied the request: [denied by all-must-have-owner] All namespaces must have an `owner` label that points to your company username
</pre>

Let's make sure our policy allows namespaces to be created when the rules are met. Edit the manifest with the following:

```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: test-namespace
  labels:
    owner: testuser.agilebank.demo
```

Now let's check this in to git:

```bash
git add "namespaces/bad-namespace.yaml"

git commit -m "fixing bad namespace with label"

git push
```

You see that this has nwo been created.

<pre>
namespace/test-namespace created
</pre>


### Summary


We've proven now that the policies we've defined through OPA Gatekeeper Contraint templates have worked.

- We created contraint templates which then created our contraint crds `K8sAllowedRepos` and `K8sRequiredLabels`.

- This allowed us to declaritively created `K8sAllowedRepos` and `K8sRequiredLabels` objects and parameterise them. We can create more adn add different values and point to different namespaces or even different resources such as `services`, `secrets` etc


