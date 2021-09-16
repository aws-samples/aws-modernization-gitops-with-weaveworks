+++
title = "Onboard Teams"
chapter = true
weight = 40
+++

# Onboard your teams

In our workshop we are going to work with two sample Teams: `team-green` and `team-blue`.

Each of these teams has a repository already configured with declarations that deploy sample applications. Now we need to onboard our teams, which means we will add declarations that use our team base and patch the necessary attributes for each team.

Create a `teams` directory in your repository and in there create two sub-directories: `team-green` and `team-blue`

```shell
mkdir -p teams/team-green
mkdir -p teams/team-blue
```

Now let's create a `kustomization` for each team, including the necessary patches to match their specific git repository, as well as the unique namespace we want to provide them:

`teams/team-green/kustomization.yaml`
```yaml
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization
namespace: green-ns
namePrefix: green
commonLabels:
  team: green
bases:
  - ../../platform/team
patchesStrategicMerge:
  - patches.yaml
```

`teams/team-green/patches.yaml`
```yaml
---
apiVersion: kustomize.toolkit.fluxcd.io/v1beta1
kind: Kustomization
metadata:
  name: -apps
spec:
  serviceAccountName: green-sa
  sourceRef:
    name: green-repository
---
apiVersion: source.toolkit.fluxcd.io/v1beta1
kind: GitRepository
metadata:
  name: -repository
spec:
  url: https://github.com/weaveworks-gitops-demo/team-green.git
  ref:
    branch: main
...
```

`teams/team-blue/kustomization.yaml`
```yaml
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization
namespace: blue-ns
namePrefix: blue
commonLabels:
  team: blue
bases:
  - ../../platform/team
patchesStrategicMerge:
  - patches.yaml
```

`teams/team-blue/patches.yaml`
```yaml
---
apiVersion: kustomize.toolkit.fluxcd.io/v1beta1
kind: Kustomization
metadata:
  name: -apps
spec:
  serviceAccountName: blue-sa
  sourceRef:
    name: blue-repository
---
apiVersion: source.toolkit.fluxcd.io/v1beta1
kind: GitRepository
metadata:
  name: -repository
spec:
  url: https://github.com/weaveworks-gitops-demo/team-blue.git
  ref:
    branch: main
...
```

## Review your structure so far

At this point, this is how your repository should look like:

```shell
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