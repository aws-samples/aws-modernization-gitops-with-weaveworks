+++
title = "Setup Kubernetes labels"
chapter = false
weight = 30
+++

## Explore metadata in pod definitions

List all the Sock Shop pods running:

```sh
kubectl get po -l product=sockshop --all-namespaces 
```

Pick up a pod and a namespace (`production` or `dev`) and get the pods details, including the <b>Labels</b> and the <b>Annotations</b>. 

```sh
kubectl describe po <pod_name> -n <namespace>
```

![pod_describe](/images/pod_describe.png)

## Grant viewer role to service accounts

Those Labels and Annotations are centrally defined and managed in Kubernetes but we also want them available in Weaveworks for grouping and filtering purposes.

The OneAgent will use a pod <b>service account</b> to query for this metadata via the Kubernetes REST API.

The service accounts must be granted viewer role in order to have this access.

In the terminal, execute the following command to grant viewer role. This needs to be done for each <b>namespace</b>.

```sh
kubectl create rolebinding serviceaccounts-view --clusterrole=view --group=system:serviceaccounts:production --namespace=production
```

You can repeat the procedure for the `dev` namespace.

```sh
kubectl create rolebinding serviceaccounts-view --clusterrole=view --group=system:serviceaccounts:dev --namespace=dev
```

### Wait...

Wait a few minutes :grinning: seriously, let's take a 10 minutes break here

![keep_calm](/images/keep_calm.png)

