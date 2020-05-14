---
title: "Test EKS access"
date: 2020-05-05T18:00:00-00:00
draft: false
weight: 40
---

## Automate assumerole with aws cli

It is possible to automate the retrieval of temporary credentials for the assumed role by configuring the aws cli using `.aws/config`and `.aws/credentials` files.
Examples we will define 3 profile:

#### add in `~/.aws/config`:
```
cat << EoF >> ~/.aws/config
[profile admin]
role_arn=arn:aws:iam::${ACCOUNT_ID}:role/k8sAdmin
source_profile=eksAdmin

[profile dev]
role_arn=arn:aws:iam::${ACCOUNT_ID}:role/k8sDev
source_profile=eksDev

[profile integ]
role_arn=arn:aws:iam::${ACCOUNT_ID}:role/k8sInteg
source_profile=eksInteg

EoF
```

#### create `~/.aws/credentials`:

```
cat << EoF > ~/.aws/credentials

[eksAdmin]
aws_access_key_id=$(jq -r .AccessKey.AccessKeyId /tmp/PaulAdmin.json)
aws_secret_access_key=$(jq -r .AccessKey.SecretAccessKey /tmp/PaulAdmin.json)

[eksDev]
aws_access_key_id=$(jq -r .AccessKey.AccessKeyId /tmp/JeanDev.json)
aws_secret_access_key=$(jq -r .AccessKey.SecretAccessKey /tmp/JeanDev.json)

[eksInteg]
aws_access_key_id=$(jq -r .AccessKey.AccessKeyId /tmp/PierreInteg.json)
aws_secret_access_key=$(jq -r .AccessKey.SecretAccessKey /tmp/PierreInteg.json)

EoF
```


#### Test this with dev profile:

```
aws sts get-caller-identity --profile dev
```

<pre>
{
    "UserId": "AROAUD5VMKW75WJEHFU4X:botocore-session-1581687024",
    "Account": "xxxxxxxxxx",
    "Arn": "arn:aws:sts::xxxxxxxxxx:assumed-role/k8sDev/botocore-session-1581687024"
}
</pre>

> the assumed-role is k8sDev, so we achieved our goal

When specifying the **--profile dev** parameter we automatically ask for temporary credentials for the role k8sDev
You can test this with **integ** and **admin** also.
 
<details>
  <summary>with admin:</summary>
  
```
aws sts get-caller-identity --profile admin
{
    "UserId": "AROAUD5VMKW77KXQAL7ZX:botocore-session-1582022121",
    "Account": "xxxxxxxxxx",
    "Arn": "arn:aws:sts::xxxxxxxxxx:assumed-role/k8sAdmin/botocore-session-1582022121"
}
```

> When specifying the **--profile admin** parameter we automatically ask for temporary credentials for the role k8sAdmin
</details>

## Using AWS profiles with Kubectl config file

It is also possible to specify the AWS_PROFILE uses with the aws-iam-authenticator in the `.kube/config` file, so that it will use the appropriate profile.


### with dev profile 

Create new KUBECONFIG file to test this:

```
export KUBECONFIG=/tmp/kubeconfig-dev && eksctl utils write-kubeconfig eksworkshop
cat $KUBECONFIG | awk "/args:/{print;print \"      - --profile\n      - dev\";next}1" | sed 's/eksworkshop./eksworkshop-dev./g' > ~/.kube/dev
rm /tmp/kubeconfig-dev
```


We just added the `--profile dev` parameter to our kubectl config file, so that this will ask kubectl to use our IAM role associated to our dev profile.

With this configuration we should be able to interract with the **development** namespace, because it as our RBAC role defined.

let's create a pod

```
kubectl run nginx-dev --image=nginx -n development
```

We can list the pods

```
kubectl get pods -n development
```

<pre>
NAME                     READY   STATUS    RESTARTS   AGE
nginx-dev   1/1     Running   0          28h
</pre>

but not in other namespaces

```
kubectl get pods -n integration 
```

<pre>
Error from server (Forbidden): pods is forbidden: User "dev-user" cannot list resource "pods" in API group "" in the namespace "integration"
</pre>

#### Test with integ profile

```bash
export KUBECONFIG=/tmp/kubeconfig-integ && eksctl utils write-kubeconfig eksworkshop
cat $KUBECONFIG | awk "/args:/{print;print \"      - --profile\n      - integ\";next}1" | sed 's/eksworkshop./eksworkshop-integ./g' > ~/.kube/integ
rm /tmp/kubeconfig-integ
```

```bash
export KUBECONFIG=~/.kube/integ
```

let's create a pod
```bash
kubectl run nginx-integ --image=nginx -n integration
```

We can list the pods

```bash
kubectl get pods -n integration
```

<pre>
NAME          READY   STATUS    RESTARTS   AGE
nginx-integ   1/1     Running   0          43s
</pre>pre>

but not in other namespaces

```
kubectl get pods -n development 
```

<pre>
Error from server (Forbidden): pods is forbidden: User "integ-user" cannot list resource "pods" in API group "" in the namespace "development"
</pre>


#### Test with admin profile

```
export KUBECONFIG=/tmp/kubeconfig-admin && eksctl utils write-kubeconfig eksworkshop
cat $KUBECONFIG | awk "/args:/{print;print \"      - --profile\n      - admin\";next}1" | sed 's/eksworkshop./eksworkshop-admin./g' | tee $KUBECONFIG
```

let's create a pod in default namespace
```bash
kubectl run nginx-admin --image=nginx 
```

We can list the pods

```bash
kubectl get pods 
```

<pre>
NAME          READY   STATUS    RESTARTS   AGE
nginx-integ   1/1     Running   0          43s
</pre>>

We can list ALL pods in all namespaces

```
kubectl get pods -A
```

<pre>
NAMESPACE     NAME                       READY   STATUS    RESTARTS   AGE
default       nginx-admin                1/1     Running   0          15s
development   nginx-dev                  1/1     Running   0          11m
integration   nginx-integ                1/1     Running   0          4m29s
kube-system   aws-node-mzbh4             1/1     Running   0          100m
kube-system   aws-node-p7nj7             1/1     Running   0          100m
kube-system   aws-node-v2kg9             1/1     Running   0          100m
kube-system   coredns-85bb8bb6bc-2qbx6   1/1     Running   0          105m
kube-system   coredns-85bb8bb6bc-87ndr   1/1     Running   0          105m
kube-system   kube-proxy-4n5lc           1/1     Running   0          100m
kube-system   kube-proxy-b65xm           1/1     Running   0          100m
kube-system   kube-proxy-pr7k7           1/1     Running   0          100m
</pre>>


## Swithing between different contexts

It is possible to merge several kubernetes API access in the same KUBECONFIG file, or just tell Kubectl several file to lookup at once:

```bash
export KUBECONFIG=~/.kube/dev:/tmp/.kube/integ:/.kube/admin

# then run the following to get a list of contexts
kubectl config get-contexts
kubectl config current-context
kubectl config use-context my-cluster-name
```



## Conclusion

In this module, we have seen how to configure EKS to provide finer access to users combining IAM Groups and Kubernetes RBAC.
You'll be able to create different groups depending on your needs, configure their associated RBAC access in your cluster, and simply add or remove users from 
the group to grand or remove them access to your cluster.

Users will only have to configure their aws cli in order to automatically retrievfe their associated rights in your cluster.