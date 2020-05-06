+++
title = "Cleanup"
chapter = true
weight = 100
+++

### AWS Cleanup 

{{% notice warning %}}
In order to prevent charges to your account we recommend cleaning up the infrastructure that was created. If you plan to keep things running so you can examine the workshop a bit more please remember to do the cleanup when you are done. It is very easy to leave things running in an AWS account, forget about it, and then accrue charges.
{{% /notice %}}

In the AWS console, go to [CloudFormation](https://us-west-2.console.aws.amazon.com/cloudformation/home?region=us-west-2), click `ModernizationWorkshop-EKS` stack and then `Delete`.  You do not need to choose the stacks for the workshop with the **Nested** tag, they will automatically be deleted as the `ModernizationWorkshop-EKS` stack is.

{{% notice info %}}
This process will take 15-30 minutes, to be sure to verify that none of the `ModernizationWorkshop-EKS` stacks are listed in CloudFormation and you are done.
{{% /notice %}}

### Weaveworks Cleanup 

You have a 15 day trial, so keep using it to monitor and manage your infrastructure and applications.

Here are some additional resources to checkout:

* [Learn more about your tenant and install more OneAgents](https://www.dynatrace.com/support/help/get-started/get-started-with-dynatrace-saas/)
* [Add other users to your tenant](https://www.dynatrace.com/support/help/how-to-use-dynatrace/user-management-and-sso/manage-groups-and-permissions/)
* [YouTube Videos](https://www.youtube.com/channel/UCcYJ-5q_AfmjQ4XTjTS0o3g)
* [More Support resources](https://www.dynatrace.com/services-support/#support-resources-section)

{{% children showhidden="false" %}}
