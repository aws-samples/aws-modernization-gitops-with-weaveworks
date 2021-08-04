+++
title = "Onboard your clusters"
chapter = true
weight = 30
+++

# Onboard your Clusters

Now that we have our repos structured and ready to operate our hybrid cluster architecture, and both our clusters are running Weave GitOps controllers, let's onboard the clusters using Weave GitOps, by having them track and reconcile against our desired, environment specific path.

## Onboard the `development` cluster

Make sure that you are using the managed EKS cluster context, it should be marked as `Current` if you run `kubectl config get-contexts`, switch to it if not current by using `kubectl config use-context development`

```shell
cd ~/environment/team-ssp
wego app add . --path=./environments/dev --name=development --branch=main
```

The command will generate a new Pull Request for your, you can see a link to that PR in the output of the command, go to that URL, inspects the changes and merge the PR.

<pre>
Adding application:

Name: development
URL: ssh://git@github.com/murillodigital/team-ssp.git
Path: ./environments/dev
Branch: main
Type: kustomize

◎ Checking cluster status
✔ Wego installed
✚ Generating deploy key for repo ssh://git@github.com/murillodigital/team-ssp.git
uploading deploy key
✚ Generating Source manifest
✚ Generating GitOps automation manifests
✚ Generating Application spec manifest
Pull Request created: https://github.com/murillodigital/team-ssp/pull/1

► Applying manifests to the cluster
</pre>

Wait a few seconds for the cluster to reconcile, you can now do a quick test to confirm the namespaces for your teams have been created, you can also dig deeper and inspect other team resources such as service accounts and roles:

<pre>
$ kubectl get ns
NAME              STATUS   AGE
blue-ns           Active   3m38s
default           Active   4h51m
green-ns          Active   3m38s
kube-node-lease   Active   4h51m
kube-public       Active   4h51m
kube-system       Active   4h51m
wego-system       Active   108m
</pre>

## Onboard the `production` cluster

Now switch over to the `production` context to manage your EKS-D production cluster, and onboard the production environment following the identical process, except changing the path we want our cluster to track and using the appropriate name for the environment.

```shell
kubectl config use-context production
wego app add . --path=./environments/prod --name=production --branch=main
```

