+++
title = "Workshop Prerequisites"
chapter = true
weight = 1
+++

# Prerequisites

Please note that in order to complete this workshop, you should have:

* Completed [Workshop 1: Introduction to GitOps on EKS](/22_workshop_1/)
* Setup you environment using [Prerequisites for all GitOps Workshops](/20_weaveworks_prerequisites.html)

Once you have completed both of these, please return here and run the following command in the **Cloud9** terminal:

```bash
aws sts get-caller-identity --query Arn | grep TeamRole -q && echo "IAM role valid" || echo "IAM role NOT valid"
```

If the IAM role is not valid, <span style="color: red;">**DO NOT PROCEED**</span>. Go back and confirm the steps on this page.

{{% notice warning %}}
If you are using your own AWS account, you will need permissions to create EKS clusters plus admin rights within your EKS cluster to configure configuration rules and install agents. Ensure you have authority within your organization to do this in your tenant.
{{% /notice %}}
