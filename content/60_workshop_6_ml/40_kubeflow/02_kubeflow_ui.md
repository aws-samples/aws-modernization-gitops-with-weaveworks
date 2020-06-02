+++
title = "Kubeflow UI"
chapter = false
weight = 402
+++

Use terminal on your workstation (not cloud9 session)

```sh
export AWS_DEFAULT_REGION=eu-west-2 # the region from cloud9 environment
EKS_CLUSTER_NAME=mlops-c9 # name of your cluster
aws eks update-kubeconfig --name $EKS_CLUSTER_NAME
kubectl port-forward svc/istio-ingressgateway -n istio-system 8080:80 &
```

Access UI via: http://127.0.0.1:8080/