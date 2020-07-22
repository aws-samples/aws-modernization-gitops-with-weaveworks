+++
title = "Deploy a Podinfo Pod"
chapter = false
weight = 21
+++

First, let's create a namespace for our application (podinfo) to execute in. In each of the terminal sessions, execute:

```sh
kubectl create namespace podinfo
```

Yes, this can also be accomplished by placing a manifest in your git repository to create the namespace.

You can run:

```sh
kubectl create namespace podinfo --dry-run -o yaml > podinfo-namespace.yaml
```

Or paste this into a new file called `podinfo-namespace.yaml`.

```yaml
apiVersion: v1
kind: Namespace
metadata:
  creationTimestamp: null
  name: podinfo
```

Add, commit, and push this change to your repository. You can call `fluxctl sync --k8s-fwd-ns flux` to get flux to deploy the changes without waiting for the set poll window.

{{% notice tip %}}
We will be creating a number of files in your git repository for these next few steps. You can choose to create all the files first, then commit and push to the repository, or you can commit and push after each file creation. If you commit and push after each file creation, you can execute `kubectl get pods -A` to see what has happened.
{{% /notice %}}


Create a file called **podinfo-deployment.yaml** in your git repository:
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: podinfo
  namespace: podinfo
spec:
  minReadySeconds: 3
  revisionHistoryLimit: 5
  progressDeadlineSeconds: 60
  strategy:
    rollingUpdate:
      maxUnavailable: 0
    type: RollingUpdate
  selector:
    matchLabels:
      app: podinfo
  template:
    metadata:
      annotations:
        prometheus.io/scrape: "true"
        prometheus.io/port: "9797"
      labels:
        app: podinfo
    spec:
      containers:
      - name: podinfod
        image: stefanprodan/podinfo:3.2.3
        imagePullPolicy: IfNotPresent
        ports:
        - name: http
          containerPort: 9898
          protocol: TCP
        - name: http-metrics
          containerPort: 9797
          protocol: TCP
        - name: grpc
          containerPort: 9999
          protocol: TCP
        command:
        - ./podinfo
        - --port=9898
        - --port-metrics=9797
        - --grpc-port=9999
        - --grpc-service-name=podinfo
        - --level=info
        - --random-delay=false
        - --random-error=false
        env:
        - name: PODINFO_UI_COLOR
          value: "#34577c"
        livenessProbe:
          exec:
            command:
            - podcli
            - check
            - http
            - localhost:9898/healthz
          initialDelaySeconds: 5
          timeoutSeconds: 5
        readinessProbe:
          exec:
            command:
            - podcli
            - check
            - http
            - localhost:9898/readyz
          initialDelaySeconds: 5
          timeoutSeconds: 5
        resources:
          limits:
            cpu: 2000m
            memory: 512Mi
          requests:
            cpu: 100m
            memory: 64Mi
```
