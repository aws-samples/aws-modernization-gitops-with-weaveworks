+++
title = "Automated Canary Promotion"
chapter = false
weight = 402
+++

When you deploy a new `podinfo` version, Flagger gradually shifts traffic to the canary. At the same time, Flagger measures the requests success rate as well as the average response duration. Based on an analysis of these App Mesh provided metrics, a canary deployment is either promoted or rolled back.

Run the following command to create a Kustomize patch for the `podinfo` deployment in `overlays/podinfo.yaml`:

```sh
mkdir -p overlays && \
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
          image: stefanprodan/podinfo:3.1.1
          env:
            - name: PODINFO_UI_LOGO
              value: https://eks.handson.flagger.dev/cuddle_bunny.gif
EOF
```

Add the patch to the kustomization file as such:

```sh
cat << EOF | tee kustomization.yaml
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization
bases:
  - base
  - flux
patchesStrategicMerge:
  - overlays/podinfo.yaml
EOF
```

Push the changes to your repository and apply using `fluxctl`:

```sh
git add -A && \
git commit -m "patch podinfo" && \
git push origin master && \
fluxctl sync --k8s-fwd-ns flux
```

When Flagger detects that the deployment revision changed it will start a new rollout. You can monitor the traffic shifting with:

```sh
kubectl -n demo get canaries --watch
```

Watch Flagger logs:

```sh
kubectl -n appmesh-system logs deployment/flagger -f | jq .msg
```

As we watch the weight of traffic increase for upgraded podinfo, we can expect to see our site change accordingly. As Flagger shifts more traffic to the canary according to the policy in the Canary object, we see requests going to our new version of the app.
