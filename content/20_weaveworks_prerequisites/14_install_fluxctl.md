+++
title = "Install the Flux CLI"
chapter = false
weight = 14

+++

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
