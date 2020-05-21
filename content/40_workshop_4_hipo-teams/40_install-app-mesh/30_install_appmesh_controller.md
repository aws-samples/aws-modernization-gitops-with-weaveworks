+++
title = "Install App Mesh Controller"
chapter = false
weight = 30
+++

Now the CRDs have been installed we can install the controller that understands them.

The AWS App Mesh Controller is available as a Helm package from the [EKS Chart Repository](https://aws.github.io/eks-charts/). We can use the **Helm Operator** along with this chart to install the controller.

To install a chart using the [Helm Operator](https://docs.fluxcd.io/projects/helm-operator/en/latest/) we need to define a **HelmRelease**. A HelmRelease is a way to declare the desired state of a Helm release. The HelmRelease can be applied to a cluster using GitOps.

Create a new file called `appmesh-controller.yaml` in the **appmesh-system** folder. Add the following as contents to the file:

```yaml
---
apiVersion: helm.fluxcd.io/v1
kind: HelmRelease
metadata:
  name: appmesh-controller
  namespace: appmesh-system
spec:
  releaseName: appmesh-controller
  chart:
    repository: https://aws.github.io/eks-charts/
    name: appmesh-controller
    version: 0.6.1
```

This **HelmRelease** is declaring that we want to create a Helm release for a chart called **appmesh-controller** with a version **0.6.1** that is available in the EKS chart repository **https://aws.github.io/eks-charts/**. The name of the release is **appmesh-controller**. 

{{% notice tip %}}
For full details on the elements of the **HelmRelease** please head to the [documentation](https://docs.fluxcd.io/projects/helm-operator/en/latest/helmrelease-guide/introduction).
{{% /notice %}}

You folder structure should look like this now:

```bash
.
├── appmesh-system
│   ├── appmesh-controller.yaml
│   └── crds.yaml
├── namespaces
│   └── appmesh-system.yaml
└── README.md
```

Add and then commit the **appmesh-controller.yaml** file and push the the changes to your GitHub repo.

Flux will now see that the desired state of the `appmesh-system` namespace has changed in Git and will apply the CRDs to our cluster. This will take up to 1 minute to apply.

Check that the App Mesh Controller is up & running by running the following command:

```bash
kubectl get pods -n appmesh-system
```

You should see the controller pod in a **Running** state:

```bash
NAME                                  READY   STATUS    RESTARTS   AGE
appmesh-controller-54dd6bdfd8-n8zlq   1/1     Running   0          43s
```

