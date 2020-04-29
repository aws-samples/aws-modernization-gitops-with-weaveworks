+++
title = "Create a Cluster with eksctl"
chapter = false
weight = 20

+++

[eksctl](https://eksctl.io/introduction/) makes it simple to provision Kubernetes clusters in EKS. For this workshop, we will create a defauklt three node EKS cluster. With `eksctl`, this is a single command line:

```
eksctl create cluster
```

You will see a number of messages scroll, ending with the `kubeconfig` message

<pre>
[ℹ]  eksctl version 0.18.0
[ℹ]  using region us-west-2
	.....
[ℹ]  deploying stack "eksctl-unique-unicorn-1588181335-nodegroup-ng-d61e2b06"
[✔]  all EKS cluster resources for "unique-unicorn-1588181335" have been created
[✔]  saved kubeconfig as "/home/ec2-user/.kube/config"
</pre>

You should now be able to view your cluster with `kubectl`

```
kubectl get nodes
```


