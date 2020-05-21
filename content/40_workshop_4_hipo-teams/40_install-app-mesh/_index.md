+++
title = "Install App Mesh"
chapter = true
weight = 40
+++

# Install App Mesh

In this section we will be deploying components to our EKS cluster that will enable integration with [AWS App Mesh](https://aws.amazon.com/app-mesh/).

AWS App Mesh is a service mesh that provides application-level networking. It allows services to communicate across different types of compute infrastructure (e.g. EKS, EC2). It also provides end-to-end visibility of how your services communicate with each other.

App Mesh is made up of quite a few components including:

* **Service mesh** – A service mesh is a logical boundary for network traffic between the services that reside within it
* **Virtual services** – A virtual service is an abstraction of an actual service that is provided by a virtual node, directly or indirectly, by means of a virtual router.
* **Virtual nodes** – A virtual node acts as a logical pointer to a discoverable service, such as an Amazon ECS or Kubernetes service. For each virtual service, you will have at least one virtual node.
* **Virtual routers and routes** – Virtual routers handle traffic for one or more virtual services within your mesh. A route is associated to a virtual router. The route is used to match requests for the virtual router and to distribute traffic to its associated virtual nodes

For more information see the [AWS App Mesh Docs](https://docs.aws.amazon.com/app-mesh/latest/userguide/what-is-app-mesh.html).
