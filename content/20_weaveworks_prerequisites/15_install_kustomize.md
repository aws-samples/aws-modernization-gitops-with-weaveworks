+++
title = "Install kustomize"
chapter = false
weight = 15

+++

## Install Kustomize

[Kustomize](https://github.com/kubernetes-sigs/kustomize) lets you customize raw, template-free YAML files for multiple purposes, leaving the original YAML untouched and usable as is.

Install kustomize for Linux:

```sh
curl --silent --location --remote-name \
"https://github.com/kubernetes-sigs/kustomize/releases/download/kustomize/v3.2.3/kustomize_kustomize.v3.2.3_linux_amd64" && \
chmod a+x kustomize_kustomize.v3.2.3_linux_amd64 && \
sudo mv kustomize_kustomize.v3.2.3_linux_amd64 /usr/local/bin/kustomize
```

Verify the install with:

```sh
kustomize version
```
