+++
title = "Install Required CLIs"
chapter = false
weight = 14
+++

# Install the required CLIs

Throughout the workshops you may need one of the following two products, both built by Weaveworks.

## Install Fluxctl

[Flux CLI](https://fluxcd.io/docs/installation/) is a CLI tool that is able to talk to [Weave Flux](https://github.com/fluxcd/flux2).

Install by running this command:

```sh
curl -s https://fluxcd.io/install.sh | sudo bash
```

Verify the installation:

```sh
flux -v
```

## Install Weave GitOps

Weave GitOps Core is the latest product from Weaveworks, and enables a streamlined and effective GitOps Workflow.

```sh
curl -L https://github.com/weaveworks/weave-gitops/releases/download/v0.2.1/wego-$(uname)-$(uname -m) -o wego
chmod +x wego
sudo mv ./wego /usr/local/bin/wego
```

Verify the installation:

```sh
wego version
```