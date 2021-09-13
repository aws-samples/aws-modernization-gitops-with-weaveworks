+++
title = "Workshop Prerequisites"
chapter = true
weight = 1
+++

# Prerequisites

In order to run through this workshop you will need to configure your Cloud9 environment with some tools and permissions.

## Configure Cloud9 permissions

* [Create your Cloud9 Development Environment](/20_weaveworks_prerequisites/10_workspace.html).
* [Create a Role for your Cloud9 Environment](/20_weaveworks_prerequisites/17_create_iam_policy_and_role.html)
* [Attach the Role to your Cloud9 Instance](/20_weaveworks_prerequisites/18_attach_the_role.html)
* [Update the settings of your Cloud9 IDE to use the role](/20_weaveworks_prerequisites/50_workspaceiam.html)

## Install the necessary tools

We have created a handy script that will install all the tools you need for this workshop and have included it as part of the repository you'll be using the spin up your infrastructure.

First, clone the repository:

```shell
git clone https://github.com/weaveworks-gitops-demo/multistage-ssp
```

Now, run the `start.sh` script in the root of the repo and you'll be good to go:

```shell
cd multistage-ssp
./start.sh
```

The command will install:

- kubectl
- wego
- terraform
- aws-iam-authenticator
- jq

As well as create an SSH key-pair and store github.com's SSH identity in a local `known_hosts` file. These will be used to grant access to the clusters to your SSP repository.