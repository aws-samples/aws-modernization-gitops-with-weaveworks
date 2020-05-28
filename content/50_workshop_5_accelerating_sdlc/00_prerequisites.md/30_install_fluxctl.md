+++
title = "Install fluxctl"
chapter = false
weight = 30

+++

We will be using [Fluxctl](https://docs.fluxcd.io/en/1.17.1/references/fluxctl.html), a CLI tool that is able to talk to [Weave Flux](https://github.com/fluxcd/flux) to immediately apply and deploy changes we make to our repository.

```sh
curl -sL https://fluxcd.io/install | sh
export PATH="$PATH:$HOME/.fluxcd/bin"
```

Verify the installation:

```sh
fluxctl version
```
