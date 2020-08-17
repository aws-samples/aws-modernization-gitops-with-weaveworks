+++
title = "Expose the Podinfo Pod Service"
chapter = false
weight = 23
+++

In order for any Kubernetes object, including other pods, to access the `podinfo` pod, we have to expose on the Kubernetes network the network ports that should be accessed. When using the **Application Load Balancer** on EKS, the service type should be `NodePort`. In more complex architectures, there are other load balancing methods that can be used.

You can create a NodePort Service yaml (as specified by the ALB ingress controller) by running the following command and adding the manifest to your repository as `podinfo-service.yaml`:

```sh
kubectl create service nodeport podinfo --namespace podinfo --tcp=9898:9898  --dry-run -o yaml > podinfo-service.yaml
```

Alternatively, you can copy and paste the service as defined below to a new file called `podinfo-service.yaml`:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: podinfo
  namespace: podinfo
spec:
  type: NodePort
  selector:
    app: podinfo
  ports:
    - name: http
      port: 9898
      protocol: TCP
```
