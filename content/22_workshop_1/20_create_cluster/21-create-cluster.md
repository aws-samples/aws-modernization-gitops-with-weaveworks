+++
title = "Create a GitOps enabled cluster using eksctl"
chapter = false
weight = 21

+++

[eksctl](https://eksctl.io/introduction/) makes it simple to provision Kubernetes clusters in EKS and enable GitOps in the cluster by automating the deployment of Flux v2.

For this workshop, we will create a default three node EKS cluster and automatically enable GitOps for it. `eksctl` automated installation of Flux v2 will create a repository and synchronize the cluster state with the manifests pushed to it.

Start by creating a configuration file that defines the specifications for your new cluster as well as the details of your GitHub account and repository. Make sure to replace `<your GitHub Username>` in the manifest below with your GitHub username.

```yaml
---
apiVersion: eksctl.io/v1alpha5
kind: ClusterConfig
metadata:
  name: aws-workshop-cluster
  region: us-west-1
nodeGroups:
  - name: aws-workshop-ng
    instanceType: m5.large
    desiredCapacity: 3
gitops:
  flux:
    gitProvider: "github"
    flags:
      owner: "<your GitHub Username>"
      repository: "aws-workshop-module1"
      private: "false"
      personal: "true"
      branch: "main"
      namespace: "flux-system"
      path: "clusters/aws-workshop"
      read-write-key: "true"
```

Save this file in your home directory and name it `~/workshop-cluster.yaml`.

In the section [Enable Git Access](/22_workshop_1/10_setup_gitops_prequisites/11_set_up_git.html), you had created and stored a GitHub personal access token in a file named `~/github.env`, load that file into the environment:

```bash
source ~/github.env
```

To validate you are ready to proceed, run the following two commands. You should see the contents of the cluster config file, and your GitHub Personal access token.

```bash
cat ~/workshop-cluster.yaml
env | grep GITHUB_TOKEN
```

Now, you are ready to run `eksctl` and create your GitOps Enabled Cluster:

```
eksctl create cluster -f ~/workshop-cluster.yaml
```

You will see a number of messages scroll, ending with the `Flux v2 installed successfully` message

<pre>
2021-06-02 15:45:46 [ℹ]  eksctl version 0.53.0
2021-06-02 15:45:46 [ℹ]  using region us-west-1
	.....
2021-06-02 16:11:05 [✔]  all EKS cluster resources for "aws-workshop-cluster" have been created
	.....
2021-06-02 16:13:41 [ℹ]  gitops configuration detected, setting installer to Flux v2
2021-06-02 16:13:41 [ℹ]  ensuring v1 repo components not installed
2021-06-02 16:13:42 [ℹ]  running pre-flight checks
► checking prerequisites
	.....
2021-06-02 16:13:43 [ℹ]  bootstrapping Flux v2 into cluster
► connecting to github.com
	.....
✔ all components are healthy
2021-06-02 16:14:53 [✔]  Flux v2 installed successfully
2021-06-02 16:14:53 [✔]  see https://toolkit.fluxcd.io/ for usage instructions
</pre>

You should now be able to view your cluster with `kubectl`

```
kubectl get nodes
```


