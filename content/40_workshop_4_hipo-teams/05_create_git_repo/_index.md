+++
title = "Set Up Your GIT Repository"
chapter = true
weight = 05
+++

The most important ingredient using eksctl enable repo is the configuration of your repository (which will include your workload manifests, HelmReleases, etc). You can start with an empty repository and push that to Git, or use the one you intend to deploy to the cluster.

The main point of GitOps is to keep everything (config, alerts, dashboards, apps, literally everything) in Git and use it as a single source of truth. To keep your cluster configuration in Git, please go ahead and create an empty repository. On GitHub, for example, [follow these steps.](https://help.github.com/articles/create-a-repo)

{{% notice info %}}
In the **Cloud9** environment, a user SSH key pair is not automatically created. [Use these instructions](https://help.github.com/en/github/authenticating-to-github/checking-for-existing-ssh-keys) to create an SSH key pair for use as your git repository deploy keys.
{{% /notice %}}

You will also want to enable the software agent in your cluster (flux) to write to your GitOps git repository by installing a valid SSH key as a **Deploy Key**. Instructions to do so [are here](https://help.github.com/en/github/authenticating-to-github/adding-a-new-ssh-key-to-your-github-account).
