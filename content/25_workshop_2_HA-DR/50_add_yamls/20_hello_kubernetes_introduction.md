+++
title = "Introducing Hello Kubernetes"
chapter = false
weight = 20
+++

`Hello Kubernetes` is a very simple web application created by Paul Bouwer, open source and [available in GitHub](https://github.com/paulbouwer/hello-kubernetes).

For our workshop, we will be using `Hello Kubernetes` to quickly demonstrate how load balancing and high availability can be simply implemented with ingresses and Kubernetes.

Our configuration will enable internal Kubernetes load balancing and scaling. Optionally, AWS **Route 53** can be used to balance between your actual clusters in different regions.
