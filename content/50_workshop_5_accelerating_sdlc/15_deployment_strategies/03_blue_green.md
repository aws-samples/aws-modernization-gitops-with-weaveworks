+++
title = "Blue / Green"
chapter = false
weight = 503
+++

In Blue / Green deployments, you will have two nearly identical environments, one of which is serving live traffic (Blue). New versions of your applications can be deployed as Green. Once any testing or quality assurance has been performed on the upgraded Green environment, traffic can be routed to the Green, and the blue will eventually be scaled down.

![Blue Green Deployment](/images/blue-green-deployment-ww.png)

### Benefits of Blue/Green Deployment

* Avoids versioning issues
* Instant rollout and rollback (while the blue deployment still exists)

### Drawbacks of Blue/Green Deployment

* Requires resource duplication
* Data synchronization between the two environments can lead to partial service interruption

### Suitable Use Cases

* Monolithic legacy applications
* Autonomous microservices
