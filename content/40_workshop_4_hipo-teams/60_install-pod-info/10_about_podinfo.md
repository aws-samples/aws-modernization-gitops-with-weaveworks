+++
title = "Introducing Podinfo"
chapter = false
weight = 10
+++

Podinfo is a tiny web application made with Go that showcases best practices of running microservices in Kubernetes. It provides a simple web interface that allows you verify the operation of ingresses and/or service meshes. Podinfo was written by Stefan Prodan of Weaveworks, and is freely available on **github.** There are many options, and URLs are available to provide a wealth of information. [More information on Podinfo is available here.](https://github.com/stefanprodan/podinfo)

For our workshop, we will be using `podinfo` to show how to use App Mesh and the observability it gives us.

We will be deploying podinfo in the following logical architecture:

![PodInfo Logical Architecture](/images/podinfo_app_mesh.png)
