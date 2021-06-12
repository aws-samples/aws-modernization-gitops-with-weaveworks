+++
title = "Enable Your Clusters for GitOps"
chapter = false
weight = 41
+++

{{% notice info %}}
Perform this command once for each one of your clusters, by selecting the appropriate context, running the command and then switching to the other context and doing it again.
{{% /notice %}}

The following command will set up your cluster with Flux v2 and configure your cluster to track its state from the repository created earlier. The first time you run this command, all manifests declaring the state of the cluster will be pushed to the repo, the second time you run it (against the second cluster), the command will refer to the state already stored in the repository.

```
flux bootstrap git \
  --url ssh://git@github.com/<your-username>/eks-ha-workshop.git \
  --private-key-file /home/ec2-user/.ssh/id_rsa \
  --path "./clusters/eks-ha" \
  --interval "1m0s" \
  --branch "main"
```

Let us go through the specified arguments one by one:

- `--url`: This points to a Git URL where the configuration for your cluster will be stored. This is the ssh URI to the repo you created earlier.
- `--private-key-file`: Specifies the private pair of the key you previously created, which will be used to gain access to the repository.
- `--path`: The entire state of your cluster will be stored in the repository you created, all manifests will be created inside this path.
- `--interval`: How often will the repo be polled for changes.
- `--branch`: Which branch to use for pulling/pushing the state of the cluster.

There are more arguments and options, please refer to the [Flux v2 CLI reference](https://fluxcd.io/docs/cmd/) which details all the flags and resulting directory structure.

Note how we are running the exact same command for both clusters. This will result in both clusters tracking the exact same repo, branch and path.
