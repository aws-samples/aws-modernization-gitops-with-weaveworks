+++
title = "Cluster Architecture"
chapter = true
weight = 5
+++

# Create Clusters

For this workshop we will simulate a three cluster architecture, running in fully isolated environments. One AWS account will be used for our Development workloads, a separate AWS Account will be used for Integration workloads, and lastly EKS-Distro will be running on a set of virtual machines deployed on premises.

{{% notice info %}}
To keep the scope of the workshop within the available time and resources, some of the architecture detailed above will be simulated by using namespacess as well a minimal version of EKS-Distro running in an EC2 instance in AWS.
{{% /notice %}}

![Cluster Architecture](/images/module_8-clusters.png)