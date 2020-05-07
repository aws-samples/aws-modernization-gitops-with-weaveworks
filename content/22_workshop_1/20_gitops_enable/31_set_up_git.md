---
title: "Set Up Your GIT Repository"
chapter: false
weight: 31
---

The most important ingredient using eksctl enable repo is the configuration of your repository (which will include your workload manifests, HelmReleases, etc). You can start with an empty repository and push that to Git, or use the one you intend to deploy to the cluster.

The main point of GitOps is to keep everything (config, alerts, dashboards, apps, literally everything) in Git and use it as a single source of truth. To keep your cluster configuration in Git, please go ahead and create an empty repository. On GitHub, for example, [follow these steps.](https://help.github.com/articles/create-a-repo)

