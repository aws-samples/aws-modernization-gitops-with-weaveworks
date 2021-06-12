+++
title = "Create Two EKS Clusters"
chapter = true
weight = 30
+++

# Create Two EKS Clusters

In order to make this exercise easier, you should use two terminal windows. In **Cloud9**, you can open a second terminal window by clicking the "+" icon on the tab bar.

![Event Engine](/images/cloud9_new_terminal.png)

In each terminal, we are going to use the `eksctl` command to create two clusters, one in the east using the `us-east-2` region, and one in the west on the `us-west-1` region.

{{% notice warning %}}
Not all AWS regions have all EKS capabilities enabled and there may be capacity constraints that may force you to use a different region. You can specify the region to use in the applicable property of the config files below.
{{% /notice %}}

Let's create two files, one for each cluster that we will create. Files will be almost identical, with the exception of the name of the cluster, and the cluster's region.

Copy and paste the following contents into a file named `~/east.yaml`:

```yaml
---
apiVersion: eksctl.io/v1alpha5
kind: ClusterConfig
metadata:
  name: eks-ha-workshop-east
  region: us-east-2
nodeGroups:
  - name: aws-workshop-ng
    instanceType: m5.large
    desiredCapacity: 2
```

And create a file named `~/west.yaml` copy-pasting the following contents:

```yaml
---
apiVersion: eksctl.io/v1alpha5
kind: ClusterConfig
metadata:
  name: eks-ha-workshop-west
  region: us-west-1
nodeGroups:
  - name: aws-workshop-ng
    instanceType: m5.large
    desiredCapacity: 2
```

In the first terminal window, execute:

```bash
eksctl create cluster -f ~/east.yaml
```

And in the second terminal window, execute:

```bash
eksctl create cluster -f ~/west.yaml
```

By running these `eksctl create cluster` commands, we will:

- Create two clusters, both running two `m5.large` worker nodes:
  - One in the `us-east-2` region
  - One in the `us-west-1` region
- Use the official AWS EKS AMI
- Create a dedicated VPC for each cluster

As we covered in the webinar and as most of you know, EKS solves some major pain points with managing Kubernetes. The onus of managing Kubernetes upgrades and patching is taken up by AWS, with zero downtime upgrades. Another benefit that EKS gives you is High Availability in a regional cluster. EKS runs the Kubernetes management infrastructure across multiple Availability Zones, and detects and replaces unhealthy control plane nodes.

{{% notice warning %}}
If you are using your own AWS account, you will need permissions to create EKS clusters plus admin rights within your EKS cluster to configure configuration rules and install agents. Ensure you have authority within your organization to do this in your tenant.
{{% /notice %}}
