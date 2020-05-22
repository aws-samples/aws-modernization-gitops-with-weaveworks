+++
title = "Create GitHub Repository"
chapter = false
weight = 10
+++

We will be using a GitHub repo to declare the desired state of our EKS cluster. This repo will eventually include your workload manifests, HelmReleases etc.

You can start with an empty repository and push that to GitHub, or use the one you intend to deploy to the cluster. As a suggestion create a blank **public** repo on GitHub called `my-eks-config` and make sure you tick `Initialize this Repository with a README`:

![github_repo_settings](/images/gh_repo_init.png)

For instructions on how to create a repository on GitHub [follow these steps.](https://help.github.com/articles/create-a-repo).

{{% notice info %}}
In the **Cloud9** environment, a user SSH key pair is not automatically created. [Use these instructions](https://help.github.com/en/github/authenticating-to-github/checking-for-existing-ssh-keys) to create an SSH key pair for use as your git repository deploy keys.
{{% /notice %}}

You will also want to add your ssh. Instructions to do so [are here](https://help.github.com/en/github/authenticating-to-github/adding-a-new-ssh-key-to-your-github-account).

The pbcopy will not work on amazon linux, so instead just run:

```bash
cat ~/.ssh/id_rsa.pub
# just copy content to clip manually using mouse/trackpad
```
