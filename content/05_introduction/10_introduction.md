+++
title = "Why EKS?"
chapter = false
weight = 10
+++

## Modern cloud environments need a different approach to observability

Conventional application performance monitoring (APM) emerged when software was mostly monolithic
and update cycles were measured in years, not days. Manual instrumentation and performance baselining, though cumbersome, were once adequate—particularly since fault patterns were generally known and well understood.

As monoliths get replaced by cloud-native applications, that are rapidly growing in size, traditional monitoring approaches are no longer enough. Rather than instrumenting for a predefined set of problems, enterprises need complete visibility into every single component of these dynamically scaling microservice environments. This includes multi-cloud infrastructures, container orchestration systems like Kubernetes, service meshes, functions-as-a-service and polyglot container payloads.

Such applications are more complex and unpredictable than ever. System health problems are rarely well understood from the outset and IT teams spend a significant amount of time manually solving problems and putting out fires after the fact. The challenge with modern cloud environments is to address the unknown unknowns—the kind of unique glitches that have never occurred in the past. These are the growing pains that the concept of observability attempts to tackle.

## Kubernetes

Kubernetes is open source software that allows you to deploy and manage containerized applications at scale. 

Kubernetes manages clusters of compute instances and runs containers on those instances with processes for deployment, maintenance, and scaling. Using Kubernetes, you can run any type of containerized applications using the same toolset on-premises and in the cloud.  

You can read more about Kubernetes [here](https://aws.amazon.com/kubernetes/)

## Amazon Elastic Kubernetes Service

AWS makes it easy to run Kubernetes. You can choose to manage Kubernetes infrastructure yourself with Amazon EC2 or get an automatically provisioned, managed Kubernetes control plane with [Amazon EKS](https://aws.amazon.com/eks/). Either way, you get powerful, community-backed integrations to AWS services like VPC, IAM, and service discovery as well as the security, scalability, and high-availability of AWS.

Amazon EKS runs Kubernetes control plane instances across multiple Availability Zones to ensure high availability. Amazon EKS automatically detects and replaces unhealthy control plane instances, and it provides automated version upgrades and patching for them.

Amazon EKS is also integrated with many AWS services to provide scalability and security for your applications, including the following:

* Elastic Load Balancing for load distribution
* IAM for authentication
* Amazon VPC for isolation
* Amazon ECR for container images

For this workshop you are going to use Amazon EKS managed service that makes it easy for you to run Kubernetes on AWS without needing to stand up or maintain your own Kubernetes control plane. 

{{% children showhidden="false" %}}
