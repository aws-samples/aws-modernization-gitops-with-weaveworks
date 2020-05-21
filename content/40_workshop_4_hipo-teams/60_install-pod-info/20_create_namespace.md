+++
title = "Create the namespace"
chapter = false
weight = 20
+++

All the **PodInfo** components will be live in the `apps` namespace in our EKS cluster. We will be managing all namespaces via GitOps. This includes the actual creation of the namespace and also the resources contained with it.

Create a file within the **namespaces** folder called `apps.yaml`.

Paste the contents of the following into the new file:

```yaml
---
apiVersion: v1
kind: Namespace
metadata:
  labels:
    name: apps
    appmesh.k8s.aws/sidecarInjectorWebhook: enabled
  name: apps
```

{{% notice note %}}
Notice that the namespace has included a label called `appmesh.k8s.aws/sidecarInjectorWebhook` with a value of **enabled**. This tells the App Mesh Injector that pods created in this namespace should have the App Mesh sidecar automatically injected into them.
{{% /notice %}}

Your repository structure should be:

```bash
.
├── amazon-cloudwatch
│   ├── cwagent-fluentd-quickstart.yaml
│   └── cwagent-prometheus-eks.yaml
├── appmesh-system
│   ├── appmesh-controller.yaml
│   ├── appmesh-inject.yaml
│   ├── appmesh-prometheus.yaml
│   └── crds.yaml
├── namespaces
│   ├── amazon-cloudwatch.yaml
│   ├── appmesh-system.yaml
│   └── apps.yaml
└── README.md
```

Add and then commit the **apps.yaml** file and push the the changes to your GitHub repo.

Flux will now see that the desired state has changed in Git and will apply the namespace to our cluster. This will take up to 1 minute to apply.

Check that that the namespace has been created by running the following command:

```bash
kubectl get ns
```

You should see the **apps** namespace listed. For example:

```bash
NAME                STATUS   AGE
amazon-cloudwatch   Active   25m
appmesh-system      Active   25m
apps                Active   1s
default             Active   84m
flux                Active   33m
kube-node-lease     Active   84m
kube-public         Active   84m
kube-system         Active   84m
```

If you don't see it listed given a few minutes longer and try again.
