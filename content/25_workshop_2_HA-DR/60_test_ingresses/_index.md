+++
title = "Test Ingress Settings"
chapter = true
weight = 60
+++
In each of your terminal windows, execute:
```
kubectl get ingress -A
```
You should see output similar to this:
<pre>
NAMESPACE   NAME              HOSTS   ADDRESS                                                                  PORTS   AGE
podinfo     podinfo-ingress   *       e737fb32-podinfo-podinfoin-2c89-1571962006.us-east-2.elb.amazonaws.com   80      4d18h
</pre>
Now, using your browser, access each of you ingresses with HTTP. From the above, the URL would be:
<pre>
http://e737fb32-podinfo-podinfoin-2c89-1571962006.us-east-2.elb.amazonaws.com
</pre>
You should see the podinfo index status page. Refreshing the browser several times will show you that the `Served by` will change as Kubernetes routes between your two `podinfo` pods.

![Event Engine](/images/podinfo_screen.png)



