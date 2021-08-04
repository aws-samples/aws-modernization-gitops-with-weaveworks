+++
title = "Grant permissions to role"
chapter = false
weight = 17
+++

Your event engine seat has been pre-provisioned with an IAM role with basic access rights, for the purpose of the workshop and considering you are running in a sandboxed environment, we will grant wider privileges to the precreated role.

{{% notice warning %}}
Granting full privileges to a role is **not** something to do in any scenario outside of sandboxed and ephemeral event engine use cases.
{{% /notice %}}

- Navigate to the Identity and Access Management (IAM) service in AWS.
- Click on `Roles` and look for a role that begins with `mod-` with `AWS Service: ec2` for Trusted Entities.
- Click on the Role and then click on `Attach policies`
- Select `AdministratorAccess` and click on `Attach policy`