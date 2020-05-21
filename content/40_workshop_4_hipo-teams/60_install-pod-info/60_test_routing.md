+++
title = "Test the routing"
chapter = false
weight = 60
+++

Now lets test the routing of requests to the backend **PodInfo** services. According to the routing rules we created request should be split roughly 50:50 between v1 and v2.

We'll need to exec onto the frontend container:

```bash
# Get the frontend pods name
export FRONTEND_NAME=$(kubectl get pods -n apps -l app=frontend-podinfo -o jsonpath='{.items[].metadata.name}')

# Exec onto the pod
kubectl -n apps exec -it ${FRONTEND_NAME} -- sh
```

We'll use curl to make a request to the `backend-podinfo.apps.svc.cluster.local` virtual service:

```bash
curl backend-podinfo.apps.svc.cluster.local:9898
```

When we run this command **AWS App Mesh** will take into account the virtual routes we have configured and route the request accordingly. You should see output similar to this:

```bash
{
  "hostname": "backend-podinfo-v2-74f967bf8b-57xfx",
  "version": "3.3.1",
  "revision": "b45cc753291a9bfb9604a596ae709dd775f52a98",
  "color": "#34577c",
  "logo": "https://raw.githubusercontent.com/stefanprodan/podinfo/gh-pages/cuddle_clap.gif",
  "message": "greetings from podinfo v3.3.1",
  "goos": "linux",
  "goarch": "amd64",
  "runtime": "go1.14.2",
  "num_goroutine": "8",
  "num_cpu": "2"
}
```

{{% notice note %}}
Notice that you can see which version of the backend has been called by looking at the `hostname` in the output. In the example above it has been routed to **v2**.
{{% /notice %}}

We will now make requests in a loop to ensure that both versions of the backend service are being called. Whilst still exec'd into the container run the following command:

```bash
while true; do curl backend-podinfo.apps.svc.cluster.local:9898; echo; sleep .5; done
```

You should see from the output that **hostname** indicates that both **v1** and **v2** are being called. And its roughly even between the 2 versions.

__Leave this command running__.
