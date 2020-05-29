+++
title = "Rolling Update"
chapter = false
weight = 502
+++

The [RollingUpdate](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/#rolling-update-deployment) deployment strategy ensures upgraded pods successfully come up before terminating older versions.

The following demonstrates an example of the RollingUpdate strategy. When using RollingUpdate, you can specify `maxUnavailable` to specify how many pods can go down as an update takes place. You can also specify `maxSurge` to specify how many pods can be created in addition to the desired replica count.

```yaml
apiVersion: apps/v1
kind: Deployment
spec:
  replicas: 2
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxSurge: 1
      maxUnavailable: 0
  minReadySeconds: 10
```

In the above sample Deployment yaml, two replicas are specified as desired number of pods.

{{<mermaid>}}
graph LR;
    A{Deployment} --> B(Replica 1, V1);
    style A color:red;
    style B color:blue;
        A --> C(Replica 2, V1);
{{</mermaid>}}

The V1 replica pods remain up while an upgraded pod attempts to come up, moving through the pending and container creating states. This meets the `maxUnavailable: 0` abd `maxSurge: 1` specifications.

{{<mermaid>}}
graph LR;
    A{Deployment} --> B(Replica 1, V1)
    A --> C(Replica 2, V1)
    A --> |Pending, ContainerCreating| D(Replica 1, V2)
{{</mermaid>}}

Once the upgraded V2 pod comes up, one of the old V1 pods can be terminated.

{{<mermaid>}}
graph LR;
    A{Deployment} --> |Terminating| B(Replica 1, V1)
    A --> C(Replica 2, V1)
    A --> D(Replica 1, V2)
{{</mermaid>}}

The upgrade continues to replace all the old pods. The second V2 pod attempts coming up.

{{<mermaid>}}
graph LR;
    A{Deployment} -->B(Replica 2, V1)
    A --> C(Replica 1, V2)
    A --> |Pending, ContainerCreating| D(Replica 2, V2)
{{</mermaid>}}

The remaining old V1 pod is then terminated.

{{<mermaid>}}
graph LR;
    A{Deployment} --> |Terminating| B(Replica 2, V1)
    A --> C(Replica 1, V2)
    A --> D(Replica 2, V2)
{{</mermaid>}}

Desired replica count is achieved with the upgraded pods.

{{<mermaid>}}
graph LR;
    A{Deployment} --> C(Replica 1, V2)
    A --> D(Replica 2, V2)
{{</mermaid>}}

### Benefits of the RollingUpdate Strategy

* Low risk due to readiness checks
* Gradual rollout with no downtime

### Drawbacks of the RollingUpdate Strategy

* Needs backwards compatibility between API versions and database migrations
* No control over the traffic during the rollout

### Suitable Use Cases

* Stateful applications
* Stateless microservices
