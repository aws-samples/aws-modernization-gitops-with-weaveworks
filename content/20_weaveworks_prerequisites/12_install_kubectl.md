+++
title = "Install kubectl"
chapter = false
weight = 12

+++

The Kubernetes command-line tool, [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/#install-kubectl-on-linux), allows you to run commands against Kubernetes clusters. You can use kubectl to deploy applications, inspect and manage cluster resources, and view logs.

At the terminal command prompt, enter the following two commands:

```
curl -LO https://storage.googleapis.com/kubernetes-release/release/`curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt`/bin/linux/amd64/kubectl
```

```
chmod +x ./kubectl
```

```
sudo mv ./kubectl /usr/local/bin/kubectl
```

This will install `kubectl` in your **Cloud9** environment. To test to make sure the command is installed properly, execute the command:

```
kubectl version --client
```

You should see the `kubectl` version message.


