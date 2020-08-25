+++
title = "Create EKS cluster"
chapter = false
weight = 15
+++

# Create EKS Cluster

- In a seperate terminal, run

```sh
eksctl create cluster --version 1.17 --nodes 1 -t t3.large my-management-cluster
```

This will save us time while we go through next steps