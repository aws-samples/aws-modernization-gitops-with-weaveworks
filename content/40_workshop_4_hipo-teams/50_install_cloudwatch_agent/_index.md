+++
title = "Install CloudWatch Agent"
chapter = true
weight = 50
+++

In the previous section we setup Prometheus to collect the metrics from App Mesh. Ideally we want to have the metrics available in Cloudwatch. We can use the [CloudWatch Agent with Prometheus metrics Collection](https://aws.amazon.com/blogs/containers/using-prometheus-metrics-in-amazon-cloudwatch/) to collect the metrics from Propmetheus and publis them to CloudWatch.

We will follow a GitOps methodology to install the agent.
