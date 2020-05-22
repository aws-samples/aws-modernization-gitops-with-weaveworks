+++
title = "Install App Mesh Injector"
chapter = false
weight = 30
+++

For App Mesh to work each of your pods needs to have a sidecar. This sidecar handles the network routing and is the point in each of services that helps to enable functionality like the end-to-end observability.

We can manually add the sidecar to each of pods (via Deployments for example). However, this can become quite a maintenance burden and additionally this shouldn't be a concern of application teams.

Luckily, we can automatically inject the App Mesh sidecar into the pods. Kubernetes has a feature called [Dynamic Admission Controllers](https://kubernetes.io/docs/reference/access-authn-authz/extensible-admission-controllers/) that allow you to mutate the resources as they are applied to Kubernetes and this can be used to inject the sidecar.....and for this purpose there is the **App Mesh Injector**.

The AWS App Mesh Injector is also available as a Helm package from the [EKS Chart Repository](https://aws.github.io/eks-charts/). We will use the **Helm Operator** along with this chart to install the injector.

Create a new file called `appmesh-injector.yaml` in the **appmesh-system** folder. Add the following as contents to the file:

```yaml
---
apiVersion: helm.fluxcd.io/v1
kind: HelmRelease
metadata:
  name: appmesh-inject
  namespace: appmesh-system
spec:
  releaseName: appmesh-inject
  chart:
    repository: https://aws.github.io/eks-charts/
    name: appmesh-inject
    version: 0.9.0
  values:
    mesh:
      create: true
      name: apps
```

This **HelmRelease** is declaring that we want to create a Helm release for a chart called **appmesh-injector** with a version **0.9.0** that is available in the EKS chart repository **https://aws.github.io/eks-charts/**. The name of the release is **appmesh-injector**. 

We are also setting some **values** within the helm chart. This is equivalent to using a **values.yaml** value, see the [Helm documentation](https://helm.sh/docs/intro/using_helm/#customizing-the-chart-before-installing) for details of values. By setting these values in our HelmRelease we are stating that we want to create a new mesh called **apps**.

You folder structure should look like this now:

```bash
.
├── appmesh-system
│   ├── appmesh-controller.yaml
│   ├── appmesh-inject.yaml
│   └── crds.yaml
├── namespaces
│   └── appmesh-system.yaml
└── README.md
```

Add and then commit the **appmesh-inject.yaml** file and push the the changes to your GitHub repo.

Flux will now see that the desired state of the `appmesh-system` namespace has changed in Git and will apply the CRDs to our cluster. This will take up to 1 minute to apply.

Check that the App Mesh Injector is up & running by running the following command:

```bash
kubectl get pods -n appmesh-system
```

You should see the injector pod in a **Running** state:

```bash
NAME                                  READY   STATUS    RESTARTS   AGE
appmesh-controller-54dd6bdfd8-n8zlq   1/1     Running   0          30m
appmesh-inject-55cdc99595-qm8pt       1/1     Running   0          17s
```
