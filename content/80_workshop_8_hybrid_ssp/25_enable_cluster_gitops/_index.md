+++
title = "Enable GitOps on both clusters"
chapter = true
weight = 25
+++

# Install Weave GitOps on both clusters

We will be using Weave GitOps as GitOps Operator in both our clusters, and will need to install all the required controllers in both of them.

This can be easily accomplished with a single command line! Let's start with your development, managed EKS Cluster.

{{% notice info %}}
Replace the name of the context with the one in your configuration. To get the full name use `kubectl config get-contexts`
{{% /notice %}}

```sh
kubectl config use-context <arn:aws:eks:....>
wego gitops install
```

You should see output similar to the following:

<pre>
✚ generating manifests
✔ manifests build completed
► installing components in wego-system namespace
◎ verifying installation
✔ image-reflector-controller: deployment ready
✔ image-automation-controller: deployment ready
✔ source-controller: deployment ready
✔ kustomize-controller: deployment ready
✔ helm-controller: deployment ready
✔ notification-controller: deployment ready
✔ install finished
</pre>

Now, switch over to your EKS-D cluster context and run `wego gitops install` as well, the output should be identical to the one above.

```shell
kubectl config use-context microk8s
wego gitops install
```

## Verify Weave GitOps is installed

You can verify that the clusters are running all required components by running `kubectl get all -n wego-system` in each of them, the output on both clusters should look like this:

<pre>
NAME                                               READY   STATUS    RESTARTS   AGE
pod/helm-controller-b5c45677f-h8h5l                1/1     Running   0          25m
pod/image-automation-controller-57f8bf65c7-pqvrl   1/1     Running   0          25m
pod/image-reflector-controller-558c56b989-rw6nq    1/1     Running   0          25m
pod/kustomize-controller-7fd4d6b4c8-746zw          1/1     Running   0          25m
pod/source-controller-5d469c75b6-h9zkh             1/1     Running   0          25m
pod/notification-controller-66545867f4-jhk5j       1/1     Running   0          25m

NAME                              TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)   AGE
service/notification-controller   ClusterIP   10.152.183.122   <none>        80/TCP    25m
service/source-controller         ClusterIP   10.152.183.97    <none>        80/TCP    25m
service/webhook-receiver          ClusterIP   10.152.183.53    <none>        80/TCP    25m

NAME                                          READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/helm-controller               1/1     1            1           25m
deployment.apps/image-automation-controller   1/1     1            1           25m
deployment.apps/image-reflector-controller    1/1     1            1           25m
deployment.apps/kustomize-controller          1/1     1            1           25m
deployment.apps/source-controller             1/1     1            1           25m
deployment.apps/notification-controller       1/1     1            1           25m

NAME                                                     DESIRED   CURRENT   READY   AGE
replicaset.apps/helm-controller-b5c45677f                1         1         1       25m
replicaset.apps/image-automation-controller-57f8bf65c7   1         1         1       25m
replicaset.apps/image-reflector-controller-558c56b989    1         1         1       25m
replicaset.apps/kustomize-controller-7fd4d6b4c8          1         1         1       25m
replicaset.apps/source-controller-5d469c75b6             1         1         1       25m
replicaset.apps/notification-controller-66545867f4       1         1         1       25m
</pre>