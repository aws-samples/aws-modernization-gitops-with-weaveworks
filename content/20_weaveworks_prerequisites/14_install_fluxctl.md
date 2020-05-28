+++
title = "Install fluxctl"
chapter = false
weight = 14

+++

## Install Fluxctl

[Fluxctl](https://docs.fluxcd.io/en/1.17.1/references/fluxctl.html) is a CLI tool that is able to talk to [Weave Flux](https://github.com/fluxcd/flux).

Install by running this command:

```sh
curl -sL https://fluxcd.io/install | sh
export PATH="$PATH:$HOME/.fluxcd/bin"
```

Verify the installation:

```sh
fluxctl version
```
