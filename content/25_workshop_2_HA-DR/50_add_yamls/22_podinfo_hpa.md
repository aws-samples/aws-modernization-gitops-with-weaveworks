---
title: "Enable the Podinfo Pod to Scale Automatically"
chapter: false
weight: 22
---
A very useful feature in Kubernetes is to enable auto-scaling. This is accomplished by created a Horizontal Pod Autoscaler (HPA) object that is targeted at a specific pod. Each HPA has criteria that Kubernetes uses in order add pods to the running deployment. This HPA will scale the podinfo deployment to a minimum of 2 replicas, and a maximum of 4 replicas.

Create a file called **podinfo-hpa.yaml** in your git repository:
```
apiVersion: autoscaling/v2beta1
kind: HorizontalPodAutoscaler
metadata:
  name: podinfo
  namespace: podinfo
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: podinfo
  minReplicas: 2
  maxReplicas: 4
  metrics:
  - type: Resource
    resource:
      name: cpu
      # scale up if usage is above
      # 99% of the requested CPU (100m)
      targetAverageUtilization: 99
```
