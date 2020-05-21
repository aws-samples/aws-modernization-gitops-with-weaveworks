+++
title = "Create the namespace"
chapter = false
weight = 10
+++

All the AWS App Mesh components will be live in the `appmesh-system` namespace in our EKS cluster. We will be managing all namespaces via GitOps. This includes the actual creation of the namespace and also the resources contained with it.

Ensure you have cloned your repository into your **Cloud9** environment.

```bash
# Make sure you are in your environment directory
cd ~/environment

# Clone your repo
git clone  git@github.com:yourname/my-eks-config.git

# Change into the repos directory
cd my-eks-config
```

Create a folder in your repo called `namespaces` and then create a file within that folder called `appmesh-system.yaml`.

Paste the contents of the following into the new file:

```yaml
---
apiVersion: v1
kind: Namespace
metadata:
  labels:
    name: appmesh-system
  name: appmesh-system
```

Your repository structure should be:

```bash
.
├── namespaces
│   └── appmesh-system.yaml
└── README.md
```

Add and then commit the **appmesh-system.yaml** file and push the the changes to your GitHub repo.

Flux will now see that the desired state has changed in Git and will apply the namespace to our cluster. This will take up to 1 minute to apply.

Check that that the namespace has been created by running the following command:

```bash
kubectl get ns
```

You should see the **appmesh-system** namespace listed. For example:

```bash
NAME              STATUS   AGE
appmesh-system    Active   26s
default           Active   76m
flux              Active   2m18s
kube-node-lease   Active   76m
kube-public       Active   76m
kube-system       Active   76m
```

If you don't see it listed, give it a few minutes longer and try again.
