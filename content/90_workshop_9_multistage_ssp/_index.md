+++
title = "Workshop Module 9: Multi Stage EKS SSP"
chapter = true
weight = 90
+++

# Welcome to Multi Stage EKS Shared Services Platform Workshop

The adoption of Kubernetes by organizations continues to grow at a rapid pace. As enterprises embark on this challenge,  patterns and requirements emerge in order to efficiently operate at scale, as well as guarantee compliance with regulations that increase in number and complexity.

Among these patterns, the reduction of cluster sprawl by consolidating workloads from a large number of single application or single team clusters, to a small number of larger multi-tenant environments is quite common and delivers benefits in terms of management overhead reduction, better utilization of deployed capacity as well as security and compliance simplification.

Together with this consolidation of workloads, the use of hybrid architectures is also becoming a usual requirement in order to satisfy regulations that require organizations to maintain user's data and cluster control with a specific set of geographical or jurisdictional boundaries.

### What you will learn in this module

* How to provision multiple EKS Environments with Terraform and use GitOps to boostrap them.
* How to structure your infrastructure repository to efficiently manage multiple teams.
* How to use Kubernetes RBAC to provide secure cluster multi-tenancy.
* How to use GitOps to promote releases across a hybrid set of independent clusters.

### What is a Shared Services Platform

A shared services platform provides infrastructure and operations teams with a centralized, consolidated management experience to fulfill the requirements of a growing number of application teams, and enables application teams with simplified and expedited access to common services and functionality in a secure and autonomous way.

## GitOps - an operating model for cloud native

GitOps can be summarized as these two things:
* An operating model for Kubernetes and other cloud native technologies, providing a set of best practices that unify deployment, management and monitoring for containerized clusters and applications.
* A path towards a developer experience for managing applications; where end-to-end CICD pipelines and git workflows are applied to both operations, and development. 


![GitOps Operating Model](/images/workshop02_gitops-operating-model.png)

## Weaveworks 
Weaveworks makes it fast and simple for developers and DevOps teams to build and operate powerful containerized applications. We minimize the complexity of operating workloads in Kubernetes by providing automated continuous delivery pipelines, observability and monitoring. 
 
Weaveworks’ mission is to minimize complexities in operating workloads and provide a developer centric operating model for cloud native applications. Our goal is to drive cloud native transformation for your developers and portability, flexibility, choice and stability for your business.
 
One of the first members of the Cloud Native Computing Foundation, Weaveworks also contributes to several open source projects, including [Weave Net](https://www.weave.works/oss/net/), [Weave Scope](https://www.weave.works/oss/scope/), [Weave Cortex](https://www.weave.works/oss/cortex/), [Weave Flux](https://www.weave.works/oss/flux/), [Firekube](https://www.weave.works/oss/firekube/) and [Flagger](https://www.weave.works/oss/flagger/).

## Weaveworks and AWS
Weaveworks is an [APN Advanced Technology Partner](https://aws.amazon.com/partners/find/partnerdetails/?n=Weaveworks&id=001E000001ImwwVIAR) and AWS Containers Competency Partner. In addition [Weaveworks](https://www.weave.works/) is the EKS certified Advanced partner that built [eksctl](https://eksctl.io/). 
 

## Weaveworks Pioneered GitOps
[Learn more about GitOps](https://www.weave.works/technologies/gitops/), a set of modern best practices for deploying and managing applications with cloud native tools and cloud services.

Don’t know where to start? Our experienced team of Customer Reliability Engineers can provide the highest level of care, advice and coaching. Our expertise is rooted in  working and supporting live production systems at scale. Get started with our [EKS + GitOps Quickstart package](https://www.weave.works/eks-gitops-quickstart/). 

### For further reading we recommend:
* [Whitepaper: Implementing a Kubernetes Strategy in your Organization](https://go.weave.works/implementing-kubernetes-strategy-wp.html)
* [Whitepaper: Automating Kubernetes with GitOps](https://go.weave.works/automating-kubernetes-with-gitops-wp.html)
* [Whitepaper: Guide to a product ready Kubernetes cluster ](https://go.weave.works/WP-Production-Ready.html)
* [Checklist: Production Readiness for Kubernetes ](https://go.weave.works/production-ready-kubernetes-checklist.html)
* [Case Study: Fidelity](https://www.weave.works/blog/gitops-driven-fidelity-fideks)
* [Case Study: Mettle](https://www.weave.works/blog/case-study-mettle-leverages-gitops-for-self-service-developer-platform)

### For hands on instructions, we recommend:
[Tutorial: A practical guide to GitOps ](https://go.weave.works/gitops-ebook.html)
