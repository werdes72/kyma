---
title: Monitoring
---

[Prometheus](https://prometheus.io/) is the open source monitoring and alerting toolkit that collects and stores metrics data. This data is consumed by different addons, including [Grafana](https://grafana.com/) for analytics and monitoring, and [Alertmanager](https://prometheus.io/docs/alerting/alertmanager/) for handling alerts.

Monitoring in Kyma is configured to collect all metrics relevant for observing the in-cluster [Istio](https://istio.io/latest/docs/concepts/observability/) Service Mesh. For diagrams of the default setup and the monitoring flow including Istio, see [Monitoring Architecture](../../../05-technical-reference/00-architecture/obsv-01-architecture-monitoring.md).

Learn how to [enable Grafana visualization](../../../04-operation-guides/operations/obsv-03-enable-grafana-for-istio.md) and [enable mTLS for custom metrics](../../../04-operation-guides/operations/obsv-04-enable-mtls-istio.md).

## Limitations

In the [production profile](../../../04-operation-guides/operations/02-install-kyma.md##choose-resource-consumption), Prometheus stores up to **15 GB** of data for a maximum period of **30 days**. If the default size or time is exceeded, the oldest records are removed first. The evaluation profile has lower limits.

The configured memory limits of the Prometheus and Prometheus-Istio instances define the number of time series samples that can be ingested. 

The default resource configuration of the monitoring component in the production profile is sufficient to serve **800K time series in the Prometheus Pod**, and **400K time series in the Prometheus-Istio Pod**. The samples are deleted after 30 days or when reaching the storage limit of 15 GB. 


The amount of generated time series in a Kyma cluster depends on the following factors:

* Number of Pods in the cluster
* Number of Nodes in the cluster
* Amount of exported (custom) metrics
* Label cardinality of metrics
* Number of buckets for histogram metrics
* Frequency of Pod recreation
* Topology of the Istio Service Mesh

You can see the number of ingested time series samples from the `prometheus_tsdb_head_series` metric, which is exported by the Prometheus itself. Furthermore, you can identify expensive metrics with the [TSDB Status](http://localhost:9090/tsdb-status) page.
