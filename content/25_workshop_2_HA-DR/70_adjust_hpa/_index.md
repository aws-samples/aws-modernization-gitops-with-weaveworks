+++
title = "Adjust HPA Settings"
chapter = true
weight = 70
+++
A Horizontal Pod Autoscaler would normally be triggered by the thresholds set in the manifest. However, there are times when you know that the minimum (or maximum) number of pods should be adjusted. This is done very easily by editing the `clusters/eks-ha/hello-kubernetes/hpa.yaml` file you created earlier.

Edit the file, and change the number of replicas to something different:

<pre>
  minReplicas: 3
  maxReplicas: 6
</pre>

Don't forget to commit and push the changes. After a few minutes, check the clusters to see your changes reflected.

```
kubectl get pods -n hello-kubernetes
```

If you can generate enough traffic, you should also be able to see the number of replicas scale up.
