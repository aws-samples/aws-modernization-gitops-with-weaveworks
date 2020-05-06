---
title: "Enable Your Cluster for GitOps"
chapter: false
weight: 20
---

The following command will set up your cluster with:

- Flux,
- Flux Helm Operator with Helm v3 support,

and add their manifests to Git, so you can configure them through pull requests.

Run this command from any directory in your file system. eksctl will clone your repository in a temporary directory that will be removed later.

```
EKSCTL_EXPERIMENTAL=true \
    eksctl enable repo \
        --git-url git@github.com:example/my-eks-config \
        --git-email your@email.com \
        --cluster your-cluster-name \
        --region your-cluster-region
```

Let us go through the specified arguments one by one:

- --git-url: this points to a Git URL where the configuration for your cluster will be stored. This will contain config for the workloads and infrastructure later on.
- --git-email: the email used to commit changes to your config repository.
- --cluster: the name of your cluster. Use `eksctl get cluster` to see all clusters in your default region.
- --region: the region of your cluster.

There are more arguments and options, please refer to the [GitOps reference of eksctl](https://eksctl.io/usage/experimental/gitops/) which details all the flags and resulting directory structure.
