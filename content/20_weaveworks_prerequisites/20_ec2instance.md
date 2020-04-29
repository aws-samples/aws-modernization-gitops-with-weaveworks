---
title: Attach the IAM role to your workspace
chapter: false
weight: 20
---

{{% notice warning %}}
The next set of steps assume that your AWS resources are fully provisioned.  If you were using your own AWS account, ensure that the CloudFormation is in a fully complete status. You can monitor in the AWS console **CloudFormation** service page. When it is done you should see **CREATE_COMPLETE** for all stack and nested stacks.
{{% /notice %}}

1. Follow [this deep link to find your Cloud9 EC2 instance](https://console.aws.amazon.com/ec2/v2/home?#Instances:tag:Name=aws-cloud9-.*workshop.*;sort=desc:launchTime)
1. Select the instance, then choose **Actions / Instance Settings / Attach/Replace IAM Role**
![c9instancerole](/images/c9instancerole.png)
1. Choose **modernization-admin** from the **IAM Role** drop down, and select **Apply** as shown below.
![c9attachrole](/images/c9attachrole.png)
