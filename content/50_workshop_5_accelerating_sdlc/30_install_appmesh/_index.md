+++
title = "Install App Mesh"
chapter = true
weight = 30
+++

## Install App Mesh

In this section, we will be using an experimental feature of `eksctl` to install App Mesh.

The [eksctl appmesh profile](https://github.com/weaveworks/eks-appmesh-profile) will deploy the App Mesh Kubernetes components along with monitoring and progressive delivery tooling on an EKS cluster.

Components that this profile will install include:

* Kubernetes custom resources: mesh, virtual nodes and virtual services
* CRD controller: keeps the custom resources in sync with the App Mesh control plane
* Admission controller: injects the Envoy sidecar and assigns pods to App Mesh virtual nodes
* Telemetry service: Prometheus instance that collects and stores Envoy's metrics
* Progressive delivery operator: [Flagger](https://flagger.app) instance that automates canary releases on top of App Mesh
