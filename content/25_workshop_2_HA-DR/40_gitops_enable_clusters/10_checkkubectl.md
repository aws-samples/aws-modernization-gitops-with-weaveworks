+++
title = "Check kubectl"
chapter = false
weight = 10
+++

With two terminal windows open, you'll have to be careful when executing commands to ensure you are working with the correct cluster. In each terminal window, verify that you are connecting to the correct cluster by executing:
```
kubectl config get-contexts
```
This should produce out similar to this. It is difficult to see because the cluster names and credentials are long. You **may** have more than one line listed. However, the line that begins with the "*" is the current context (the cluster `kubectl` will be connecting to) for the `kubectl` command.
<pre>
CURRENT   NAME                                               CLUSTER                    AUTHINFO                                           NAMESPACE
*         paul.curtis@weave.works@ha-1.us-east-2.eksctl.io   ha-1.us-east-2.eksctl.io   paul.curtis@weave.works@ha-1.us-east-2.eksctl.io   
</pre>
In this case, the `kubectl` command will be executed on the `ha-1.us-east-2.eksctl.io` cluster.

Check in your two terminal windows to ensure that each terminal session is connecting to your two different clusters.:w
