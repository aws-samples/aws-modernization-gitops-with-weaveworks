+++
title = "Recreate"
chapter = false
weight = 501
+++

When upgrading pods using the [recreate strategy](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/#recreate-deployment), existing pods are terminated before new ones are created.

To use the recreate strategy, define the Deployment yaml as such:

```yaml
apiVersion: apps/v1
kind: Deployment
spec:
  replicas: 2
  strategy:
    type: Recreate

```

The Deployment as defined above will create two replica pods:

{{<mermaid>}}
graph LR;
    A{Deployment} --> B(Replica 1, V1)
    A --> C(Replica 2, V1)
{{</mermaid>}}

The recreate strategy will terminate both replicas. During this time, the service is down and will not be able to handle traffic:

{{<mermaid>}}
graph LR;
    A{Deployment} --> |Terminating| B(Replica 1, V1)
    A --> |Terminating| C(Replica 2, V1)
{{</mermaid>}}

The upgraded replicas will then move into the Pending and ContainerCreating. During this time, the service is unable to handle traffic:

{{<mermaid>}}
graph LR;
    A{Deployment} -->|Pending, ContainerCreating| B(Replica 1, V2)
    A --> |Pending, ContainerCreating| C(Replica 2, V2)
{{</mermaid>}}

Once the upgraded replicas have come up successfully, the pods are ready to handle traffic.

{{<mermaid>}}
graph LR;
    A{Deployment} --> B(Replica 1, V2)
    A --> C(Replica 2, V2)
{{</mermaid>}}

### Benefits of the Recreate Strategy

* Avoids versioning issues
* Avoids database schema incompatibilities

### Caveats for the Recreate Strategy

* Involves downtime between V1 complete shutdown and V2 startup

### Suitable Use Cases

* Monolithic legacy applications
* Non production environments
* Workers in a queue based system
