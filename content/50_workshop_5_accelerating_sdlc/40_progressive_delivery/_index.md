+++
title = "Progressive Delivery"
chapter = true
weight = 40
+++

## Progressive Delivery

In this section, we will create a Canary deployment using [Flagger](https://github.com/weaveworks/flagger) to incrementally introduce traffic to new versions of your services. Documentation for Flagger can be found [here](https://docs.flagger.app).

We will demonstrate a successful Canary deployment and how an unsuccessful Canary deployment automatically gets rolled back without impacting application uptime.
