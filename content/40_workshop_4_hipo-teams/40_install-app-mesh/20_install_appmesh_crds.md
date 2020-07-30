+++
title = "Install App Mesh CRDs"
chapter = false
weight = 20
+++

For every namespace in our EKS cluster we will create a folder in our repo that will contain the declarations of every resource we want to live in that namespace. __This becomes the desired state of that namespace__.

The controller requires CRDs to be installed. We have a couple of options to do this:

1. Apply the CRDs directly (using kubectl)
2. Apply via GitOps by adding the CRDs to our repo

As we are using GitOps we will be going with option 2!

Firstly create a `appmesh-system` folder in our repo:

```bash
.
├── appmesh-system
├── namespaces
│   └── appmesh-system.yaml
└── README.md
```

Now run the following commands to download the App Mesh CRDs into the new folder:

```bash
curl https://raw.githubusercontent.com/aws/eks-charts/v0.0.19/stable/appmesh-controller/crds/crds.yaml -o appmesh-system/crds.yaml
```

Your folder structure should look like this now:

```bash
.
├── appmesh-system
│   └── crds.yaml
├── namespaces
│   └── appmesh-system.yaml
└── README.md
```

Add and then commit the **crds.yaml** file and push the the changes to your GitHub repo.

Flux will now see that the desired state of the `appmesh-system` namespace has changed in Git and will apply the CRDs to our cluster. This will take up to 1 minute to apply.

Check that that the CRDs has been created by running the following command:

```bash
kubectl get crds
```

You should see 3 App Mesh CRDs installed:

```bash
NAME                               CREATED AT
eniconfigs.crd.k8s.amazonaws.com   2020-05-20T10:45:46Z
helmreleases.helm.fluxcd.io        2020-05-20T12:01:29Z
meshes.appmesh.k8s.aws             2020-05-20T12:58:31Z
virtualnodes.appmesh.k8s.aws       2020-05-20T12:58:31Z
virtualservices.appmesh.k8s.aws    2020-05-20T12:58:32Z
```
