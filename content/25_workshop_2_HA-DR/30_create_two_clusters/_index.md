+++
title = "Create Two EKS Clusters"
chapter = true
weight = 30
+++

# Create Two EKS Clusters

In order to make this exercise easier, you should use two terminal windows. In **Cloud9**, you can open a second terminal window by clicking the "+" icon on the tab bar.

![Event Engine](/images/cloud9_new_terminal.png)

In each terminal, we are going to use the `eksctl` command to create a cluster, and give them specific names. I suggest simple names, like "east" and "west" or something similar. Optionally, you can add the `-r region_id` option to the command line in order to place the cluster in a specific AWS region.

{{% notice warning %}}
Not all AWS regions have all EKS capabilities enabled
{{% /notice %}}

In the first terminal window, execute:
```
eksctl create cluster -n east --alb-ingress-access
```
and in the second terminal window, execute:
```
elsctl create cluster -n west --alb-ingress-access
```
These commands will take approximately 10 minutes to execute, so now would be a good time to take a break or ask a question.

When they have completed, you will have two node EKS clusters to work with.

{{% notice warning %}}
If you are using your own AWS account, you will need permissions to create EKS clusters plus admin rights within your EKS cluster to configure configuration rules and install agents. Ensure you have authority within your organization to do this in your tenant. 
{{% /notice %}}

