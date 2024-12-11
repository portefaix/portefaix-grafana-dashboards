# Portefaix Grafana Dashboards

## Description

This repository contains some [Grafana](https://github.com/grafana/grafana) dashboards.

You can also download them on [Grafana.com](https://grafana.com/grafana/dashboards/?plcmt=top-nav&cta=downloads&search=portefaix).

## Dashboard Naming Convention

Dashboard JSON files should follow this naming convention:

```
service-name-of-dashboard-version.json
```

**service**: The name of the service being monitored (e.g., mysql).
**name-of-dashboard**: overview, clusterplane, ... related to the service monitored
**version**: Start with `v1` for the initial version and increment with future versions (e.g., `v2`, `v3`).

Example:

```
mysql-overview-v1.json
prometheus-operator-overview-v1.json
kubernetes-cluster-metrics-v1.json
```

## Tags convention

| Tag            |                    Description                     |
| :------------- | :------------------------------------------------: |
| datasource/xxx | Which datasource is used. Multiple tag are allowed |
| service/xxx    |             Which service is monitored             |

Ex:

```
datasource/prometheus
datasource/loki
service/kubernetes
```

## Dashboards

| Dashboard                 |                      Screenshot                       | Grafana dashboard ID | Datasource(s) |
| :------------------------ | :---------------------------------------------------: | :------------------: | :-----------: |
| Node Fleet Overview       | [LINK](https://grafana.com/grafana/dashboards/22269)  |       `22269`        | `Prometheus`  |
| Node Overview             | [LINK](https://grafana.com/grafana/dashboards/22272)  |       `22272`        | `Prometheus`  |
| Node CPU and System       |                                                       |                      | `Prometheus`  |
| Node Disks and Filesystem |                                                       |                      | `Prometheus`  |
| Node Memory               |                                                       |                      | `Prometheus`  |
| Node Network              |                                                       |                      | `Prometheus`  |
| Node Logs                 |                                                       |                      | `Prometheus`  |
| SRE                       | [LINK](https://grafana.com/grafana/dashboards/22268/) |       `22268`        | `Prometheus`  |

## Installation

### Helm (Grafana or KubePrometheusStack)

```yaml
grafana:
  dashboardProviders:
    dashboardproviders.yaml:
      apiVersion: 1
      providers:
        - name: "portefaix-grafana-dashboards"
          orgId: 1
          folder: "Portefaix"
          type: file
          disableDeletion: true
          editable: true
          options:
            path: /var/lib/grafana/dashboards/portefaix-grafana-dashboards
  dashboards:
    portefaix-grafana-dashboards:
      sre:
        url: httpshttps://raw.githubusercontent.com/portefaix/portefaix-grafana-dashboards/compute/dashboards/portefaix-sre.json
        token: ""
```

### Grafana Operator

```yaml
---
apiVersion: grafana.integreatly.org/v1beta1
kind: Grafana
metadata:
  name: grafana
  labels:
    grafana.com/dashboard: main
spec:
  config:
    log:
      mode: "console"
    auth:
      disable_login_form: "false"
    security:
      admin_user: root
      admin_password: secret
---
apiVersion: grafana.integreatly.org/v1beta1
kind: GrafanaDashboard
metadata:
  name: portefaix-sre-from-url
  labels:
    grafana.com/dashboard: main
spec:
  instanceSelector:
    matchLabels:
      grafana.com/dashboard: main
  url: "https://raw.githubusercontent.com/portefaix/portefaix-grafana-dashboards/dashboards/compute/portefaix-sre.json"
```
