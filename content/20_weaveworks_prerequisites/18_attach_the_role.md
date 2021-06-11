+++
title = "Attach Role to instance"
chapter = false
weight = 18
+++

Now, we need to attach the IAM role we created in the previous step, to the Cloud9 Instance that you will be working on:

- Switch to the **AWS Console** (You can open the console from the "Team Dashboard")
- Under **Services**, select **EC2**
- Click **Running Instances**
- Select the instance named "aws-cloud9-..." by clicking the check box to the left of the name
- On **Actions** pull down, select **Security -> Modify IAM Role**
- In the **IAM role** pull down, select **eks-ha-workshop-role**
- To the right, click **Apply**
