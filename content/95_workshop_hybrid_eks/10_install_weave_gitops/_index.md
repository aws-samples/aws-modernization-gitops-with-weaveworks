+++
title = "Install Weave GitOps"
chapter = true
weight = 10
+++

# Install Weave GitOps

## Create a values files with your credentials

Save it as `weave-gitops-values.yaml`

```yaml
adminUser:
  create: true
  username: admin
  passwordHash: $2y$10$zTRdq9bLcEmGF27exGcKZ.LnSNIOpwV.n5H7tLP4/oyuSRGjTk7Ai
```

## Add the Weave GitOps Helm Repo to your Git Repository

```shell
flux create source helm ww-gitops \
  --url=https://helm.gitops.weave.works \
  --export > ./clusters/workshop/weave-gitops-source.yaml
```

Commit and push this to your repository

```shell
git commit -am "Adding Weave GitOps Helm repo"
git push
```

## Install Weave GitOps Helm Release

```shell
flux create helmrelease ww-gitops \
  --source=HelmRepository/ww-gitops \
  --chart=weave-gitops \
  --values=./weave-gitops-values.yaml \
  --export > ./clusters/workshop/weave-gitops-helmrelease.yaml
```

Commit and push this to your repository

```shell
git commit -am "Adding Weave GitOps Helm Release"
git push
```

## Expose the Weave GitOps UI

Create a file named `./clusters/workshop/weave-gitops-external.yaml`

```yaml
apiVersion: v1
kind: Service
metadata:
  name: ww-gitops-weave-gitops-external
  namespace: flux-system
spec:
  ports:
  - name: http
    port: 9001
    protocol: TCP
    targetPort: http
  selector:
    app.kubernetes.io/instance: ww-gitops
    app.kubernetes.io/name: weave-gitops
  sessionAffinity: None
  type: LoadBalancer
```

Commit and push to your repository

```shell
git commit -am "Exposing Weave GitOps with Load Balancer"
git push
```

## Confirm access to Weave GitOps UI

### Get the ELB URL

```shell
kubectl get svc ww-gitops-weave-gitops-external -n flux-system
NAME                              TYPE           CLUSTER-IP       EXTERNAL-IP                                                              PORT(S)          AGE
ww-gitops-weave-gitops-external   LoadBalancer   10.100.223.168   a144849f871e9405ab7024b276f80e74-240529350.us-east-1.elb.amazonaws.com   9001:30164/TCP   21h
```

Use the URL shown under EXTERNAL-IP to go to the Weave GitOps UI using port 9001. Use `admin` for username and password.