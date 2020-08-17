+++
title = "Explore Container Insights"
chapter = false
weight = 40
+++

We will now explore some of the observability that Container Insights gives us. This is a very quick look at CloudWatch....it can do a lot!

In the **AWS Console** make sure you are in **CloudWatch** (which you will probably be from the last section).

On the left navigation pane click **Resources** under **Container Insights**.

Click any of the items from the list of Resources and you will see a performance dashboard open giving you an insight into the performance:

![Performance Monitoring](/images/ci_perf_monitoring.png)

There is a specific resource for **Prometheus AppMesh** from the left most dropdown box on the **Performance Monitoring** pane. This shows a dashboard specifically for **App Mesh**. We will return to this later.

On the left navigation pane click **Metrics**. Then click **ContainerInsights** under **Custom Namespaces**. You will then be shown a list of groups at different granularities. Click **ClusterName, Namespace**.

This will show a list of the metrics that you can add to the graph. Check the box next to these 2 metrics:

![Select metrics](/images/cw_select_metrics.png)

The graph will then be updated to display the metrics.

![Metrics graph](/images/cw_metrics_graph.png)

Now click on **Log Groups** under **Logs**. You will see a number of log groups created by Container Insights for our cluster, these are prefixed with **/aws/containerinsights/gitopsworkshop**.

![Log groups](/images/cw_log_groups.png)

Click **/aws/containerinsights/gitopsworkshop/application**. You will then see a list of the log streams for your pods. Select any log stream from the list and you will be taken to the logs:

![Logs](/images/cw_app_logs.png)

{{% notice note %}}
This has been a brief look at what Container Insights and CloudWatch gives us for observability of our EKS cluster. CloudWatch enables you to do a lot more including creating alarms (a.k.a alerts), dashboards, end-to-end tracing and a lot more!
{{% /notice %}}
