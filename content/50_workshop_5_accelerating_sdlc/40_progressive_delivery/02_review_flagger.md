+++
title = "Review Flagger"
chapter = false
weight = 402
+++

In `base/demo/podinfo/canary.yaml`, you'll see the specifications for Flagger to automatically increase traffic to new versions of the application. In this workshop, we will be demonstrating a successful canary release, as well mimicking a failed canary release.

Below is the Canary configuration:

```yaml
apiVersion: flagger.app/v1beta1
kind: Canary
metadata:
  name: podinfo
  namespace: demo
spec:
  provider: appmesh
  # the maximum time in seconds for the canary deployment
  # to make progress before it is rollback (default 600s)
  progressDeadlineSeconds: 60
  # deployment reference
  targetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: podinfo
  # HPA reference (optional)
  autoscalerRef:
    apiVersion: autoscaling/v2beta1
    kind: HorizontalPodAutoscaler
    name: podinfo
  service:
    # container port
    port: 9898
    # container port name (optional)
    # can be http or grpc
    portName: http
    # App Mesh Gateway external domain
    hosts:
      - "*"
    # App Mesh reference
    meshName: "gitopssdlcworkshop"
    # App Mesh retry policy (optional)
    retries:
      attempts: 3
      perTryTimeout: 1s
      retryOn: "gateway-error,client-error,stream-error"
  # define the canary analysis timing and KPIs
  analysis:
    # schedule interval (default 60s)
    interval: 10s
    # max number of failed metric checks before rollback
    threshold: 10
    # max traffic percentage routed to canary
    # percentage (0-100)
    maxWeight: 50
    # canary increment step
    # percentage (0-100)
    stepWeight: 5
    # App Mesh Prometheus checks
    metrics:
      - name: request-success-rate
        # minimum req success rate (non 5xx responses)
        # percentage (0-100)
        thresholdRange:
          min: 99
        interval: 1m
      - name: request-duration
        # maximum req duration P99
        # milliseconds
        thresholdRange:
          max: 500
        interval: 30s
    # testing (optional)
    webhooks:
        - name: acceptance-test
        type: pre-rollout
        url: http://flagger-loadtester.demo/
        timeout: 30s
        metadata:
            type: bash
            cmd: "curl -sd 'test' http://podinfo-canary.demo:9898/token | grep token"
        - name: load-test
        url: http://flagger-loadtester.demo/
        timeout: 5s
        metadata:
            cmd: "hey -z 1m -q 10 -c 2 http://podinfo-canary.demo:9898/"
```

Flagger will create the following Kubernetes resources on our behalf, as we have specified the `TargetRef` and `AutoscalerRef`:

```sh
# generated Kubernetes objects
deployment.apps/podinfo-primary
horizontalpodautoscaler.autoscaling/podinfo-primary
service/podinfo
service/podinfo-canary
service/podinfo-primary
```

If you run `kubectl get pods -n demo`, you will notice that you have `podinfo-primary` pods running. The primary deployment is treated as the stable release of your app. All traffic is routed to this version and the target deployment is scaled to zero (`kubectl get deployment podinfo -n demo`). Flagger will detect changes to the target deployment (including secrets and configmaps) and will perform a canary analysis before promoting the new version as primary.

Flagger will also create the following App Mesh resources on our behalf:

```sh
# generated App Mesh objects (Kubernetes Custom Resources)
virtualnode.appmesh.k8s.aws/podinfo
virtualnode.appmesh.k8s.aws/podinfo-canary
virtualnode.appmesh.k8s.aws/podinfo-primary
virtualservice.appmesh.k8s.aws/podinfo.test
virtualservice.appmesh.k8s.aws/podinfo-canary.test
```

`virtualnode.appmesh.k8s.aws` defines the logical pointer to a Kubernetes workload, and `virtualservice.appmesh.k8s.aws` defines the routing rules for a workload inside the mesh. After the bootstrap, the podinfo deployment will be scaled to zero and the traffic to `podinfo.demo` will be routed to the primary pods. During the canary analysis, the `podinfo-canary.demo` address can be used to target directly the canary pods.

You can inspect these resources for yourself by running:

`kubectl get virtualservice -n demo`

and

`kubectl get virtualnodes -n demo`

You can also view the Virtual Service, Virtual Route, and Virtual Nodes in the AWS Console under the App Mesh service.

To perform a canary analysis, Flagger uses the `analysis` definition to determine how long to run the canary for (running periodically until it reaches the maximum traffic weight or the number of iteration) before rolling out the new version as the primary. On each run, Flagger calls the webhooks, checks the metrics and if the failed checks threshold is reached, it will stop the analysis and roll back the canary. We will demonstrate this shortly.

Navigate to the following url (or just `echo $URL` if you are in the same terminal window you were in before):

```sh
export URL="http://$(kubectl -n demo get svc/appmesh-gateway -ojson | jq -r ".status.loadBalancer.ingress[].hostname")"
echo $URL
```

You can leave this up as we complete this workshop.
