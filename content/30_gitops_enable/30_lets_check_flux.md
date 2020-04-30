---
title: "Flux and GitOps"
chapter: false
weight: 30
---

The command will take a while to run and it's a good idea to scan the output. You will note a similar bit of information in the log like this one:

<pre>
[ℹ]  Flux will only operate properly once it has write-access to the Git repository
...
[ℹ]  please configure git@github.com:YOURUSER/eks-quickstart-app-dev.git so that the following Flux SSH public key has write access to it
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQC8msUDG9tEEWHKKJw1o8BpwfMkCvCepeUSMa9iTVK6Bmxeu2pA/ivBS8Qgx/Lg8Jnu4Gk2RbXYMt3KL3/lcIezLwqipGmVvLgBLvUccbBpeUpWt+SlW2LMwcMOnhF3n86VOYjaRPggoPtWfLhFIfnkvKOFLHPRYS3nqyYspFeCGUmOzQim+JAWokf4oGOOX4SNzRKjusboh93oy8fvWk8SrtSwLBWXOKu+kKXC0ecZJK7G0jW91qb40QvB+VeSAbfk8LJZcXGWWvWa3W0/woKzGNWBPZz+pGuflUjVwQG5GoOq5VVWu71gmXoXBS3bUNqlu6nDobd2LlqiXNViaszX
</pre>

Copy the lines starting with ssh-rsa and give it read/write access to your repository. For example, in GitHub, by adding it as a deploy key. There you can easily do this in the **Settings > Deploy keys > Add deploy key**. Just make sure you check Allow write access as well.

The next time Flux syncs from Git, it will start updating the cluster and actively deploying.

If you run **git pull** next, you will see that eksctl has committed them to your config repository already.

In our case we are going to see these new arrivals (flux and helm operator) running in the cluster:

<pre>
$ kubectl get pods --all-namespaces
NAMESPACE              NAME                                                      READY   STATUS                       RESTARTS   AGE
flux                   flux-56b5664cdd-nfzx2                                     1/1     Running                      0          11m
flux                   flux-helm-operator-6bc7c85bb5-l2nzn                       1/1     Running                      0          11m
flux                   memcached-958f745c-dqllc                                  1/1     Running                      0          11m
kube-system            aws-node-l49ct                                            1/1     Running                      0          14m
kube-system            coredns-7d7755744b-4jkp6                                  1/1     Running                      0          21m
kube-system            coredns-7d7755744b-ls5d9                                  1/1     Running                      0          21m
kube-system            kube-proxy-wllff                                          1/1     Running                      0          14m
</pre>

All of the cluster configuration can be easily edited in Git now. Welcome to a fully GitOps'd world!
