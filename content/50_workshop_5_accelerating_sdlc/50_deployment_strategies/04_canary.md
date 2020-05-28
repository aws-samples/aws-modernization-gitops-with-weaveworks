+++
title = "Canary"
chapter = false
weight = 504
+++

In Canary deployments, you will incrementally introduce upgraded versions of your services. Upgraded versions of your service are introduced as Canary deployments, where percentages of live traffic are routed to the upgraded services. Depending on the success / failure threshold, the rollout of the upgraded service will continue through to accepting 100% of live traffic in the instance of successful metrics / requests, or rolled back in the instance of failed requests.

![Canary Deployments](/images/canary-deployment-ww.png)

## Benefits of Canary Deployments

* Low impact as the new version is released only to a subset of users
* Controlled rollout with no downtime
* Fast rollback

## Drawbacks of Canary Deployments

* Typically requires a traffic management solution / service mesh (Envoy, Istio, Linkerd)
* Requires backwards compatibility between API versions and database migrations

## Suitable Use Cases

* User facing applications
* Stateless microservices
