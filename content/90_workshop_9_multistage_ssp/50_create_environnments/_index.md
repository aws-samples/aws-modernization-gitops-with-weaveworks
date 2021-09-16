+++
title = "Create Environments"
chapter = true
weight = 50
+++

# Create your environments

Now we are ready to declare our multiple environments, one for each our different stages. For the purposes of this workshop we will only create two stages, `development` and `production`.

Create a directory to hold your environment declarations, and one subdirectory for each environment:

```bash
mkdir -p environments/dev
mkdir -p environments/prod
```

In each environment we will use a `kustomization` to declare the core components and teams we want to bring into each stage, as well as the specific configuration parameters we want each team to use.

## Declare your dev environment

`environments/dev/kustomization.yaml`
```bash
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization
bases:
  - ../../platform/core
  - ../../teams/team-blue
  - ../../teams/team-green
patches:
  - path: blue-patches.yaml
    target:
      kind: Kustomization
      labelSelector: team=blue
  - path: green-patches.yaml
    target:
      kind: Kustomization
      labelSelector: team=green
```

`environments/dev/blue-patches.yaml`
```bash
apiVersion: kustomize.toolkit.fluxcd.io/v1beta1
kind: Kustomization
metadata:
  name: -apps
spec:
  path: "./blue/dev"
```

`environments/dev/green-patches.yaml`
```bash
apiVersion: kustomize.toolkit.fluxcd.io/v1beta1
kind: Kustomization
metadata:
  name: -apps
spec:
  path: "./green/dev"
```

# And now your production environments

`environments/dev/kustomization.yaml`
```bash
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization
bases:
  - ../../platform/core
  - ../../teams/team-blue
  - ../../teams/team-green
patches:
  - path: blue-patches.yaml
    target:
      kind: Kustomization
      labelSelector: team=blue
  - path: green-patches.yaml
    target:
      kind: Kustomization
      labelSelector: team=green
```

`environments/dev/blue-patches.yaml`
```bash
apiVersion: kustomize.toolkit.fluxcd.io/v1beta1
kind: Kustomization
metadata:
  name: -apps
spec:
  path: "./blue/prod"
```

`environments/dev/green-patches.yaml`
```bash
apiVersion: kustomize.toolkit.fluxcd.io/v1beta1
kind: Kustomization
metadata:
  name: -apps
spec:
  path: "./green/prod"
```

## Review your structure so far

At this point, this is how your repository should look like:

```shell
environments
├── dev
│   ├── blue-patches.yaml
│   ├── green-patches.yaml
│   └── kustomization.yaml
└── prod
    ├── blue-patches.yaml
    ├── green-patches.yaml
    └── kustomization.yaml
platform
├── core
│   ├── ingress.yaml
│   └── kustomization.yaml
└── team
    ├── applications.yaml
    ├── kustomization.yaml
    ├── namespace.yaml
    ├── repository.yaml
    ├── role.yaml
    ├── rolebinding.yaml
    └── serviceaccount.yaml
teams
├── team-blue
│   ├── kustomization.yaml
│   └── patches.yaml
└── team-green
    ├── kustomization.yaml
    └── patches.yaml
```

## Commit and push your changes

We now have a fully configure SSP repository, lets push that and spin up our infrastructure.

```bash
git add -A
git commit -am "Shared Service Platform Repository"
git push
```