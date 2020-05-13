---
title: "Configure Kubernetes RBAC"
date: 2020-05-12T18:00:00-00:00
draft: false
weight: 30
---

You can refere to [intro to RBAC](https://eksworkshop.com/beginner/090_rbac/) module to understand the basic of Kubernetes RBAC.

#### Create kubernetes namespaces

* **development** namespace will be accessible for IAM users from **k8sDev** group
* **integration** namespace will be accessible for IAM users from **k8sInteg** group

```bash
mkdir integration
cat << EOF > integration/integration-ns.yaml
apiVersion: v1
kind: Namespace
metadata:
  name: integration
EOF

mkdir development
cat << EOF > development/development-ns.yaml
apiVersion: v1
kind: Namespace
metadata:
  name: development
EOF
```

#### Configuring access to development namespace

We create a kubernetes role and rolebinding in the development namespace giving full access to the kubernetes user **dev-user**

```bash
mkdir development/roles

cat << EOF > ./roles/dev-role.yaml
kind: Role
apiVersion: rbac.authorization.k8s.io/v1beta1
metadata:
  name: dev-role
  namespace: development
rules:
  - apiGroups:
      - ""
      - "apps"
      - "batch"
      - "extensions"
    resources:
      - "configmaps"
      - "cronjobs"
      - "deployments"
      - "events"
      - "ingresses"
      - "jobs"
      - "pods"
      - "pods/attach"
      - "pods/exec"
      - "pods/log"
      - "pods/portforward"
      - "secrets"
      - "services"
    verbs:
      - "create"
      - "delete"
      - "describe"
      - "get"
      - "list"
      - "patch"
      - "update"
---
kind: RoleBinding
apiVersion: rbac.authorization.k8s.io/v1beta1
metadata:
  name: dev-role-binding
  namespace: development
subjects:
- kind: User
  name: dev-user
roleRef:
  kind: Role
  name: dev-role
  apiGroup: rbac.authorization.k8s.io
EOF
```

The role we define will give full access to everything in that namespace. It is a Role, and not a ClusterRole, so it is going to be applied only in the **development** namespace.

> feel free to adapt or duplicate to any namespace you prefer.

#### Configuring access to integration namespace

We create a kubernetes role and rolebinding in the integration namespace for full access with the kubernetes user **integ-user**


```bash
mkdir integration/roles
cat << EOF >| ./roles/integ-role.yaml
kind: Role
apiVersion: rbac.authorization.k8s.io/v1beta1
metadata:
  name: integ-role
  namespace: integration
rules:
  - apiGroups:
      - ""
      - "apps"
      - "batch"
      - "extensions"
    resources:
      - "configmaps"
      - "cronjobs"
      - "deployments"
      - "events"
      - "ingresses"
      - "jobs"
      - "pods"
      - "pods/attach"
      - "pods/exec"
      - "pods/log"
      - "pods/portforward"
      - "secrets"
      - "services"
    verbs:
      - "create"
      - "delete"
      - "describe"
      - "get"
      - "list"
      - "patch"
      - "update"
---
kind: RoleBinding
apiVersion: rbac.authorization.k8s.io/v1beta1
metadata:
  name: integ-role-binding
  namespace: integration
subjects:
- kind: User
  name: integ-user
roleRef:
  kind: Role
  name: integ-role
  apiGroup: rbac.authorization.k8s.io
EOF
```

The role we define will give full access to everything in that namespace. It is a Role, and not a ClusterRole, so it is going to be applied only in the **integration** namespace.


## Check this in to git!

run the following to add our changes:

```bash
git add "integration/integration-ns.yaml"

git add "development/development-ns.yaml"

git commit -m "adding dev adn int namespaces"

git add "integration/roles/"

git add "development/roles/"

git commit -m "adding int & dev roles"

git push
```


