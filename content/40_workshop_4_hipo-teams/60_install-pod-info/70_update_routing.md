+++
title = "Update the routing"
chapter = false
weight = 70
+++

We are happy that **v2** of the backend service is behaving as expected and want to switch 100% of the traffic to it.

Edit the `apps/3-podinfo-virtual-services.yaml` file so it looks like this:

```bash
apiVersion: appmesh.k8s.aws/v1beta1
kind: VirtualService
metadata:
  name: backend-podinfo.apps.svc.cluster.local
  namespace: apps
spec:
  meshName: apps
  virtualRouter:
    name: backend-podinfo-router
  routes:
    - name: podinfo-route
      http:
        match:
          prefix: /
        action:
          weightedTargets:
            - virtualNodeName: backend-podinfo-v2
              weight: 100
```

{{% notice note %}}
We have removed the target for **v1** and updated the target for **v2** to be 100.
{{% /notice %}}

Open a new terminal window in **Cloud9**  (make sure you leave the other terminal window running the query).

Use the second terminal window to add and then commit **3-podinfo-virtual-services.yaml**. Push the the changes to your GitHub repo. Flux will pick this change up and apply it.

Go back to the original terminal window. In about a minutes you will see the output change and the **hostname** will always show **v2**.

