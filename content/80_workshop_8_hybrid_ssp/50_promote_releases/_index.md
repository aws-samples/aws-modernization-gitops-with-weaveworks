+++
title = "Release changes across environments"
chapter = true
weight = 50
+++

# Release Changes Across Environments

Now, to demonstrate how a team would promote changes across these environments, lets look at **Team Green**.

Change the version of the image used in the `dev/patches.yaml` file, and see how this new image is released to the development cluster, now, change the version in `prod/patches.yaml` and you'll see the same change being promoted to production as well. 

## Bonus points: Demonstrate security in action

What would happen if **Team Green** would try to deploy resources to some namespace that is not the one you assigned for them?

Give it a try, save this manifest as `team-green/base/badteam.yaml` and add it to the kustomization - notice how the namespace used is **not** `green-ns`:

```yaml
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  namespace: blue-ns
spec:
  selector:
    matchLabels:
      app: nginx
  replicas: 1
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.14.2
        ports:
        - containerPort: 80 
```

Take a look at the `green-apps` `Kustomization`!

```shell
kubectl get Kustomization green-apps -n green-ns
```