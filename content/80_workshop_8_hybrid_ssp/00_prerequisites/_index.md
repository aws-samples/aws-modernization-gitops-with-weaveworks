+++
title = "Workshop Prerequisites"
chapter = true
weight = 1
+++

# Prerequisites

Please note that in order to complete this workshop, you should have completed [Prerequisites for all GitOps Workshops](/20_weaveworks_prerequisites.html). Once you have completed the Introduction to GitOps Workshop, please return to perform this workshop.

{{% notice warning %}}
If you are using your own AWS account, you will need permissions to create EKS clusters plus admin rights within your EKS cluster to configure configuration rules and install agents. Ensure you have authority within your organization to do this in your tenant. 
{{% /notice %}}

## Add the EKS security group to your Cloud9 Instance

As part of your Event Engine setup you have access to a pre-created EKS cluster, in order to access that cluster you will need to attach the security group in use by the EKS cluster to your Cloud9 Environment.

Go to your EC2 Instances and select the instance with a name starting with `aws-cloud9`, Click on `Actions`, `Security` and select `Change Security Groups`.

![Modify Security Groups](/images/module_8-modify-security-groups.png)

Now click on the `Select security groups` field, you will see a list of all available security groups, look for the one that starts with `eks-cluster-sg-basic-eks`, select it and click on `Add security group`, you should now see two security groups in the list below. Now, hit `Save`

![Add EKS Security Group](/images/module_8-add-security-groups.png)