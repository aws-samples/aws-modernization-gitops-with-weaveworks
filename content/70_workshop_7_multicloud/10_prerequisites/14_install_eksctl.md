+++
title = "Install eksctl"
chapter = false
weight = 14

+++

For this workshop you will use a [eksctl](https://eksctl.io/introduction/#installation). Once you install eksctl, you will be ready to get started.

At the terminal command prompt, enter the following two commands:

```
curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
sudo mv /tmp/eksctl /usr/local/bin
```

This will install `eksctl` in your **Cloud9** environment. To test to make sure the command is installed properly, execute the command:

```
eksctl version
```
