+++
title = "Open Workspace"
chapter = false
weight = 10
+++

## Create admin role

- Switch to the **AWS Console** (You can open the console from the "Team Dashboard")
- Under **Services**, select **IAM > Roles > Create Role**
- Confirm that AWS service and EC2 are selected, then click Next to view permissions.
- Confirm that AdministratorAccess is checked, then click Next: Tags to assign tags.
- Take the defaults, and click Next: Review to review.
- Enter `eksworkshop-admin` for the Name, and click **Create role**

## Create Cloud9 Instance

{{% notice info %}}
[AWS Cloud9](https://aws.amazon.com/cloud9/) is a cloud-based integrated development environment (IDE) that lets you write, run, and debug your code with just a browser. It includes a code editor, debugger, and terminal. Cloud9 comes prepackaged with essential tools for popular programming languages, including JavaScript, Python, PHP, and more, so you don’t need to install files or configure your development machine to start new projects.
{{% /notice %}}

{{% notice tip %}}
Ad blockers, javascript disablers, and tracking blockers should be disabled for
the cloud9 domain, or connecting to the workspace might be impacted.
Cloud9 requires third-party-cookies. You can whitelist the [specific domains]( https://docs.aws.amazon.com/cloud9/latest/user-guide/troubleshooting.html#troubleshooting-env-loading).
{{% /notice %}}

- In the AWS console, navigate to the cloud9 service and click `Create Environment`
- For the Name type in `WorkshopIDE` and click `Next Step`
- Leave all the defaults and click `Next Step` once again
- On the Review page click `Create Environment`

Your environment should deploy and you should see something similar to the image below

![c9before](/images/c9before.png)

### Show Hidden Files

* Show hidden files in Cloud9 environment by going to `Settings > User Settings > Tree and Go Panel`, then set the Hidden File Pattern to `*.pyc, __pycache__`