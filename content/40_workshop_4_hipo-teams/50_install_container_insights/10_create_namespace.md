+++
title = "Create the namespace"
chapter = false
weight = 10
+++

Create a file within the **namespaces** that folder called `amazon-cloudwatch.yaml`.

Paste the contents of the following into the new file:

```yaml
---
apiVersion: v1
kind: Namespace
metadata:
  labels:
    name: amazon-cloudwatch
  name: amazon-cloudwatch
```

Your repositoiry structure should be:

```bash
.
├── appmesh-system
│   ├── appmesh-controller.yaml
│   ├── appmesh-inject.yaml
│   ├── appmesh-prometheus.yaml
│   └── crds.yaml
├── namespaces
│   ├── amazon-cloudwatch.yaml
│   └── appmesh-system.yaml
└── README.md
```

Add and then commit the **amazon-cloudwatch.yaml** file and push the the changes to your GitHub repo.

Flux will now see that the desired state has changed in Git and will apply the namespace to our cluster. This will take up to 1 minute to apply.

Check that that the namespace has been created by running the following command:

```bash
kubectl get ns
```

You should see the **amazon-cloudwatch** namespace listed. For example:

```bash
NAME              STATUS   AGE
amazon-cloudwatch   Active   5s
appmesh-system      Active   3h2m
default             Active   4h18m
flux                Active   3h4m
kube-node-lease     Active   4h18m
kube-public         Active   4h18m
kube-system         Active   4h18m
```

If you don't see it listed given a few minutes longer and try again.
