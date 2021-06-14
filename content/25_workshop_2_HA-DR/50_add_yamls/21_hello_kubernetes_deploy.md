+++
title = "Deploy the Hello Kubernetes Application"
chapter = false
weight = 21
+++

One of the principles of GitOps states that the only way to interact with a cluster should be by modifying the state declared in code. Following that principle means we will not be performing any action with `kubectl` or otherwise interacting with either of our clusters directly.

You are now going to deploy the `Hello Kubernetes` application and associated resources in both of your clusters, by pushing manifests into your repository.

Create a file called `clusters/eks-ha/hello-kubernetes/application.yaml` in your git repository, and paste the following contents:

```yaml
---
apiVersion: v1
kind: Namespace
metadata:
  name: hello-kubernetes
---
apiVersion: v1
kind: ServiceAccount
metadata:
  name: hello-kubernetes-eks-ha
  namespace: hello-kubernetes
  labels:
    app.kubernetes.io/name: hello-kubernetes
    app.kubernetes.io/instance: eks-ha
    app.kubernetes.io/version: "1.10"
---
apiVersion: v1
kind: Service
metadata:
  name: hello-kubernetes-eks-ha
  namespace: hello-kubernetes
  labels:
    app.kubernetes.io/name: hello-kubernetes
    app.kubernetes.io/instance: eks-ha
    app.kubernetes.io/version: "1.10"
spec:
  type: ClusterIP
  ports:
    - port: 80
      targetPort: http
      protocol: TCP
      name: http
  selector:
    app.kubernetes.io/name: hello-kubernetes
    app.kubernetes.io/instance: eks-ha
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: hello-kubernetes-eks-ha
  namespace: hello-kubernetes
  labels:
    app.kubernetes.io/name: hello-kubernetes
    app.kubernetes.io/instance: eks-ha
    app.kubernetes.io/version: "1.10"
spec:
  replicas: 2
  selector:
    matchLabels:
      app.kubernetes.io/name: hello-kubernetes
      app.kubernetes.io/instance: eks-ha
  template:
    metadata:
      labels:
        app.kubernetes.io/name: hello-kubernetes
        app.kubernetes.io/instance: eks-ha
    spec:
      serviceAccountName: hello-kubernetes-eks-ha
      containers:
        - name: hello-kubernetes
          image: "paulbouwer/hello-kubernetes:1.10"
          imagePullPolicy: IfNotPresent
          ports:
            - name: http
              containerPort: 8080
              protocol: TCP
          livenessProbe:
            httpGet:
              path: /
              port: http
          readinessProbe:
            httpGet:
              path: /
              port: http
          env:
          - name: HANDLER_PATH_PREFIX
            value: ""
          - name: RENDER_PATH_PREFIX
            value: ""
          - name: KUBERNETES_NAMESPACE
            valueFrom:
              fieldRef:
                fieldPath: metadata.namespace
          - name: KUBERNETES_POD_NAME
            valueFrom:
              fieldRef:
                fieldPath: metadata.name
          - name: KUBERNETES_NODE_NAME
            valueFrom:
              fieldRef:
                fieldPath: spec.nodeName
          - name: CONTAINER_IMAGE
            value: "paulbouwer/hello-kubernetes:1.10"
      nodeSelector:
        kubernetes.io/arch: amd64
        kubernetes.io/os: linux
---
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: hello-kubernetes-ingress
  namespace: hello-kubernetes
  annotations:
    ingress.kubernetes.io/rewrite-target: /
spec:
  rules:
  - http:
      paths:
        - path: /
          backend:
            serviceName: hello-kubernetes-eks-ha
            servicePort: 80
```