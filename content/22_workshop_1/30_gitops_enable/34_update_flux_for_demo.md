---
title: "Managing configuration with GitOps"
chapter: false
weight: 34
---

Lets look at how configuration is managed following GitOps by modifying declarations in the repository created by Flux v2.

To demonstrate this, you will change the default polling interval configured by Flux in the `Kustomization` that pulls changes from the `./clusters/aws-workshop` path in the repository. This change will also help accelerate changes being applied to the cluster, which will make the demo go smoother.

First, check the current configuration as applied to the cluster, by running the following command:

```bash
kubectl get kustomization flux-system -n flux-system  -o yaml
```

You should see output similar to the one shown below, pay close attention to `spec.interval` (the current value will be `10m0s`):

<pre>
apiVersion: kustomize.toolkit.fluxcd.io/v1beta1
kind: Kustomization
metadata:
	.....
  name: flux-system
  namespace: flux-system
	.....
spec:
  force: false
  interval: 10m0s
  path: ./clusters/aws-workshop
  prune: true
  sourceRef:
    kind: GitRepository
    name: flux-system
  validation: client
	.....
</pre>

Now, open up the file `./clusters/aws-workshop/flux-system/gotk-sync.yaml`. You will find two objects declared in this file, a `GitRepository`and a `Kustomization`, you will notice how the contents of the `Kustomization` match the configuration shown in the output of the previous command. 

Edit the file using your favorite editor, and set `spec.interval` in the `Kustomization` to `1m0s`, this will reduce the polling frequency to one minute:

```yaml
 20 spec:
 21   interval: 1m0s
 22   path: ./clusters/aws-workshop
```

Now commit and push this change to the repo using the following command:

```bash
git commit -am "Changing frequency of polling"
git push
```

If you wait a few seconds, and run the following command again, you will see the change applied to the object's configuration automatically.

```bash
kubectl get kustomization flux-system -n flux-system  -o yaml
```