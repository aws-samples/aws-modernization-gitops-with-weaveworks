+++
title = "Create EKS Cluster"
chapter = true
weight = 20
+++

# Create EKS Cluster

For this exercise we will need to create an EKS cluster and enable AWS APP Mesh Support. We will be using the **Cloud9** terminal window to do this.

{{% notice warning %}}
Not all AWS regions have all EKS capabilities enabled
{{% /notice %}}

In the terminal window, execute:

```bash
eksctl create cluster --name=eksworkshop --appmesh-access
```

This command will take approximatley 10 minutes to create our cluster, so now would be a good time to take a break or ask a question.

When the command completes check that you can access the cluster using **kubectl** by executing the following command in the terminal window:

```bash
kubectl get nodes
```

You should see 2 nodes listed.

{{% notice warning %}}
If you are using your own AWS account, you will need permissions to create EKS clusters plus admin rights within your EKS cluster to configure configuration rules and install agents. Ensure you have authority within your organization to do this in your tenant.
{{% /notice %}}
