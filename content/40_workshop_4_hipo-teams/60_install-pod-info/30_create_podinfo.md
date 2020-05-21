+++
title = "Create PodInfo"
chapter = false
weight = 30
+++

We first need to install the 3 different instances of PodInfo:

* Frontend
* Backend v1
* Backend v2

Create a `apps` folder in our repo:

```bash
.
├── amazon-cloudwatch
│   ├── cwagent-fluentd-quickstart.yaml
│   └── cwagent-prometheus-eks.yaml
├── appmesh-system
│   ├── appmesh-controller.yaml
│   ├── appmesh-inject.yaml
│   ├── appmesh-prometheus.yaml
│   └── crds.yaml
├── apps
├── namespaces
│   ├── amazon-cloudwatch.yaml
│   ├── appmesh-system.yaml
│   └── apps.yaml
└── README.md
```

Now download the yaml file for the deployments and services:

```bash
curl https://weaveworks-gitops.awsworkshop.io/40_workshop_4_hipo-teams/60_install-pod-info/deploy.files/1-podinfo.yaml -o apps/1-podinfo.yaml
```

Add and then commit the **1-podinfo.yaml** file and push the the changes to your GitHub repo.

Flux will now see that the desired state of the `apps` namespace has changed in Git and will apply the resources to our cluster. This will take up to 1 minute to apply.

Check that that the deployment and services has been created by running the following command:

```bash
kubectl get pods,svc -n apps
```

You should see the pods running and services:

```bash
NAME                                      READY   STATUS    RESTARTS   AGE
pod/backend-podinfo-v1-5454d76d7f-66cg7   2/2     Running   0          56s
pod/backend-podinfo-v2-74f967bf8b-4lb28   2/2     Running   0          56s
pod/frontend-podinfo-54f9995b4f-r668p     2/2     Running   0          56s

NAME                         TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)             AGE
service/backend-podinfo-v1   ClusterIP   10.100.52.43    <none>        9898/TCP,9999/TCP   56s
service/backend-podinfo-v2   ClusterIP   10.100.34.160   <none>        9898/TCP,9999/TCP   56s
service/frontend-podinfo     ClusterIP   10.100.73.93    <none>        9898/TCP,9999/TCP   56s
```

{{% notice note %}}
You'll see that there are 2 containers running in each of the PodInfo pods. This is denoted by the **2/2** in the **READY** column. If you look at the **1-podinfo.yaml** file you'll see that there is only 1 container declared (i.e. podinfo). The AWS App Mesh Injector has injected a sidecar container into the deployment when Flux applied the yaml. You can verify this by doing a `kubectl describe` on one of the pods.
{{% /notice %}}
