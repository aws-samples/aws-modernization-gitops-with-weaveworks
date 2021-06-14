+++
title = "Doing GitOps with Flux v2"
chapter = true
weight = 30
+++

## GitOps and Flux v2

[gitops](https://www.weave.works/technologies/gitops/) is a way to do Kubernetes application delivery and cluster management. It works by using Git as a single source of truth for Kubernetes resources and everything else.

With Git at the center of your delivery pipelines, you and your team can make pull requests to accelerate and simplify application deployments and operations tasks to Kubernetes.

Flux v2 runs in your EKS cluster and tracks changes to one or more Git repositories. These repositories store all manifests that define the desired state of your cluster, Flux will continuously and automatically reconcile the running state of the cluster with the state declared in code.
