+++
title = "Automated Canary Rollback"
chapter = false
weight = 403
+++

Now we will generate some failed requests to trigger an automated rollback.

During the canary analysis you can generate HTTP 500 errors and high latency to test if Flagger pauses and rolls back the faulted version.

Trigger another canary release:

```sh
cat << EOF | tee overlays/podinfo.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: podinfo
  namespace: demo
spec:
  template:
    spec:
      containers:
        - name: podinfod
          image: stefanprodan/podinfo:3.1.2
          env:
            - name: PODINFO_UI_LOGO
              value: https://eks.handson.flagger.dev/cuddle_bunny.gif
EOF
```

Apply changes:

```sh
git add -A && \
git commit -m "update podinfo" && \
git push origin master && \
fluxctl sync --k8s-fwd-ns flux
```

Watch Flagger logs with:

```sh
kubectl -n appmesh-system logs deployment/flagger -f | jq .msg
```

Exec into the tester pod and generate HTTP 500 errors:

```sh
kubectl -n demo exec -it $(kubectl -n demo get pods -o name | grep -m1 flagger-loadtester | cut -d'/' -f 2) bash

hey -z 1m -c 5 -q 5 http://podinfo-canary.demo:9898/status/500
hey -z 1m -c 5 -q 5 http://podinfo-canary.demo:9898/delay/1
```

When the number of failed checks reaches the canary analysis threshold, the traffic is routed back to the primary and the canary is scaled to zero.

Watch Flagger logs with:

```sh
kubectl -n appmesh-system logs deployment/flagger -f | jq .msg
```

You should see the following:

```sh
 Starting canary analysis for podinfo.prod
 Advance podinfo.test canary weight 5
 Advance podinfo.test canary weight 10
 Advance podinfo.test canary weight 15
 Halt podinfo.test advancement success rate 69.17% < 99%
 Halt podinfo.test advancement success rate 61.39% < 99%
 Halt podinfo.test advancement success rate 55.06% < 99%
 Halt podinfo.test advancement request duration 1.20s > 0.5s
 Halt podinfo.test advancement request duration 1.45s > 0.5s
 Rolling back podinfo.prod failed checks threshold reached 5
 Canary failed! Scaling down podinfo.test
```
