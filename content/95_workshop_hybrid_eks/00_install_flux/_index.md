+++
title = "Install Flux and Bootstrap your Cluster"
chapter = true
weight = 1
+++

# Install Flux and Bootstrap your Cluster

## Create a GitHub Personal Access Token

Follow the official GitHub Instructions on how to [create a Personal Access Token](https://docs.github.com/en/github/authenticating-to-github/keeping-your-account-and-data-secure/creating-a-personal-access-token). Make sure to grant `repo` scope to your token.

Note that you will only be able to see this token once at the time of creation, make sure to copy it and keep it safe. You should delete the token at the end of this workshop.

## Install Flux

```shell
curl -s https://fluxcd.io/install.sh | sudo bash
```

## Export your PAT to the environment

```shell
export GITHUB_TOKEN={your PAT}
```

## Make sure you can talk to your EKS cluster

```shell
kubectl get nodes
```

## Bootstrap your cluster

```shell
flux bootstrap github --owner={your GitHub Username} --repository=workshop-devops --path=clusters/workshop --personal --private=false
```

## Clone the Repo created to your Cloud9 Instance

```shell
git clone https://github.com/{your GitHub Username}/workshop-devops
cd workshop-devops
```