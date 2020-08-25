+++
title = "Add YAMLs"
chapter = true
weight = 31
+++

# Add YAMLs

In this section we're going to deploy resources using GitOps.

Make sure you're in your repo

```sh
cd aws-gitops-multicloud
```

## Create a Pod

Create a pod using gitops

```sh
cp ../gitops-cluster-management/examples/k8s/nginx.yaml flux-mgmt/nginx.yaml
git add flux-mgmt
git commit -m 'deploy nginx pod'
git push
```

Shortly after you should find the pod in the default namespace

```sh
kubectl get pod
```

## Create a HelmRelease

```sh
cp ../gitops-cluster-management/examples/k8s/helm/redis.yaml flux-mgmt/redis.yaml
git add flux-mgmt
git commit -m 'deploy redis helmrelease'
git push
```

Validate that it worked by checking if the helmrelease and the pod are running

```sh
kubectl get hr
kubectl get pod
```

## Deploy secret-copier custom operator

```sh
cp -R ../gitops-cluster-management/examples/k8s/custom-operators/ flux-mgmt
cp ../gitops-cluster-management/examples/k8s/all-ns-deployment.yaml flux-mgmt
cp ../gitops-cluster-management/examples/k8s/all-ns-secret.yaml flux-mgmt
git add flux-mgmt
git commit -m 'deploy custom operators'
git push
```

Now any secret or deployment in the default namespace that has the labels `secret-copier: "yes"` will be copied across namespaces.

Validate that it worked by checking if the example secret and deployment in your cluster are copied across all namespaces.

```sh
kubectl get secret -A | grep copy-me
kubectl get deployment -A | grep memcached
```

For more info you can go through the shell-operator source code for secret-copier and deployment-copier in `gitops-cluster-management/operators`.

## Create ec2 clusters

Let's create two EC2 clusters using CAPI.

```sh
cp -R ../gitops-cluster-management/examples/k8s/clusters/ flux-mgmt
```

Modify the two cluster files `ec2-cluster-1.yaml` and `ec2-cluster-2.yaml` as follows:

* `AWSCluster.spec.region` to `us-west-2`
* `AWSCluster.spec.sshKeyName` to `weaveworks-workshop`
* `AWSMachineTemplate.spec.template.spec.sshKeyName` to `weaveworks-workshop` (There should be 2 machine templates per cluster. One for the control plane instances and one for the worker instances)

Finally, let's push our changes to git

```sh
git add flux-mgmt
git commit -m 'create two ec2 clusters'
git push
```

We can monitor cluster creation

```
kubectl get clusters -w
kubectl get machines -w
kubectl logs --tail 100 -f -n capa-system deploy/capa-controller-manager -c manager
```

## Simulate a disaster

```sh
kubectl get machines
kubectl delete machine <MACHINE-NAME>
```

Watch what happens

```sh
kubectl get machines -w
```

CAPI should take care of destroying the AWS EC2 instance, and provision a new one to replace it.


## Scale up your cluster

You can scale up or down our cluster instances by increasing or decreasing the number of replicas for the control plane or worker nodes in our yaml files.

Let's bump up the replicas from 3 to 5 for the worker nodes.

```sh
git add flux-mgmt
git commit -m 'scale up'
git push
```

Watch what happens

```sh
kubectl get machines -w
```