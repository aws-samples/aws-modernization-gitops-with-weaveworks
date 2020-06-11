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
# NOTE: don't run this on the terminal tab we're using for the management cluster
# terminal for cluster 1
export KUBECONFIG=./ec2-cluster-1.kubeconfig
# terminal for cluster 2
export KUBECONFIG=./ec2-cluster-1.kubeconfig

```

## GitOps Enable the Clusters

Typically we would use a different repo for the target clusters. But for today we'll reuse the same git repo under a different directory called `flux-ec2`.

* Install Flux

```sh
kubectl create namespace fluxcd
helm repo add fluxcd https://charts.fluxcd.io
kubectl apply -f https://raw.githubusercontent.com/fluxcd/helm-operator/master/deploy/crds.yaml
helm upgrade -i flux fluxcd/flux --wait \
    --namespace fluxcd \
    --set git.url=git@github.com:${GIT_USER}/${GIT_REPO_NAME}.git \
    --set git.path="flux-ec2" \
    --set git.timeout=120s \
    --set git.pollInterval=1m \
    --set syncGarbageCollection.enabled=true \
    --set rbac.create=true
```

Get flux's public key

```sh
fluxctl identity --k8s-fwd-ns fluxcd
```

* Copy printed public key and paste it in your git repo's Settings > Deploy Keys > Add Deploy Key. Make sure to turn on write access. If no key shows up, try re-running `fluxctl identity --k8s-fwd-ns fluxcd` until it shows up.

* Install Helm Operator

```sh
helm upgrade helm-operator fluxcd/helm-operator \
    --force \
    -i \
    --wait \
    --namespace fluxcd \
    --set helm.versions=v3
```

## Setup CNI on all ec2 clusters

```sh
cp ../gitops-cluster-management/examples/cni/weavenet.yaml flux-ec2/weavenet.yaml
git add flux-ec2
git commit -m 'deploy cni to ec2 clusters'
git push
```

## Deploy applications across clusters

```
cp ../gitops-cluster-management/examples/k8s/pod.yaml flux-ec2/pod.yaml
git add flux-ec2
git commit -m 'deploy nginx pod to ec2 clusters'
git push
```