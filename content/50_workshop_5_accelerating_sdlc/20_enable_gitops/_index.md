+++
title = "Enable GitOps"
chapter = true
weight = 20
+++

## Create Workshop Git Repository

A GitOps workflow requires several components, including a Git repository that acts as the source of truth for your Kubernetes resources.

We will be using [`eksctl enable repo`](https://eksctl.io/usage/experimental/gitops/) on our newly-built cluster to install GitOps operator components.
