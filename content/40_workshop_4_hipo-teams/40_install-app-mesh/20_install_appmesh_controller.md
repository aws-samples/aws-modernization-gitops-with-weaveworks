+++
title = "Install App Mesh controller"
chapter = false
weight = 20
+++

For every namespace in our EKS cluster we will create a folder in our repo that will contain the declarations of every resource we want to live in that namespace. __This becomes the desired state of that namespaces__. 

### Add the CRDs

The controller requires CRDs to be installed. We have a coupld of options to do this:

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
curl https://raw.githubusercontent.com/aws/eks-charts/master/stable/appmesh-controller/crds/crds.yaml -o appmesh-system/crds.yaml
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


### Add the controller

The AWS App Mesh Controller is available as a Helm package from the [EKS Chart Repository](https://aws.github.io/eks-charts/). We can use the **Helm Operator** along with this chart to install the controller.

To install a chart using the [Helm Operator](https://docs.fluxcd.io/projects/helm-operator/en/latest/) we need to define a **HelmRelease**. A HelmRelease is a way to declare the desired state of a Helm release. The HelmRelease can be applied to a cluster using GitOps.

Create a new file called `appmesh-controller.yaml` in the **appmesh-system** folder. Add the following as contents to the file:

```yaml


```