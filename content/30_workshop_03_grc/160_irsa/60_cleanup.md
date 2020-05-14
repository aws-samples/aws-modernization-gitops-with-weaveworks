---
title: "Cleanup"
date: 2020-05-12T13:59:44+01:00
weight: 60
draft: false
---

To cleanup, follow the below steps.

To remove sample application from git and delete the namespace

```bash
rm -rf ./alpha/
git add ./alpha
git commit -m "removing alpha ns"
git push
kubectl delete ns alpha
```

To remove IAM role and Service Account stack from cloudformation

```bash
eksctl delete iamserviceaccount --name iam-test --namespace alpha --cluster eksworkshop-eksctl
```
