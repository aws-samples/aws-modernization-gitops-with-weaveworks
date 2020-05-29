+++
title = "Install Podinfo demo app"
chapter = false
weight = 401
+++

In the `/base/demo` folder, you will find several components that will help us deploy our sample `podinfo` application.

Install the demo app by setting `fluxcd.io/ignore` to `false` in base/demo/namespace.yaml:

```sh
cat << EOF | tee base/demo/namespace.yaml
apiVersion: v1
kind: Namespace
metadata:
  name: demo
  annotations:
    fluxcd.io/ignore: "false"
  labels:
    appmesh.k8s.aws/sidecarInjectorWebhook: enabled
EOF
```

Push the changes to your git repository and apply using `fluxctl`:

```sh
git add -A && \
git commit -m "init demo" && \
git push origin master && \
fluxctl sync --k8s-fwd-ns flux
```

Wait for Flagger to initialize the canary:

```sh
kubectl get canary -n demo
```

Find the ingress public address with:

```sh
export URL="http://$(kubectl -n demo get svc/appmesh-gateway -ojson | jq -r ".status.loadBalancer.ingress[].hostname")"
echo $URL
```

In a few minutes or so, podinfo will be accessible via the public URL.

While we wait for podinfo to be accessible, let's review some of the components that Flagger creates / uses in order to canary your deployments on the next page.
