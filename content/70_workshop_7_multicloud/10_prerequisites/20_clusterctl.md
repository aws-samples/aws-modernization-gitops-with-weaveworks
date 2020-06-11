+++
title = "Install clusterctl"
chapter = false
weight = 20
+++

The CluserAPI (CAPI) is a project of SIG Cluster Lifecycle to bring declarative, Kubernetes-style APIs to cluster creation, configuration, and management.

[cluserctl](https://cluster-api.sigs.k8s.io/) is the cli that we'll use to bootstrap the CAPI controllers.

```sh
curl -L https://github.com/kubernetes-sigs/cluster-api/releases/download/v0.3.6/clusterctl-linux-amd64 -o clusterctl
chmod +x ./clusterctl
sudo mv ./clusterctl /usr/local/bin/clusterctl
```

Validate that it's working

```sh
clusterctl version
```