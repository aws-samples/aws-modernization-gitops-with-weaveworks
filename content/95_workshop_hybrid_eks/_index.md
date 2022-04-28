+++
title = "Workshop Module 10: Simplified Hybrid EKS"
chapter = true
weight = 95
+++

# Welcome to Hybrid EKS Workshop

GitOps as an operating model together with EKS Distro and Cluster API provide a unique platform that simplifies the deployment of Kubernetes clusters across environments and infrastructure providers. In this Workshop we will look at some of the fundamental processes involved in setting up this architecture.

### What you will learn in this module

* Bootstrapping a cluster with Weave Flux, Weave GitOps and Cluster API.
* Create a new Cluster on EC2 using Cluster API.
* Upgrade your cluster to a new Kubernetes version using GitOps.


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
