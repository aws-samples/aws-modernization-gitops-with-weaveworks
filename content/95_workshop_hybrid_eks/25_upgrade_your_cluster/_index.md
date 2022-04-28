+++
title = "Upgrade your Cluster"
chapter = true
weight = 25
+++

# Upgrade your Cluster

## Modify your cluster manifest to a newer K8s version

Open `capi-quickstart.yaml` and look for the two references to `version: v1.21.5`, change those to `version: v1.22.6`

## Push the change to your repository

```shell
git commit -am "Moving version of cluster to 1.22.6"
git push
```

## Watch as the cluster upgrades

```shell
clusterctl describe cluster capi-quickstart
```