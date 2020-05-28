+++
title = "Eksctl Enable Repo"
chapter = false
weight = 202
+++

Ensure you are in the root of your Cloud9 Environment and clone your repository.

```sh
cd ~/environment

export GH_USER=YOUR_GITHUB_USERNAME
```

```sh
export GH_REPO=sdlc-workshop
```

```sh
git clone git@github.com:${GH_USER}/${GH_REPO}
cd ${GH_REPO}
```

Next, we will be running running `eksctl enable repo`. This takes an existing EKS cluster and an empty repository and sets up a GitOps pipeline.

```sh
export EKSCTL_EXPERIMENTAL=true

eksctl enable repo \
--cluster=gitopssdlcworkshop \
--region=us-west-2 \
--git-user="fluxcd" \
--git-email="${GH_USER}@users.noreply.github.com" \
--git-url="git@github.com:${GH_USER}/${GH_REPO}"
```

`eksctl enable repo` will install FluxCD and the Helm Operator (with support for Helm v3).

When the command finishes, you will be prompted to add Flux's SSH public key to your GitHub repository.

Copy the public key and create a deploy key with write access to your GitHub repository.

You can add a deploy key to your repository by going to: `Settings > Deploy keys`. Click on `Add deploy key`, and check `Allow write access`. Then paste the Flux public key and click `Add key`.

Once you have added the deploy key, Flux will pick up the changes in the repository and deploy them to the cluster.
