+++
title = "Check Everything"
chapter = false
weight = 25
+++

When all the manifests have been created and pushed to your git repository, we can verify that everything worked as expected. The software agent in the cluster, `flux`, checks for commit updates every five minutes (by default). After it has checked your git repository, and applied the changed manifests, we will want to verify everything.

In each terminal session, execute:
```
kubectl get pods -A
```
You should see output very similar to this:
<pre>
NAMESPACE     NAME                                      READY   STATUS    RESTARTS   AGE
flux          flux-7ddc59f94c-zxdcx                     1/1     Running   0          4d18h
flux          helm-operator-74546bd6f5-9mcls            1/1     Running   0          4d18h
flux          memcached-689f7846dc-f587t                1/1     Running   0          4d18h
kube-system   alb-ingress-controller-769b6d855d-fntnm   1/1     Running   0          5d20h
kube-system   aws-node-25p47                            1/1     Running   0          5d20h
kube-system   aws-node-lgpvw                            1/1     Running   0          5d20h
kube-system   coredns-5fb4bd6df8-f6c2d                  1/1     Running   0          5d21h
kube-system   coredns-5fb4bd6df8-n7rws                  1/1     Running   0          5d21h
kube-system   kube-proxy-bm8gp                          1/1     Running   0          5d20h
kube-system   kube-proxy-rf92b                          1/1     Running   0          5d20h
podinfo       podinfo-5f6f84ff7b-7nndj                  1/1     Running   0          4d18h
podinfo       podinfo-5f6f84ff7b-hfw2j                  1/1     Running   0          4d18h
podinfo       podinfo-5f6f84ff7b-zjtnc                  1/1     Running   0          4d18h
</pre>
We also want to find out how to access the clsuter from the outside world. To see the host name of your ingress controller, execute:
```
kubectl get ingress -A
```
You should see output similar to this:
<pre>
NAMESPACE   NAME              HOSTS   ADDRESS                                                                  PORTS   AGE
podinfo     podinfo-ingress   *       e737fb32-podinfo-podinfoin-2c89-1571962006.us-east-2.elb.amazonaws.com   80      4d18h
</pre>
