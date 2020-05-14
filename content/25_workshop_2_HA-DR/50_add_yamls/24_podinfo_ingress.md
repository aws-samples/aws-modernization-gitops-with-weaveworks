+++
title = "Connect the Podinfo Service to the Ingress Controller"
chapter = false
weight = 24
+++

The final step is to inform the **ALB Ingress Controller** that a service needs to be exposed externally from the cluster. The ingress controller watches for the creation of **Ingress** objects, and then creates the routing required to reach the object externally.

In the **Ingress** object definition, ther eare annotations to indicate how the **Ingress Controller** should handle this service, as well as which controller to utilize. As there may be several different ingress controllers in a single Kubernetes cluster, this ability is key to managing access to multiple applications and microservices.

Ther is also a `path` parameter in this definition. The `path` parameter designates the external URL (URI) that would be required to reach this service. Each path for each specific ingress controller must be unique. For simplicity, we have mapped the root URI ("/") to the service directly.

Create a file called **podinfo-ingress.yaml** in your git repository containing:
```
apiVersion: networking.k8s.io/v1beta1
kind: Ingress
metadata:
  name: podinfo-ingress
  namespace: podinfo
  annotations:
    kubernetes.io/ingress.class: alb
    alb.ingress.kubernetes.io/scheme: internet-facing

spec:
  rules:
  - http:
      paths:
      - path: /*
        backend:
          serviceName: podinfo
          servicePort: 9898
```
