+++
title = "Enable Your Cluster for GitOps"
chapter = false
weight = 20
+++

To enable GitOps we will be installing the following to our EKS cluster:

- Flux
- Flux Helm Operator with Helm v3 support

{{% notice tip %}}
In other workshops we have used `eksctl enable repo` to install these. However, for this workshop we will be using Helm to install them both as we want access to additional configuration options (specifically reduce the polling period).
{{% /notice %}}

### Base Setup

Both **Flux** and **Flux Helm Operator** will reside in their won namespace. Run the following command to create the namespace:

```bash
 kubectl create ns flux
```

Both Flux and Helm Operator are via helm charts from the [Flux Chart Repository](https://charts.fluxcd.io). Tell Helm about this repository by running the following command:

```bash
helm repo add fluxcd https://charts.fluxcd.io
```

### Flux Install

Lets install Flux using its Helm Chart making sure you replace `yourname/my-eks-config.git` with your repo details:

```bash
helm upgrade -i flux fluxcd/flux --wait --namespace flux --set git.url=git@github.com:yourname/my-eks-config.git --set git.pollInterval=1m
```

Let us go through the specified arguments one by one:

- **--git-url**: this points to a Git URL where the configuration for your cluster will be stored. This will contain config for the workloads and infrastructure later on. This should be the GitHub repo you created earlier
- **--git-pollInterval**: indicates how often Flux should try check the desired state in Git.

There are more arguments and options, please refer to the [Flux documentation](https://docs.fluxcd.io/en/1.19.0/references/daemon/) which details all the flags.

When Flux is installed you will see a message similar to this:

```bash
NAME: flux
LAST DEPLOYED: Wed May 20 09:50:15 2020
NAMESPACE: flux
STATUS: deployed
REVISION: 1
TEST SUITE: None
NOTES:
Get the Git deploy key by either (a) running

  kubectl -n flux logs deployment/flux | grep identity.pub | cut -d '"' -f2

or by (b) installing fluxctl through
https://docs.fluxcd.io/en/latest/references/fluxctl#installing-fluxctl
and running:

  fluxctl identity --k8s-fwd-ns flux
```

As indicated by the message we need to add the Flux deploy key to our GitHub repo. Run the following command:

```bash
kubectl -n flux logs deployment/flux | grep identity.pub | cut -d '"' -f2
```

Copy the output from this command (which will start with `ssh-rsa`).

Go to the **Settings** for your git repo and then click **Deploy keys**.

Click the **Add deploy** button, enter a title of `Workshop`, paste the contents of the key into **Key** box and make sure you tick **Allow write acess**:

![github_repo_settings](/images/gh_deploy_key.png)

Click **Add key**.

### Helm Operator Install

Install the CRDs that are needed for **HelmRelease**:

```bash
kubectl apply -f https://raw.githubusercontent.com/fluxcd/helm-operator/master/deploy/crds.yaml
```

And then install Helm Operator using the helm chart:

```bash
helm upgrade -i helm-operator fluxcd/helm-operator --wait --namespace flux --set git.ssh.secretName=flux-git-deploy --set helm.versions=v3
```
