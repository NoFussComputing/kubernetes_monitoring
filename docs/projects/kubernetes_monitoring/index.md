---
title: Kubernetes Monitoring Helm Chart
description: How to use No Fuss Computings kubernetes monitoring helm chart for full stack monitoring.
date: 2023-09-19
template: project.html
about: https://gitlab.com/nofusscomputing/projects/kubernetes_monitoring
---

This Helm chart deploys the components needed for full stack monitoring.

What this chart is:

- Collectors for metrics and logging

- Distribution / Routing of metrics/logging

- Data visualization

- Ready for logging long term storag integration, in particular Loki

- Ready for metrics long term storage integration, **Still to be developed, see [GL-#1](https://gitlab.com/nofusscomputing/projects/kubernetes_monitoring/-/issues/1)**

What this chart is not:

- A complete monitoring stack inclusive of long term storage.

Intentionally we have kept this helm chart to the monitoring components only. Keeping the monitoring components seperate from storage is advantageous. for all intents and purposes this helm chart is ephemeral.

This helm chart started off with components from multiple open-source projects. As such attribution is warrented, so head on over to thier projects and give them a star. Projects used to create the building blocks of this helm chart are

- [ceph](https://github.com/ceph/ceph) _mixin alerts/rules_

- [Kube prometheus](https://github.com/prometheus-operator/kube-prometheus)

- [prometheus adaptor](https://github.com/kubernetes-sigs/prometheus-adapter)


## Features

- Alert Manager

- Grafana as the visualization frontend

    - Dashboards

        - Ceph
        
        - Cluster Overview

        - Node Exporter Full

        - Alert Manager
    
    - Sidecar to load dashboards from ConfigMaps _Optional_

- Grafana-Agent 

    - daemonset for cluster nodes for exposing metrics and an injecter to loki for node and container logs.

- Loki integration for storing logs

- [Mixins](https://github.com/monitoring-mixins/website) Compatible _(partial)_, Tested with:

    - alert-manager

    - ceph

    - coreDNS

    - Kubernetes

    - loki _Loki [helm chart](https://artifacthub.io/packages/helm/grafana/loki) included, see [loki repo](https://github.com/grafana/loki/tree/f4ab1e3e89ac66e1848764dc17826abde929fdc5/production/loki-mixin-compiled) for dashboards_

    - Node-Exporter

    - Prometheus

    - Promtail

- Prometheus as the metrics collector

- Service monitors

    - API Server

    - CAdvisor
    
    - Calico, _Optional_

        > enable calico metrics with `kubectl patch felixconfiguration default --type merge --patch '{"spec":{"prometheusMetricsEnabled": true}}'`

    - Ceph _Optional_

    - CoreDNS

    - Grafana

    - Kube-Controler-Manager

    - Kube-Scheduler

    - Kubelet

    - Kube-state-metrics

    - Node
    
    - Node-Exporter

    - Prometheus

    - Prometheus-Adaptor

- kyverno policies _(optional, set in values.yaml)_

    - auto deploy policy for prometheus role and rolebinding to access other namespaces

- `values.yaml` customization of components. _See values.yaml for more details._


## Installation

This chart as the following **dependencies:**

- [Grafana Operator](https://artifacthub.io/packages/olm/community-operators/grafana-operator)

- [Prometheus Operator](https://artifacthub.io/packages/olm/community-operators/prometheus)

These dependencies must be present before installing this chart. to install run the following commands:

``` bash

git clone -b development https://gitlab.com/nofusscomputing/projects/kubernetes_monitoring.git

helm upgrade -i nfc_monitoring kubernetes_monitoring/

```


## Template Values

The values file included in this helm chart is below.

!!! note
    This values file is from the development branch and as such may not reflect the current version you have deployed. If you require a specific version, head on over to the git repository and select the git tag that matches the version you are after.

``` yaml title="values.yaml" linenums="1"

--8<-- "values.yaml"

```
