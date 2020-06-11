+++
title = "Workshop 7: Multicloud"
chapter = true
weight = 70
+++

# Welcome to Cluster Management using Operators

Many modern organizations use Machine Learning and also Kubernetes. In this workshop we will demonstrate how to use eksctl and gitops to create an AWS EKS cluster and use gitops to deploy and application development profile. Then we will deploy Kubeflow and use it to execute a Machine learning sample.

In this workshop we will learn:

* Setup Flux and Helm Operator
* Install Cluster API components with Flux
* Bootstrap and manage EC2 clusters declaratively with GitOps
* Write and ship a custom operator
* Sync applications across clusters

<!-- * How to turn a complex set of components into a profile
* How profiles allow for reliable and automated deployments to EKS
* Provision and manage a ML stack with Kubeflow
* Install a sample app using AWS Elastic MapReduce (EMR)
* How GitOps managed profiles enable portability for workloads across different clouds, on-premise and your laptop -->

## GitOps - An operating model for cloud native

![GitOps Operating Model](/images/workshop02_gitops-operating-model.png)

For further reading we recommend:

* [A guide to GitOps](https://www.weave.works/technologies/gitops/)
* [FAQs on GitOps](https://www.weave.works/technologies/gitops-frequently-asked-questions/)
* Blog Series:
 * [Operations by Pull Request](https://www.weave.works/blog/gitops-operations-by-pull-request)
 * [The GitOps Pipeline](https://www.weave.works/blog/the-gitops-pipeline)
 * [GitOps Observability](https://www.weave.works/blog/gitops-part-3-observability)
 * [Compliance and Secure CICD](https://www.weave.works/blog/gitops-compliance-and-secure-cicd)
* [Case Study: Fidelity](https://www.weave.works/blog/gitops-driven-fidelity-fideks)
