+++
title = "Using your own account"
chapter = false
weight = 20
+++


{{% notice warning %}}
PLEASE NOTE THAT THESE INSTRUCTIONS ARE INCOMPLETE AS OF **MAY 14, 2020**. PLEASE DO NOT PERFORM THESE WORKSHOPS UNLESS AT AN AWS HOSTED EVENT UNTIL THIS WARNING IS REMOVED.
{{% /notice %}}

{{% notice warning %}}
Only complete this section if you are running the workshop on your own. If you are at an AWS hosted event (such as re:Invent, Kubecon, Immersion Day, etc), goto [Start the workshop at an AWS event](/10_aws_prerequisites/10_aws_event.html).
{{% /notice %}}

### Create an AWS account

{{% notice warning %}}
You are responsible for the cost of the AWS services used while running this workshop in your AWS account.
{{% /notice %}}

{{% notice note %}}
Your account must have the ability to create new IAM roles and scope other IAM permissions.
{{% /notice %}}

1. If you don't already have an AWS account with Administrator access: [create
one now by clicking here](https://aws.amazon.com/getting-started/)

1. Once you have an AWS account, ensure you are following the remaining workshop steps
as an IAM user with administrator access to the AWS account:
[Create a new IAM user to use for the workshop](https://console.aws.amazon.com/iam/home?#/users$new)

1. Enter the user details:
![Create User](/images/iam-1-create-user.png)

1. Attach the AdministratorAccess IAM Policy:
![Attach Policy](/images/iam-2-attach-policy.png)

1. Click to create the new user:
![Confirm User](/images/iam-3-create-user.png)

1. Take note of the login URL and save:
![Login URL](/images/iam-4-save-url.png)

### Login

Now logout and login as as the new **workshop** user.

### Create keypair

This workshop will provision several EC2 VMs, so we will need to make a [key pair](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html) that the CloudFormation script expects. 

1. Within the AWS web console, navigate to **EC2** services and click on the **key pair** left side menu.

    ![keypair-menu](/images/keypair-menu.png)

1. Click on the **create key pair** button and fill in these values

    * name = eksworkshop
    * fileformat = pem

    ![keypair-menu](/images/create-keypair.png)

{{% notice warning %}}
The name must be **eksworkshop** or the CloudFormation stack creation will fail.
{{% /notice %}}

1. Click on the **create key pair**, and a **PEM** file will be downloaded and you will return to the keypair page. 

    ![keypair](/images/keypair.png)

{{% notice info %}}
We will not need the PEM file for the workshop, but we recommend you save this file to a safe place especially if this is your AWS account.
{{% /notice %}}

### Provision the workshop environment

This workshop creates an AWS account and a Cloud9 environment using a CloudFormation script.

1. First need to add keypair named “eksworkshop” in us west-2 (Oregon).
1. Follow [this deep link to load the CloudFormation script.](https://us-west-2.console.aws.amazon.com/cloudformation/home?region=us-west-2#/stacks/create/template?stackName=ModernizationWorkshop-EKS&templateURL=https://modernization-workshop-bucket.s3-us-west-2.amazonaws.com/cfn/master-stacks/vpc-cloud9-eks-QS-based.yaml)

1. The new web page should look like this:
    ![cf_create](/images/cf_create.png)

1. Choose all the defaults and click the **Next** button on each page. On the last page, accept all terms at the last page the CloudFormation flow and then click the **Create Stack** button as shown below.

    ![cf_confirm](/images/cf_confirm.png)

1. The deployment process takes approximately 20 minutes to complete. You can monitor in the AWS console **CloudFormation** service page. When it is done you should see **CREATE_COMPLETE** for all stack and nested stacks as shown below.

![cf_done.png](/images/cf_done.png)

### While you are waiting ...

You will be asked to come back to the AWS console to review that CloudFormation status, but for now head to [**GitOps for EKS Prerequisites**](/20_weaveworks_prerequisites/) section. 
