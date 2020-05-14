---
title: "Intro to OPA"
date: 2020-04-12T18:00:00-00:00
draft: false
weight: 20
---


# What is OPA?

Open Policy Agent (OPA) is an open source, general-purpose policy engine that enables unified, context-aware policy enforcement across the entire stack.

It allows you to express policies in a high level, declaritive way using `rego`.

Here's a link which you can run through to get a ful understanding on how opa and rego code works: [OPA](https://www.openpolicyagent.org/docs/latest/)


OPA is platform/service agnostic, it can be used in your jenkins pipelines, terraform, kubernetes and much more...

This is great as it gives us a unified way to enforce policy as code across all of our stacks.


# OPA in Kubernetes

### Admission Controllers

Kubernetes uses built in `Admission Controllers` to intercept requests sent to the api server to validate or even mutate the request before they persist in etcd.

Some examples of this are:

- **NamespaceLifecycle** - this prevents objects being created in namespaces when they are set to be deleted, also deletes all objects from namespaces when they are set to be deleted.

- **PodSecurityPolicy** - applied a security plicy to all pods in the cluster outside of the control plane. If you don't have a policy and this is enabled, then no pods can be created!!

The downside of using **PodSecurityPolicy** is that you can only have 1 thwta applied to everything, maybe you would like to have different policies for different namespaces?

Kubernetes has 2 special admission controllers: `MutatingAdmissionWebhook` and `ValidatingAdmissionWebhook`.

This allows us to decouple Admission Control from the api server and create our own so any time certain requests are made to the api server, we can now intercept them.

### OPA Gatekeeper


This is where OPA Gatekeeper comes in to play.

We want to be able to intercept `Create` & `Update` requests sent to the api server and validate them on a set of given rules. 

When we deploy OPA Gatekeeper, we create a `ValidatingWebhookConfiguration` which will intercept the requests and send them to the gatekeeper-controler, this will then query against a set of defined rules.

We will dicsuss later on how we define these rules.

#### Overview of OPA Gatekeeper Architecture

![OPA gatekeeper](/images/opa-gatekeeper.png)

- **Configure Replication** - this is for auditing what has already been deployed to check for pre-existing violations
- **Authz Webhook** - this is used by the api server to query opa for authz decisions
- **AdmissionController** - this is the `ValidatingAdmissionWebhook`, we set up a webhook config to leverage this controller
- **Policy/Template crd's** - the opa gatekeeper-controller will watch for these objects which wil define what actions the api server will take (see above)


Reference docs [OPA Gatekeeper](https://kubernetes.io/blog/2019/08/06/opa-gatekeeper-policy-and-governance-for-kubernetes/)
