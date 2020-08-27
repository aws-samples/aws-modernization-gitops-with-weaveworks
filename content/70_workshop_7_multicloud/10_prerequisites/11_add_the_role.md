+++
title = "Add the Required IAM Role"
chapter = false
weight = 11

+++

Creating and using EKS clusters in AWS requires specific IAM roles for the user creating and accessing EKS clusters.

## Attach to cloud9 instance

- Switch to the **AWS Console** (You can open the console from the "Team Dashboard")
- Under **Services**, select **EC2**
- Click **Running Instances**
- Select the instance named "aws-cloud9-..." by clicking the check box to the left of the name
- On **Actions** pull down, select **Instance Settings -> Attach/Replace IAM Role**
- In the **IAM role** pull down, select **eksworkshop-admin**
- To the right, click **Apply**
