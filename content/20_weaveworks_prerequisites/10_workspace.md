+++
title = "Open Workspace"
chapter = false
weight = 10
+++

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
- Click `Network Settings (advanced)` and select the VPC that starts with `mod-`
- Select `Public subnet 1` for Subnet and click `Next Step` once again
- On the Review page click `Create Environment`

Your environment should deploy and you should see something similar to the image below

![c9before](/images/c9before.png)

