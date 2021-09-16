+++
title = "Add Core Components"
chapter = true
weight = 20
+++

# Add Core Components

A shared service platform allows platform operations teams to add components to multiple environments declaratively from a single location. We are going to call those components `core components` and will place them in a specific location in our `team-ssp` repository.

We're going to store all declarations that we want to include as core components to our platform in a `platform` directory, and in there create multiple subdirectories for the various type of state declarations we will want to deploy to our environments.

Go into your empty `team-ssp` clone and create a directory called `platform` and in platform, let's create a directory called `core`:

```shell
cd team-ssp
mkdir -p platform/core
```

We are going to store all our core services as manifests in this directory.

## Add an ingress controller.

All of our teams will need to expose services to the outside world, and for that, we are going to provide them an ingress controller.

Create a new file named `platform/core/ingress.yaml` and copy into it the contents below. This manifest will create a namespace for our `ingress-nginx` service and use `Helm` to deploy the NGINX Ingress controller. As you'll notice we are doing this using Custom Resource Definitions from our GitOps Operator, flux.

```yaml
---
apiVersion: v1
kind: Namespace
metadata:
  name: ingress-nginx
---
apiVersion: source.toolkit.fluxcd.io/v1beta1
kind: HelmRepository
metadata:
  name: ingress-nginx
  namespace: ingress-nginx
spec:
  interval: 5m
  url: https://kubernetes.github.io/ingress-nginx
---
apiVersion: helm.toolkit.fluxcd.io/v2beta1
kind: HelmRelease
metadata:
  name: ingress-nginx
  namespace: ingress-nginx
spec:
  releaseName: ingress-nginx
  chart:
    spec:
      chart: ingress-nginx
      sourceRef:
        kind: HelmRepository
        name: ingress-nginx
        namespace: ingress-nginx
  interval: 5m
...
```

To learn more about Flux visit the [official Flux Documentation](https://fluxcd.io/docs/)

## Create a kustomization

In order for our `ingress.yaml` file to be loaded into our clustes during bootstraping, make sure to add a `platform/core/kustomization.yaml` file that will load our ingress, as well as any future services we add to the cluster.

```yaml
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization
resources:
  - ingress.yaml
```

## Review your structure so far

At this point, this is how your repository should look like:

```shell
platform/core
├── ingress.yaml
└── kustomization.yaml
```