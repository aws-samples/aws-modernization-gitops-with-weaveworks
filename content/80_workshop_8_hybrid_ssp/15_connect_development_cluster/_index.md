+++
title = "Connect to your Development Cluster"
chapter = true
weight = 15
+++

# Connect to your Development EKS Cluster

Your development EKS Cluster has been pre-provisioned with your event engine seat, you will need to get the `kubectl` configuration for the created cluster into your Cloud9 Environment as well.

Running the following command, you will get a new context available for `kubectl` which will allow you to work with the managed EKS that you will use for development environment.

```shell
aws eks update-kubeconfig --name basic-eks
```

After doing this, you will have two different contexts in your `kubectl` configuration, one for the EKS-D cluster we manually created, and another for the managed EKS cluster that was created as part of your event engine seat.

Confirm that by showing the available configuration contexts:

```sh
kubectl config get-contexts
```

The output should be similar to this:

<pre>
CURRENT   NAME                                                   CLUSTER                                                AUTHINFO                                               NAMESPACE
*         arn:aws:eks:us-east-1:302266848463:cluster/basic-eks   arn:aws:eks:us-east-1:302266848463:cluster/basic-eks   arn:aws:eks:us-east-1:302266848463:cluster/basic-eks   
          microk8s                                               microk8s-cluster                                       admin
</pre>

## Let's rename contexts and make our lifes easier

Now, for sake of simplicity, we're going to rename the managed EKS cluster context to `development`, and the EKS-D cluster context to `production`, since we'll be switching back and forth, this will save us a lot of typing:

```shell
kubectl config rename-context <name of your aws context> development
kubectl config rename-context microk8s production
```

Running `kubectl config get-contexts` should show you the following output:

<pre>
CURRENT   NAME          CLUSTER                                                AUTHINFO                                               NAMESPACE
*         development   arn:aws:eks:us-east-1:302266848463:cluster/basic-eks   arn:aws:eks:us-east-1:302266848463:cluster/basic-eks   
          production    microk8s-cluster                                       admin
</pre>