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
NAMESPACE          NAME                       CLASS    HOSTS   ADDRESS                                                                         PORTS   AGE
hello-kubernetes   hello-kubernetes-ingress   <none>   *       a94ff1031fbaf4a7e98124de4f1cfe48-541c97dd6045340c.elb.us-east-2.amazonaws.com   80      11h
</pre>

Now, using your browser, access each of you ingresses with HTTP. From the above, the URL would be:

<pre>
http://a94ff1031fbaf4a7e98124de4f1cfe48-541c97dd6045340c.elb.us-east-2.amazonaws.com
</pre>

You should see the `Hello Kubernetes` status page. Refreshing the browser several times will show you that the `Pod` value will change as Kubernetes routes between your two `hello-kubernetes` pods.

![Hello Kubernetes](/images/hello-kubernetes.png)



