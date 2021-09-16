+++
title = "Create your SSP Repository"
chapter = true
weight = 10
+++

# The SSP Repository

Your Shared Services Platform repo will be the source of your desired state for all core services, as well as where you will declare the various teams you want to onboard on your platform.

This repository is usually owned and managed by Platform Operation or Infrastucture Teams.

Application Development Teams will not require access rights to this repository at all, since we will use this repo to onboard each App Team's own repositories within their own namespace.

## Create a repository in your GitHub Account

Follow the [official GitHub instructions](https://docs.github.com/en/get-started/quickstart/create-a-repo) on how to create a Git repository under your personal account. We will be using the name `team-ssp` for repository going forward, but you can name your repository anything you want.

To simplify some of the tasks going forward, and considering we will not be storing any confidential or sensitive information during this working on your repository, it's easiest to create a public repository.

Also, make sure to have GitHub automatically add a README.md to your repo, just so you will not need to initiliaze it.

## Create a personal access token (PAT)

The terraform provisioning process will automatically create an SSH key pair for your clusters to be able to fetch their desired state from your GitHub Repository. In order for terraform to be able to add this deploy key, it will need to authenticate using a Personal Access Token.

Follow the official GitHub Instructions on how to [create a Personal Access Token](https://docs.github.com/en/github/authenticating-to-github/keeping-your-account-and-data-secure/creating-a-personal-access-token). Make sure to grant `repo` scope to your token.

Note that you will only be able to see this token once at the time of creation, make sure to copy it and keep it safe. You should delete the token at the end of this workshop.

## Export your PAT in your Cloud9 Environment

Take your token and export it to your Cloud9 Environment:

```shell
export GITHUB_TOKEN={value of your token}
```

## Clone the repository to your Cloud9 Environment

Now let's clone the repository to your Cloud9 Environment! We are ready to start structuring our repo so that we'll be able to bootstrap our clusters with core components, as well as onboard a couple of sample teams.

```shell
git clone https://github.com/{your username}/team-ssp
```