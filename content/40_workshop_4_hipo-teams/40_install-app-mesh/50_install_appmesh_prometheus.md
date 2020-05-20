+++
title = "Install Prometheus for App Mesh"
chapter = false
weight = 50
+++

We also want to collect the metrics that App Mesh publishes and so we will also deploy Prometheus. We can use the metrics to create alerts or to help enable progressive delivery (covered in a later workshop).

{{% notice tip %}}
We could also use Container Insights, see the [documentation](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/deploy-container-insights-EKS.html).
{{% /notice %}}

Prometheus for App Mesh is also available as a Helm package from the [EKS Chart Repository](https://aws.github.io/eks-charts/). We will use the **Helm Operator** along with this chart to install the injector.

Create a new file called `appmesh-prometheus.yaml` in the **appmesh-system** folder. Add the following as contents to the file:

```yaml
---
---
apiVersion: helm.fluxcd.io/v1
kind: HelmRelease
metadata:
  name: appmesh-prometheus
  namespace: appmesh-system
spec:
  releaseName: appmesh-prometheus
  chart:
    repository: https://aws.github.io/eks-charts/
    name: appmesh-prometheus
    version: 0.3.0
```

Your folder structure should look like this now:

```bash
.
├── appmesh-system
│   ├── appmesh-controller.yaml
│   ├── appmesh-inject.yaml
│   ├── appmesh-prometheus.yaml
│   └── crds.yaml
├── namespaces
│   └── appmesh-system.yaml
└── README.md
```

Add and then commit the **appmesh-prometheus.yaml** file and push the the changes to your GitHub repo.

Flux will now see that the desired state of the `appmesh-system` namespace has changed in Git and will apply the CRDs to our cluster. This will take up to 1 minute to apply.

Check that Prometheus is up & running by running the following command:

```bash
kubectl get pods -n appmesh-system
```

You should see the prometheus pod in a **Running** state:

```bash
appmesh-controller-54dd6bdfd8-n8zlq   1/1     Running   0          45m
appmesh-inject-55cdc99595-qm8pt       1/1     Running   0          15m
appmesh-prometheus-5cb5d88d6-pbg6w    1/1     Running   0          64s
```
