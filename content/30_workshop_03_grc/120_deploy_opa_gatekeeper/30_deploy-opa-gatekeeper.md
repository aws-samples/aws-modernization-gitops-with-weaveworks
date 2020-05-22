---
title: "Deploy OPA gatekeeper"
date: 2020-04-12T18:00:00-00:00
draft: false
weight: 30
---

Let's deploy opa gatekeeper using a prebuilt image.

This manifest will deplpy the following in to the cluster:

- **gatekeeper-system** namespace
- **gatekeeper-admin** service account
- **config** custom resource defintion
- **ConstraintTemplate** custom resource
- **gatekeeper-webhook-service** - clusterip service
- **gatekeeper-controller-manager** deployment
- **secret** - self signed certificate `think about using something else, such as cert-manager + pki`
- **required roles**
- **ValidatingWebhookConfiguration** - this basically creates a validating webhook whenever a resource is created or udpated in the cluster


```bash
cd eksworkshop
```

make sure you have navigated into your git repo

```bash
mkdir opa

curl https://weaveworks-gitops.awsworkshop.io/30_workshop_03_grc/120_deploy_opa_gatekeeper/deploy.files/opa-gatekeper.yaml -o opa/opa-gatekeeper.yaml
```

You should see the manifest now exists insde the `opa` folder. We're no gonna let Flux install this and manage for us. Run the following to check in to git:

```bash
git add opa/opa-gatekeeper.yaml
git commit -m "adding opa gatekeeper"
git push
```

Now let's see what's installed:

```bash

kubectl get pods --namespace gatekeeper-system

```

You should see the gatekeeper-controller-manager running now.

Let's see what else is there. Run the following to see what crd's have been deployed:

```bash

kubectl get crd
```

You should see two, one is called `config`, this is mainly used for auditing and checking what has already been deployed in to the cluster before OPA and flagging any pre-existing violations.

You should see another one called `constrainttemplates`, let's focus on this as this is what helps us define our policies.
