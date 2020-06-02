+++
title = "AWS and Kubenetes Environment Setup"
chapter = false
weight = 501
+++

## AWS and Kubenetes Environment Setup

```sh
# Set default region
export AWS_REGION=$(curl -s 169.254.169.254/latest/dynamic/instance-identity/document | jq -r '.region') # For ec2 client or cloud9
export AWS_DEFAULT_REGION=$AWS_REGION
# I encountered issues with EMR in eu-west-2 but it works fine in eu-west-1.
# Use this variable to set the region to run the EMR cluster in if you encounter issues in your local/default region
export EMR_REGION=$AWS_REGION

export BUCKET_NAME=mlops-kubeflow-pipeline-data2

aws iam create-user --user-name mlops-user
aws iam create-access-key --user-name mlops-user > $HOME/mlops-user.json
export THE_ACCESS_KEY_ID=$(jq '."AccessKey"["AccessKeyId"]' $HOME/mlops-user.json)
echo $THE_ACCESS_KEY_ID
export THE_SECRET_ACCESS_KEY=$(jq '."AccessKey"["SecretAccessKey"]' $HOME/mlops-user.json)

export ACCOUNT_ID=$(aws sts get-caller-identity --output text --query Account)

aws iam create-policy --policy-name mlops-s3-access \
    --policy-document https://raw.githubusercontent.com/paulcarlton-ww/ml-workshop/master/resources/s3-policy.json  > s3-policy.json

aws iam create-policy --policy-name mlops-emr-access \
    --policy-document https://raw.githubusercontent.com/paulcarlton-ww/ml-workshop/master/resources/emr-policy.json > emr-policy.json

aws iam create-policy --policy-name mlops-iam-access \
    --policy-document https://raw.githubusercontent.com/paulcarlton-ww/ml-workshop/master/resources/iam-policy.json > iam-policy.json

aws iam attach-user-policy --user-name mlops-user  --policy-arn $(jq '."Policy"["Arn"]' s3-policy.json)
aws iam attach-user-policy --user-name mlops-user  --policy-arn $(jq '."Policy"["Arn"]' emr-policy.json)

curl  https://raw.githubusercontent.com/paulcarlton-ww/ml-workshop/master/resources/kubeflow-aws-secret.yaml | \
    sed s/YOUR_BASE64_SECRET_ACCESS/$(echo -n "$THE_SECRET_ACCESS_KEY" | base64)/ | \
    sed s/YOUR_BASE64_ACCESS_KEY/$(echo -n "$THE_ACCESS_KEY_ID" | base64)/ | kubectl apply -f -;echo

aws s3api create-bucket --bucket $BUCKET_NAME --region $AWS_DEFAULT_REGION --create-bucket-configuration LocationConstraint=$AWS_DEFAULT_REGION
```
