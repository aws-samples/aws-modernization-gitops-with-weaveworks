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

In the Scope: 

- Navigate to one of the process group, for example the front-end (<i>Technologies -> Node.js -> server.js -> Process group details</i>)
- Expand the properties. 
- The imported Kubernetes <b>labels</b> will show as <b>tags</b> and the <b>annotations</b> as <b>properties</b>.

![pg_labels_annotations](/images/pg_labels_annotations.png)

## Search/filter with tags based on labels

- Go in the <i>Technologies</i> or the <i>Services and Transactions</i> view. 

- In the Filtered by text box, you will see the available tags that you can select.

![filter_by_tag](/images/filter_by_tag.png)

You can also perform searches for label values in the Super Search box!

You will see that not only processes are showing up but services too. This is because the labels are automatically propagated from the process (container) entity to the services implemented by the process!

- Select and drill-down the service to see the labels attached as tags.

![super_search_box_tag](/images/super_search_box_tag.png)