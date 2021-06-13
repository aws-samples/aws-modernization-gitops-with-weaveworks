+++
title = "Configure Route53 (Optional)"
chapter = true
weight = 80
+++
To see what your ingress controller and cluster look like, head to the console -> EC2 -> LoadBalancers.

You'll see that there is a load balancer between the nodes of your kubernetes cluster, and it is assigned a host name. That is the same host name that appears s output from this command:
```
kubectl get ingress -A
```

To finish a complete HA solution, you could add a **Route 53** DNS round robin (Simple) or a `Failover` type of entry, using one cluster's `Ingress` for `primary` and the other cluster's for `secondary`. This would complete the simplest high availability configuration for EKS.

There are other ingress available for EKS that provide additional features.
