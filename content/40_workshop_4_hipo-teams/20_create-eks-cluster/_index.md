+++
title = "Create EKS Cluster"
chapter = true
weight = 20
+++

# Create EKS Cluster

For this exercise we will need to create an EKS cluster. We will be using the **Cloud9** terminal window to do this.

{{% notice warning %}}
Not all AWS regions have all EKS capabilities enabled. The config file below assumes you are running the workshop from `us-west-2 (Oregon)`. If you are using a different region make sure you replace `us-west-2` in the config with your region.
{{% /notice %}}

Create a new file in the root of your environment called `cluster.yaml` and paste in the following as the contents:

```yaml
apiVersion: eksctl.io/v1alpha5
kind: ClusterConfig

metadata:
  name: gitopsworkshop
  region: us-west-2

nodeGroups:
  - name: ng-1
    instanceType: m5.large
    desiredCapacity: 1
    iam:
      withAddonPolicies:
        albIngress: true
        appMesh: true
        xRay: true
        cloudWatch: true
```

In the terminal window, execute the following:

```bash
# make sure we are in the root envionment folder
cd ~/environment

# Create the cluster
eksctl create cluster -f cluster.yaml
```

This command will take around 10-15 minutes to create our cluster, so now would be a good time to take a break or ask a question.

When the command completes check that you can access the cluster using **kubectl** by executing the following command in the terminal window:

```bash
kubectl get nodes
```

You should see 2 nodes listed.

{{% notice warning %}}
If you are using your own AWS account, you will need permissions to create EKS clusters plus admin rights within your EKS cluster to configure configuration rules and install agents. Ensure you have authority within your organization to do this in your tenant.
{{% /notice %}}
