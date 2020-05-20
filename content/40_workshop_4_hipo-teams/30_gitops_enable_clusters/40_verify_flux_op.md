+++
title = "Verify Flux and Helm are Operational"
chapter = false
weight = 40
+++

Now we have installed Flux and Helm Operator we need to get that they are both operational.

Run the following command in your **Cloud9** terminal:

```bash
kubectl get pods -n flux
```

You should see that there are 3 pods and the status should be **Running**:

```bash
NAME                             READY   STATUS    RESTARTS   AGE
flux-5884545b8c-thfxh            1/1     Running   0          22m
flux-memcached-97fc488-zwn8w     1/1     Running   0          22m
helm-operator-6489b5cc6b-5pdvl   1/1     Running   0          7m21s
```

{{% notice warning %}}
If the status isn't running then don't proceed with the workshop until its fixed. Please ask for help.
{{% /notice %}}
