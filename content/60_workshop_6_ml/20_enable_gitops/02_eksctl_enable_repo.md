+++
title = "Eksctl Enable Repo"
chapter = false
weight = 202
+++

Ensure you are in the root of your Cloud9 Environment and clone your repository.

```sh
GITHUB_DIR=$PWD/src/github.com
GIT_EMAIL=$(git config -f ~/.gitconfig --get user.email)
GIT_ORG=$(git config -f ~/.gitconfig --get user.name)

EKSCTL_EXPERIMENTAL=true \
    eksctl enable repo \
        --git-url git@github.com:$GIT_ORG/${EKS_CLUSTER_NAME}-config \
        --git-email $GIT_EMAIL \
        --cluster $EKS_CLUSTER_NAME \
        --region "$AWS_DEFAULT_REGION"
```

`eksctl enable repo` will install FluxCD and the Helm Operator (with support for Helm v3).

When the command finishes, you will be prompted to add Flux's SSH public key to your GitHub repository.

Copy the public key and create a deploy key with write access to your GitHub repository.

You can add a deploy key to your repository by going to: `Settings > Deploy keys`. Click on `Add deploy key`, and check `Allow write access`. Then paste the Flux public key and click `Add key`.

Once you have added the deploy key, Flux will pick up the changes in the repository and deploy them to the cluster.
