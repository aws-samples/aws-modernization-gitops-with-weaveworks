+++
title = "Install eksctl"
chapter = false
weight = 11

+++

For this workshop you will use [eksctl](https://eksctl.io/introduction/#installation). Once you install eksctl, you will be ready to get started.

At the terminal command prompt, enter the following two commands:

```
curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
```

```
sudo mv /tmp/eksctl /usr/local/bin
```

This will install `eksctl` in your **Cloud9** environment. To test to make sure the command is installed properly, execute the command:

```
eksctl get cluster
```

You should get a "No clusters found" message. Running the following command will indicate which version of the command line tool you have installed:

```
eksctl version
```

For the current workshop, we will be using `eksctl` **0.53** or newer. Please verify the version, as some features are only available in the **0.53** version of `eksctl` and newer.
