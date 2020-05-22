+++
title = "Create App Mesh Resources"
chapter = false
weight = 40
+++

We are now going to the the following App Mesh resources for our PodInfo application:

* Virtual Nodes - for our existing PodInfo services (physical) and also for our new "virtual" backend service
* Virtual Service - defines a new "virtual" service for the backend that will distribute traffic between the existing v1 and v2 backend PodInfo services/pods

To do this download the yaml files into the **apps** folder:

```bash
# Virtual nodes
curl https://weaveworks-gitops.awsworkshop.io/40_workshop_4_hipo-teams/60_install-pod-info/deploy.files/2-podinfo-virtual-nodes.yaml -o apps/2-podinfo-virtual-nodes.yaml

# Virtual Services
curl https://weaveworks-gitops.awsworkshop.io/40_workshop_4_hipo-teams/60_install-pod-info/deploy.files/3-podinfo-virtual-services.yaml -o apps/3-podinfo-virtual-services.yaml

# Placeholder services
curl https://weaveworks-gitops.awsworkshop.io/40_workshop_4_hipo-teams/60_install-pod-info/deploy.files/4-podinfo-placeholder-services.yaml -o apps/4-podinfo-placeholder-services.yaml
```

You folder structure should look similar to:

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
│   ├── 1-podinfo.yaml
│   ├── 2-podinfo-virtual-nodes.yaml
│   ├── 3-podinfo-virtual-services.yaml
│   └── 4-podinfo-placeholder-services.yaml
├── namespaces
│   ├── amazon-cloudwatch.yaml
│   ├── appmesh-system.yaml
│   └── apps.yaml
└── README.md
```

Add and then commit the 3 file and push the the changes to your GitHub repo.

Flux will now see that the desired state of the `apps` namespace has changed in Git and will apply the resources to our cluster. This will take up to 1 minute to apply.

Check that that the deployment and services has been created by running the following command:

```bash
kubectl get virtualservices,virtualnodes,svc -n apps
```

You should see the new resources:

```bash
virtualservice.appmesh.k8s.aws/backend-podinfo.apps.svc.cluster.local   26s

NAME                                             AGE
virtualnode.appmesh.k8s.aws/backend-podinfo      26s
virtualnode.appmesh.k8s.aws/backend-podinfo-v1   26s
virtualnode.appmesh.k8s.aws/backend-podinfo-v2   26s
virtualnode.appmesh.k8s.aws/frontend-podinfo     26s

NAME                         TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)             AGE
service/backend-podinfo      ClusterIP   10.100.39.139   <none>        9898/TCP            26s
service/backend-podinfo-v1   ClusterIP   10.100.52.43    <none>        9898/TCP,9999/TCP   3m29s
service/backend-podinfo-v2   ClusterIP   10.100.34.160   <none>        9898/TCP,9999/TCP   3m29s
service/frontend-podinfo     ClusterIP   10.100.73.93    <none>        9898/TCP,9999/TCP   3m29s
```
