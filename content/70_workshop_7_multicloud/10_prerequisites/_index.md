+++
title = "Prerequisites for Workshop"
chapter = true
weight = 20
+++

# Setup All the Required Tools & Permissions

To complete these workshops, you will first need to install eksctl. Instructions for doing this can be found at [eksctl.io](https://eksctl.io/introduction/#installation). eksctl (pronounced "eks control") is a simple CLI tool for creating clusters on EKS. It is written in Go, uses CloudFormation, was created by [Weaveworks](https://weave.works) and it welcomes contributions from the community.

{{% notice warning %}}
If you are using your own AWS account, you will need permissions to create EKS clusters plus admin rights within your EKS cluster to configure configuration rules and install agents. Ensure you have authority within your organization to do this in your tenant.
{{% /notice %}}

