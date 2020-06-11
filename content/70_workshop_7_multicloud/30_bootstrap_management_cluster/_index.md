+++
title = "Bootstrap Management Cluster"
chapter = true
weight = 30
+++

# Bootstrap Management Cluster

## Namespaces

Let's create all the namespaces we need

```sh
kubectl create namespace fluxcd
kubectl create namespace mgmt-clusters
```

## Install Flux

```sh
helm repo add fluxcd https://charts.fluxcd.io
kubectl apply -f https://raw.githubusercontent.com/fluxcd/helm-operator/master/deploy/crds.yaml
helm upgrade -i flux fluxcd/flux --wait \
    --namespace fluxcd \
    --set git.url=git@github.com:${GIT_USER}/${GIT_REPO_NAME}.git \
    --set git.path="flux-mgmt" \
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

## Install Helm Operator

```sh
helm upgrade helm-operator fluxcd/helm-operator \
    --force \
    -i \
    --wait \
    --namespace fluxcd \
    --set helm.versions=v3
```

## Install CAPI

```sh
export AWS_B64ENCODED_CREDENTIALS=$(cat <<EOF | base64 -w 0
[default]
aws_access_key_id = ${CAPI_AWS_ACCESS_KEY_ID}
aws_secret_access_key = ${CAPI_AWS_SECRET_ACCESS_KEY}
EOF
)

clusterctl init --infrastructure aws || true
```

## Validate

* `kubectl get pod` should now show pods under `flux-mgmt` directory

* Create EC2 clusters with GitOps
  * copy `examples/clusters/ec2-cluster-1.yaml` into `flux-mgmt/clusters`. Then, modify the new file's region to `us-west-2`.
