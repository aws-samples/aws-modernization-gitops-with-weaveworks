+++
title = "Check Everything"
chapter = false
weight = 25
+++

Let's inspect what we have deployed to the cluster by pushing the various manifests into the repo. Using the `kubectl get all -n <namespace>` command, let's look at what has been automatically created in each namespace.

You will now have three new namespaces:

- `flux-system` which holds all the resources that Flux v2 runs to automatically reconcile your cluster against one or more repos

```bash
NAME                                           READY   STATUS    RESTARTS   AGE
pod/helm-controller-85bfd4959d-m9f98           1/1     Running   0          11h
pod/kustomize-controller-7d5959c758-k94sm      1/1     Running   0          11h
pod/notification-controller-758d759586-cz5bx   1/1     Running   0          11h
pod/source-controller-6d986bbb7f-486qv         1/1     Running   0          11h

NAME                              TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)   AGE
service/notification-controller   ClusterIP   10.100.142.52   <none>        80/TCP    11h
service/source-controller         ClusterIP   10.100.26.85    <none>        80/TCP    11h
service/webhook-receiver          ClusterIP   10.100.25.99    <none>        80/TCP    11h

NAME                                      READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/helm-controller           1/1     1            1           11h
deployment.apps/kustomize-controller      1/1     1            1           11h
deployment.apps/notification-controller   1/1     1            1           11h
deployment.apps/source-controller         1/1     1            1           11h

NAME                                                 DESIRED   CURRENT   READY   AGE
replicaset.apps/helm-controller-85bfd4959d           1         1         1       11h
replicaset.apps/kustomize-controller-7d5959c758      1         1         1       11h
replicaset.apps/notification-controller-758d759586   1         1         1       11h
replicaset.apps/source-controller-6d986bbb7f         1         1         1       11h
```

- `ingress-nginx` contains the various resources used by the NGINX Ingress Controller

```bash
NAME                                           READY   STATUS      RESTARTS   AGE
pod/ingress-nginx-admission-create-8gx6c       0/1     Completed   0          11h
pod/ingress-nginx-admission-patch-cb565        0/1     Completed   0          11h
pod/ingress-nginx-controller-98f46f89d-ksntq   1/1     Running     0          11h

NAME                                         TYPE           CLUSTER-IP      EXTERNAL-IP                                                                     PORT(S)                      AGE
service/ingress-nginx-controller             LoadBalancer   10.100.201.90   xxx.elb.xxx.amazonaws.com   80:31893/TCP,443:31157/TCP   11h
service/ingress-nginx-controller-admission   ClusterIP      10.100.173.7    <none>                                                                          443/TCP                      11h

NAME                                       READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/ingress-nginx-controller   1/1     1            1           11h

NAME                                                 DESIRED   CURRENT   READY   AGE
replicaset.apps/ingress-nginx-controller-98f46f89d   1         1         1       11h

NAME                                       COMPLETIONS   DURATION   AGE
job.batch/ingress-nginx-admission-create   1/1           4s         11h
job.batch/ingress-nginx-admission-patch    1/1           3s         11h
```
  
- `hello-kubernetes` is where our sample `Hello Kubernetes` application lives.

```bash
NAME                                           READY   STATUS    RESTARTS   AGE
pod/hello-kubernetes-eks-ha-69968cd578-5k7wq   1/1     Running   0          11h
pod/hello-kubernetes-eks-ha-69968cd578-lmzl7   1/1     Running   0          11h

NAME                              TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)   AGE
service/hello-kubernetes-eks-ha   ClusterIP   10.100.97.246   <none>        80/TCP    11h

NAME                                      READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/hello-kubernetes-eks-ha   2/2     2            2           11h

NAME                                                 DESIRED   CURRENT   READY   AGE
replicaset.apps/hello-kubernetes-eks-ha-69968cd578   2         2         2       11h
```

Go ahead, switch contexts to your other cluster and run the same commands, you will find that both clusters are running identical configurations!