+++
title = "Deploy app-dev profile"
chapter = false
weight = 301
+++

```sh
EKSCTL_EXPERIMENTAL=true eksctl enable profile app-dev \
        --git-url git@github.com:$GIT_ORG/${EKS_CLUSTER_NAME}-config \
        --git-email $GIT_EMAIL \
        --cluster $EKS_CLUSTER_NAME \
        --region "$AWS_DEFAULT_REGION"

cd $GITHUB_DIR/$GIT_ORG
git clone git@github.com:$GIT_ORG/${EKS_CLUSTER_NAME}-config.git
cd ${EKS_CLUSTER_NAME}-config
```
Wait a minute or two for flux to deploy applications, verify with 'kubectl get pods -A'