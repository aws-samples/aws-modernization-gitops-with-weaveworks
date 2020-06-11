+++
title = "GitOps Enable Clusters"
chapter = true
weight = 40
+++

# GitOps Enable the Two EC2 Clusters

With two terminal windows open, you'll have to be careful when executing commands to ensure you are working with the correct cluster. In these exercises, you will be flipping back and forth between your two terminal windows fairly often. As this can get confusing, just be sure to verify which window you are using each time.

Let's create two new kubeconfigs for the two EC2 clusters we have. The admin kubeconfigs are stored as secrets on the management cluster.

```sh
kubectl get secret | grep ec2-cluster
```

Let's write the kubeconfig files. (Replace SECRETNAME with your cluster's secret name)

```sh
kubectl --namespace=default get secret/SECRETNAME-1 -o jsonpath={.data.value} \
  | base64 --decode \
  > ./ec2-cluster-1.kubeconfig

kubectl --namespace=default get secret/SECRETNAME-2 -o jsonpath={.data.value} \
  | base64 --decode \
  > ./ec2-cluster-2.kubeconfig
```

Open a new terminal for each cluster and in each one point KUBECONFIG to the right file.

```sh
export KUBECONFIG=./ec2-cluster-1.kubeconfig
```
