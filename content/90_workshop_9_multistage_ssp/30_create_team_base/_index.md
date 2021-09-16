+++
title = "Create Team Base"
chapter = true
weight = 30
+++

# Create a team base

Each one of our teams will need some common resources: we will want to give every team their own `namespace` as well as a `ServiceAccount` with permissions to perform actions only within that namespace, also each team will  have a `GitRepository` that will point to their specific repository and a `Kustomization` object that will deploy resources from some specific path or Git reference in their own repository.

Not that some of these attributes will need to be updated for every team, and some may need to be updated for every team and every stage. We will use the power of Kustomize to patch those specific resources as we use these bases when onboarding our teams.

Create a directory for the base resources you will use across your teams:

```bash
mkdir -p platform/team
```

And now, create the various resources that will be needed for every team to be onboarded onto the various stages:

`platform/team/applications.yaml`
```yaml
apiVersion: kustomize.toolkit.fluxcd.io/v1beta1
kind: Kustomization
metadata:
  name: -apps
spec:
  interval: 1m
  prune: true
  sourceRef:
    kind: GitRepository
  timeout: 2m
  serviceAccountName: thisMustBeReplaced
```

`platform/team/namespace.yaml`
```yaml
---
apiVersion: v1
kind: Namespace
metadata:
  name: -ns
```

`platform/team/repository.yaml`
```yaml
apiVersion: source.toolkit.fluxcd.io/v1beta1
kind: GitRepository
metadata:
  name: -repository
spec:
  interval: 1m
```

`platform/team/role.yaml`
```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: -role
rules:
  - apiGroups: ["*"]
    resources: ["*"]
    verbs: ["*"]
```

`platform/team/rolebinding.yaml`
```yaml
---
kind: RoleBinding
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: -binding
subjects:
  - kind: ServiceAccount
    name: -sa
roleRef:
  kind: Role
  name: -role
  apiGroup: rbac.authorization.k8s.io
```

`platform/team/serviceaccount.yaml`
```yaml
---
apiVersion: v1
kind: ServiceAccount
metadata:
  name: -sa
```

## Wrap your resources into a kustomization

We will use Kustomize to patch the various attributes that are team specific or team/environments specific. Wrap your various base manifests into a `kustomization.yaml`

`platform/team/kustomization.yaml`
```yaml
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization
resources:
  - namespace.yaml
  - repository.yaml
  - serviceaccount.yaml
  - role.yaml
  - rolebinding.yaml
  - applications.yaml
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
```