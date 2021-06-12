+++
title = "GitOps Enable Clusters"
chapter = true
weight = 40
+++

# GitOps Enable the Two EKS Clusters

Having created two clusters, you will now have two different contexts available for `kubectl`, you should be careful with selecting the right context as you proceed enabling GitOps on each of them.

Running the command `kubectl config get-contexts` will show you the available contexts and which is currently active, as marked by the `*` in the first column.

<pre>
AdministratorAccess:~/environment $ kubectl config get-contexts
CURRENT   NAME                                                           CLUSTER                                    AUTHINFO                                                       NAMESPACE
*         i-041b36df51a73d3da@aws-workshop-cluster.us-east-2.eksctl.io   aws-workshop-cluster.us-east-2.eksctl.io   i-041b36df51a73d3da@aws-workshop-cluster.us-east-2.eksctl.io   
          i-041b36df51a73d3da@aws-workshop-cluster.us-west-1.eksctl.io   aws-workshop-cluster.us-west-1.eksctl.io   i-041b36df51a73d3da@aws-workshop-cluster.us-west-1.eksctl.io   
</pre>

You should always make sure you are paying attention to which cluster you are interacting with when using `kubectl` or the `flux` CLI.