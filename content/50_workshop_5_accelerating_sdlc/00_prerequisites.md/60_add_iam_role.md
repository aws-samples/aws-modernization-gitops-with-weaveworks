+++
title = "Add the Required IAM Role"
chapter = false
weight = 60

+++

Creating and using EKS clusters in AWS requires specific IAM roles for the user creating and accessing EKS clusters.

- Click the **A** button next to the `Share` button in the upper right hand corner of your Cloud9 workspace
- Click **Manage EC2 Instance**
- Make sure your `aws-cloud9-*` instance is selected
- On **Actions** pull down, select **Instance Settings -> Attach/Replace IAM Role**
- In the **IAM role** pull down, select **TeamRoleInstanceProfile**
- To the right, click **Apply**
