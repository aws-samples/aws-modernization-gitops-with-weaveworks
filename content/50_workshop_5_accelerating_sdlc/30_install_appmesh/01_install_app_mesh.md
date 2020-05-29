+++
title = "Enable App Mesh eksctl Profile"
chapter = false
weight = 301
+++

In the same terminal window that you exported your `GH_USER` and `GH_REPO` to, we will be enabling the `appmesh` eksctl profile.

Run the eksctl profile command:

```sh
eksctl enable profile appmesh \
--profile-revision=demo \
--cluster=gitopssdlcworkshop \
--region=us-west-2 \
--git-user="fluxcd" \
--git-email="${GH_USER}@users.noreply.github.com" \
--git-url="git@github.com:${GH_USER}/${GH_REPO}"
```

Run the fluxctl sync command to install the App Mesh control plane on your cluster:

```sh
fluxctl sync --k8s-fwd-ns flux
```

Inspect the components that were installed:

```sh
kubectl get helmreleases --all-namespaces
```

You should see the following:

```sh
NAMESPACE        NAME                 RELEASE              PHASE       STATUS     MESSAGE                                                                             AGE
appmesh-system   appmesh-controller   appmesh-controller   Succeeded   deployed   Release was successful for Helm release 'appmesh-controller' in 'appmesh-system'.   19h
appmesh-system   appmesh-grafana      appmesh-grafana      Succeeded   deployed   Release was successful for Helm release 'appmesh-grafana' in 'appmesh-system'.      19h
appmesh-system   appmesh-inject       appmesh-inject       Succeeded   deployed   Release was successful for Helm release 'appmesh-inject' in 'appmesh-system'.       19h
appmesh-system   appmesh-prometheus   appmesh-prometheus   Succeeded   deployed   Release was successful for Helm release 'appmesh-prometheus' in 'appmesh-system'.   19h
appmesh-system   flagger              flagger              Succeeded   deployed   Release was successful for Helm release 'flagger' in 'appmesh-system'.              19h
demo             appmesh-gateway      appmesh-gateway      Succeeded   deployed   Release was successful for Helm release 'appmesh-gateway' in 'demo'.                19h
kube-system      metrics-server       metrics-server       Succeeded   deployed   Release was successful for Helm release 'metrics-server' in 'kube-system'.          19h
```

Verify that the mesh was created successfully:

```sh
kubectl describe mesh
```

The mesh was created and is active if you see the following:

```sh
Name:         appmesh
API Version:  appmesh.k8s.aws/v1beta1
Kind:         Mesh
Spec:
  Service Discovery Type:  dns
Status:
  Mesh Condition:
    Status: True
    Type:   MeshActive
```
