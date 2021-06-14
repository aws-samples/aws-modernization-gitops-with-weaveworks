+++
title = "Create IAM Policy and Role"
chapter = false
weight = 17
+++

Creating and using EKS clusters in AWS requires specific IAM permissions for the user performing those actions. Since we are using a Cloud9 Environment during this workshop, we'll create a policy with all required grants, and use that policy in a role that we will attach to our Cloud9 instance.

First, you'll need to create a new IAM policy. In your AWS Console go to `Identity and Access Management (IAM)` and click on `Policies` under `Access Management` in the menu on the left, and click the `Create Policy` button.

![Create Policy Button](/images/create-policy.png)

In the `Create policy`screen, select the `JSON` tab, and paste the JSON document available below:

![JSON Tab](/images/create-policy-json.png)

{{% notice warning %}}
You will need to replace all instances of `<your-account-number>` in the JSON policy below with your actual AWS account number, you can get it by running `aws sts get-caller-identity` in your Cloud9 Console, use the value shown in `Account`
{{% /notice %}}

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Action": "ec2:*",
      "Effect": "Allow",
      "Resource": "*"
    },
    {
      "Effect": "Allow",
      "Action": "elasticloadbalancing:*",
      "Resource": "*"
    },
    {
      "Effect": "Allow",
      "Action": "cloudwatch:*",
      "Resource": "*"
    },
    {
      "Effect": "Allow",
      "Action": "autoscaling:*",
      "Resource": "*"
    },
    {
      "Effect": "Allow",
      "Action": "iam:CreateServiceLinkedRole",
      "Resource": "*",
      "Condition": {
        "StringEquals": {
          "iam:AWSServiceName": [
            "autoscaling.amazonaws.com",
            "ec2scheduled.amazonaws.com",
            "elasticloadbalancing.amazonaws.com",
            "spot.amazonaws.com",
            "spotfleet.amazonaws.com",
            "transitgateway.amazonaws.com"
          ]
        }
      }
    },
    {
      "Effect": "Allow",
      "Action": [
        "cloudformation:*"
      ],
      "Resource": "*"
    },
    {
      "Effect": "Allow",
      "Action": "eks:*",
      "Resource": "*"
    },
    {
      "Action": [
        "ssm:GetParameter",
        "ssm:GetParameters"
      ],
      "Resource": [
        "arn:aws:ssm:*:<your-account-number>:parameter/aws/*",
        "arn:aws:ssm:*::parameter/aws/*"
      ],
      "Effect": "Allow"
    },
    {
      "Action": [
        "kms:CreateGrant",
        "kms:DescribeKey"
      ],
      "Resource": "*",
      "Effect": "Allow"
    },
    {
      "Effect": "Allow",
      "Action": [
        "iam:CreateInstanceProfile",
        "iam:DeleteInstanceProfile",
        "iam:GetInstanceProfile",
        "iam:RemoveRoleFromInstanceProfile",
        "iam:GetRole",
        "iam:CreateRole",
        "iam:DeleteRole",
        "iam:AttachRolePolicy",
        "iam:PutRolePolicy",
        "iam:ListInstanceProfiles",
        "iam:AddRoleToInstanceProfile",
        "iam:ListInstanceProfilesForRole",
        "iam:PassRole",
        "iam:DetachRolePolicy",
        "iam:DeleteRolePolicy",
        "iam:GetRolePolicy",
        "iam:GetOpenIDConnectProvider",
        "iam:CreateOpenIDConnectProvider",
        "iam:DeleteOpenIDConnectProvider",
        "iam:ListAttachedRolePolicies",
        "iam:TagRole"
      ],
      "Resource": [
        "arn:aws:iam::<your-account-number>:instance-profile/eksctl-*",
        "arn:aws:iam::<your-account-number>:role/eksctl-*",
        "arn:aws:iam::<your-account-number>:oidc-provider/*",
        "arn:aws:iam::<your-account-number>:role/aws-service-role/eks-nodegroup.amazonaws.com/AWSServiceRoleForAmazonEKSNodegroup",
        "arn:aws:iam::<your-account-number>:role/eksctl-managed-*"
      ]
    },
    {
      "Effect": "Allow",
      "Action": [
        "iam:GetRole"
      ],
      "Resource": [
        "arn:aws:iam::<your-account-number>:role/*"
      ]
    },
    {
      "Effect": "Allow",
      "Action": [
        "iam:CreateServiceLinkedRole"
      ],
      "Resource": "*",
      "Condition": {
        "StringEquals": {
          "iam:AWSServiceName": [
            "eks.amazonaws.com",
            "eks-nodegroup.amazonaws.com",
            "eks-fargate.amazonaws.com"
          ]
        }
      }
    }
  ]
}
```

Hit `Next` twice, to get to the `Review` screen. Name your new policy `eks-ha-workshop` and click the `Create policy` button.

![Create policy](/images/create-policy-save.png)

Now we just need to create a `Role` that uses this policy so we can attach it to our instance in the next section. Click in `Roles` in the left menu of `Identity and Access Management (IAM)`, then click on `Create role`.

![Create role](/images/create-role.png)

In the `Create role` screen select `AWS service` and `EC2` under `Common use cases`, then click `Next: Permissions`.

![Select service and use case](/images/create-role-service.png)

Now, look for the `policy` we created previously and select it so that it will be attached to our new role.

![Attack policy to role](/images/create-role-policy.png)

Click `Next` twice to get to the `Review` screen, use `eks-ha-workshop-role` for `Role name` and click on `Create role`

![Save new role](/images/create-role-create.png)