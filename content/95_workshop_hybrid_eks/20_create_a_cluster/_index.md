+++
title = "Create a Cluster"
chapter = true
weight = 20
+++

# Creating a Cluster

## Create the Cluster Manifests


```shell
export AWS_REGION=us-east-1
export AWS_SSH_KEY_NAME=default

# Select instance types
export AWS_CONTROL_PLANE_MACHINE_TYPE=t3.large
export AWS_NODE_MACHINE_TYPE=t3.large

clusterctl generate cluster capi-quickstart --kubernetes-version v1.21.5 --control-plane-machine-count=1 --worker-machine-count=1 > ./clusters/workshop/capi-quickstart.yaml
```

## Modify the Cluster Configuration to use a single AZ

In order to reduce workshop resource utilization we will limit the number of AZs to use to one:

```yaml
---
apiVersion: infrastructure.cluster.x-k8s.io/v1beta1
kind: AWSCluster
metadata:
  name: capi-quickstart
  namespace: default
spec:
  region: us-east-1
  sshKeyName: lmurillo-workshop
  network:
    vpc:
      availabilityZoneUsageLimit: 1
      availabilityZoneSelection: Random
```

## Commit the new cluster

```shell
git commit -am "Adding our first cluster"
git push
```

## Watch as the cluster is reconciled and created

```shell
clusterctl describe cluster capi-quickstart
```

## Get the cluster's KubeConfig

Verify the Control Plane is up

```shell
kubectl get kubeadmcontrolplane
```

Once ready, get the kubeconfig

```shell
clusterctl get kubeconfig capi-quickstart > ~/capi-quickstart.kubeconfig
```

## Deploy a CNI to the cluster

```bash
kubectl --kubeconfig=~/capi-quickstart.kubeconfig \
  apply -f https://docs.projectcalico.org/v3.21/manifests/calico.yaml
```

## Confirm the cluster nodes are up and running

```bash
kubectl --kubeconfig=~/capi-quickstart.kubeconfig get nodes
```