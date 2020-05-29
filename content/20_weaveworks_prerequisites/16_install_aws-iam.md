+++
title = "Install AWS IAM"
chapter = false
weight = 16

+++

Amazon EKS uses IAM to provide authentication to your Kubernetes cluster through the AWS IAM authenticator for Kubernetes. You can configure the stock kubectl client to work with Amazon EKS by installing the AWS IAM authenticator for Kubernetes and modifying your kubectl configuration file to use it for authentication. 

To install aws-iam-authenticator on Cloud9

Download the Amazon EKS-vended aws-iam-authenticator binary from Amazon S3: 

```
curl -o aws-iam-authenticator https://amazon-eks.s3.us-west-2.amazonaws.com/1.15.10/2020-02-22/bin/linux/amd64/aws-iam-authenticator
```

Apply execute permissions to the binary:

```
chmod +x ./aws-iam-authenticator
```

And move it into a common directory:

```
sudo mv ./aws-iam-authenticator /usr/local/bin
```

Test that the aws-iam-authenticator binary works:

```
aws-iam-authenticator help
```
