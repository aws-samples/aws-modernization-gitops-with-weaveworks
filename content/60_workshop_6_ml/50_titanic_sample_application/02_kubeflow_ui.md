+++
title = "Kubeflow UI"
chapter = false
weight = 502
+++

To enable the mlops-user to do a kubectl port-forward:

```sh
aws iam attach-user-policy --user-name mlops-user  --policy-arn arn:aws:iam::aws:policy/AdministratorAccess
eksctl create  iamidentitymapping --cluster $EKS_CLUSTER_NAME --group system:masters --username mlops-user --arn $(aws iam get-user --user-name mlops-user | jq -r '."User"["Arn"]')
```

A shell script is provided to generate the commands required to run kubectl port-forward on workstation

```sh
./get-port-forward-cmds.sh $EKS_CLUSTER_NAME
```

Run the commands generated on a terminal on your workstation (not cloud9 session)

Access UI via: http://127.0.0.1:8080/