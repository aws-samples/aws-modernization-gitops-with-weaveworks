---
title: "Introducing Podinfo"
chapter: false
weight: 20
---
Podinfo is a tiny web application made with Go that showcases best practices of running microservices in Kubernetes. It provides a simple web interface that allows you verify the operation of ingresses and/or service meshes. Podinfo was written by Stefan Prodan of Weaveworks, and is freely available on **github.** There are many options, and URLs are available to provide a wealth of information. [More information on Podinfo is available here.](https://github.com/stefanprodan/podinfo)

For our workshop, we will be using `podinfo` to show how load balancing and high availability can be simply implemented with ingresses and Kubernetes.

Our configuration will enable internal Kubernetes load balancing and scaling. Optionally, AWS **Route 53** can be used to balance between your actual clusters in different regions.
