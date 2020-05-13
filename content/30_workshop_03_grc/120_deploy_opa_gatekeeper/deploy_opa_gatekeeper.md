---
title: "Deploy OPA gatekeeper"
date: 2020-04-12T18:00:00-00:00
draft: false
weight: 20
---

Let's deploy opa gatekeeper using a prebuilt image.

This manifest will deplpy the following in to the cluster:

- gatekeeper-system namespace
- gatekeeper-admin service account
- config custom resource defintion
- gatekeeper-webhook-service


```bash

kubectl apply -f https://raw.githubusercontent.com/open-policy-agent/gatekeeper/master/deploy/gatekeeper.yaml
```

Now let's se what's installed:

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




