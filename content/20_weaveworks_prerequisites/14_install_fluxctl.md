+++
title = "Install fluxctl"
chapter = false
weight = 14

+++

## Install Fluxctl

[Fluxctl](https://docs.fluxcd.io/en/1.17.1/references/fluxctl.html) is a CLI tool that is able to talk to [Weave Flux](https://github.com/fluxcd/flux).

Install by running this command:

```sh
curl -Ls https://fluxcd.io/install | sh && \
sudo mv $HOME/.fluxcd/bin/fluxctl /usr/local/bin/fluxctl
```

Verify the installation:

```sh
fluxctl version
```
