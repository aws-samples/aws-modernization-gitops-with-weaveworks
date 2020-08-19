+++
title = "Create EKS Cluster"
chapter = false
weight = 101
+++

If an eks cluster has already been created for you:
```sh
export AWS_REGION=$(curl -s 169.254.169.254/latest/dynamic/instance-identity/document | jq -r '.region')
export AWS_DEFAULT_REGION=$AWS_REGION
export EKS_CLUSTER_NAME=$(eksctl get cluster -o json | jq -r '.[0]["name"]')
eksctl utils write-kubeconfig --name $EKS_CLUSTER_NAME
```

Before creating the EKS cluster, please make sure that you have the right role.

```sh
aws sts get-caller-identity --query Arn | grep TeamRole -q && echo "IAM role valid" || echo "IAM role NOT valid"
```

If the IAM role is not valid, <span style="color: red;">**DO NOT PROCEED**</span>. Go back and confirm the steps on this page.

Once you have ensured your IAM role is valid, proceed to define your cluster configuration.

To create the EKS cluster, you will need to run the following commands:

```sh
export AWS_REGION=$(curl -s 169.254.169.254/latest/dynamic/instance-identity/document | jq -r '.region')
export AWS_DEFAULT_REGION=$AWS_REGION
export EKS_CLUSTER_NAME=mlops-c9

# Create KMS key for encrypted secrets support
key_arn=$(aws --region $AWS_DEFAULT_REGION kms create-key | jq -r '."KeyMetadata"["Arn"]')

mkdir -p ${EKS_CLUSTER_NAME}
pushd ${EKS_CLUSTER_NAME}
# Generate ssh key pair for access to EC2 nodes
ssh-keygen -b 4096 -f id_rsa -q -t rsa -N "" 2>/dev/null <<< y >/dev/null
curl  https://raw.githubusercontent.com/paulcarlton-ww/ml-workshop/master/resources/eks-template.yaml | \
    sed s/NAME/$EKS_CLUSTER_NAME/ | sed s/REGION/$AWS_DEFAULT_REGION/ | sed s#KEY#$key_arn# > eks.yaml
```
Then create the cluster:
```
eksctl create cluster --config-file eks.yaml
popd
```

The creation of the cluster typically takes about 20 minutes. You can monitor the progress of your cluster by navigating to `AWS Console > Services > CloudFormation`.

