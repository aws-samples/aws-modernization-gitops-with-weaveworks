+++
title = "Create EKS Cluster"
chapter = false
weight = 101
+++

Before creating the EKS cluster, please make sure that you have the right role.

```sh
aws sts get-caller-identity --query Arn | grep TeamRole -q && echo "IAM role valid" || echo "IAM role NOT valid"
```

If the IAM role is not valid, <span style="color: red;">**DO NOT PROCEED**</span>. Go back and confirm the steps on this page.

Once you have ensured your IAM role is valid, proceed to define your cluster configuration.

Create a `cluster.yaml` file in the root of your Cloud9 environment. Copy and paste the yaml below:

```yaml
apiVersion: eksctl.io/v1alpha5
kind: ClusterConfig
metadata:
  name: gitopssdlcworkshop
  region: us-west-2
nodeGroups:
  - name: default
    instanceType: m5.large
    desiredCapacity: 2
    volumeSize: 120
    iam:
      withAddonPolicies:
        appMesh: true
        xRay: true
```

To create the EKS cluster, you will need to run the following command:

```bash
# make sure we are in the root environment folder
cd ~/environment

# Create the cluster
eksctl create cluster -f cluster.yaml
```

The creation of the cluster typically takes about 20 minutes. You can monitor the progress of your cluster by navigating to `AWS Console > Services > CloudFormation`.

Since we have some time, let's review some [Advanced Deployment Strategies](50_workshop_5_accelerating_sdlc/50_deployment_strategies.html).
