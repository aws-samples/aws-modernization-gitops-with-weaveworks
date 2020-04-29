+++
title = "GitOps for EKS Prerequisites"
chapter = true
weight = 20
+++

# eksctl Setup

To complete the workshop, you first need to install eksctl. Instructions for doing this can be found at [eksctl.io](https://eksctl.io/introduction/#installation). eksctl (pronounced "eks control") is a simple CLI tool for creating clusters on EKS. It is written in Go, uses CloudFormation, was created by [Weaveworks](https://weave.works) and it welcomes contributions from the community. 

{{% notice warning %}}
You will need admin rights within your EKS cluster to configure configuration rules and install agents. Ensure you have authority within your organization to do this in your tenant. 
{{% /notice %}}

