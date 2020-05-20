+++
title = "Install Container Insights"
chapter = false
weight = 20
+++

As every namespace in our EKS cluster has a folder in our repo that contains the declarations of the resources we want to live in that namespace we will need a new folder for the **amazon-cloudwatch** namespace.

Create a `amazon-cloudwatch` folder in our repo:

```bash
.
├── amazon-cloudwatch
├── appmesh-system
│   ├── appmesh-controller.yaml
│   ├── appmesh-inject.yaml
│   ├── appmesh-prometheus.yaml
│   └── crds.yaml
├── namespaces
│   ├── amazon-cloudwatch.yaml
│   └── appmesh-system.yaml
└── README.md
```

Now run the following commands to download the CloudWatch Agent resources into the new folder, make sure you replace **<AWS_REGION>** with the name AWS region of your cluster:

```bash
curl https://raw.githubusercontent.com/aws-samples/amazon-cloudwatch-container-insights/latest/k8s-deployment-manifest-templates/deployment-mode/daemonset/container-insights-monitoring/quickstart/cwagent-fluentd-quickstart.yaml | sed "s/{{cluster_name}}/eksworkshop/;s/{{region_name}}/<AWS_REGION>/" > amazon-cloudwatch/cwagent-fluentd-quickstart.yaml
```

Your folder structure should look like this now:

```bash
.
├── amazon-cloudwatch
│   └── cwagent-fluentd-quickstart.yaml
├── appmesh-system
│   ├── appmesh-controller.yaml
│   ├── appmesh-inject.yaml
│   ├── appmesh-prometheus.yaml
│   └── crds.yaml
├── namespaces
│   ├── amazon-cloudwatch.yaml
│   └── appmesh-system.yaml
└── README.md
```

Edit the **cwagent-fluentd-quickstart** and delete the following (which should appear at the beginning):

```yaml
# create amazon-cloudwatch namespace
apiVersion: v1
kind: Namespace
metadata:
  name: amazon-cloudwatch
  labels:
    name: amazon-cloudwatch

---
```

Add and then commit the **cwagent-fluentd-quickstart.yaml** file and push the the changes to your GitHub repo.

Flux will now see that the desired state of the `amazon-cloudwatch` namespace has changed in Git and will apply the resources to our cluster. This will take up to 1 minute to apply.

Check that that the CRDs has been created by running the following command:

```bash
kubectl get pods -n amazon-cloudwatch
```

You should see the agent running:

```bash
NAME                                  READY   STATUS    RESTARTS   AGE
cwagent-prometheus-75dfcd47d7-h24pd   1/1     Running   0          21s
```
