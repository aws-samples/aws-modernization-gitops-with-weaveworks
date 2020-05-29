+++
title = "Install fluxctl"
chapter = false
weight = 30

+++

We will be using [Fluxctl](https://docs.fluxcd.io/en/1.17.1/references/fluxctl.html), a CLI tool that is able to talk to [Weave Flux](https://github.com/fluxcd/flux) to immediately apply and deploy changes we make to our repository.

```sh
curl -Ls https://fluxcd.io/install | sh && \
sudo mv $HOME/.fluxcd/bin/fluxctl /usr/local/bin/fluxctl
```

Verify the installation:

```sh
fluxctl version
```
