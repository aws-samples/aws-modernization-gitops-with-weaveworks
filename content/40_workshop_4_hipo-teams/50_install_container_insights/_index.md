+++
title = "Install Container Insights"
chapter = true
weight = 50
+++

# Install Container Insights

In the previous section we setup Prometheus to collect the metrics from App Mesh. Ideally we want to have the metrics available in Cloudwatch. We can use the [Container Insights](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/ContainerInsights.html) which recently [added support for collecting prometheus metrics](https://aws.amazon.com/blogs/containers/using-prometheus-metrics-in-amazon-cloudwatch/).

We will follow a GitOps methodology to install Container Insights.
