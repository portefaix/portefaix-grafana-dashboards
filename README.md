# Portefaix Grafana Dashboards

## Description

This repository contains some [Grafana](https://github.com/grafana/grafana) dashboards.

You can also download them on [Grafana.com](https://grafana.com/grafana/dashboards/?plcmt=top-nav&cta=downloads&search=portefaix).

## Dashboards

| Dashboard                 |                      Screenshot                       | Grafana dashboard ID |
| :------------------------ | :---------------------------------------------------: | :------------------: |
| Node Fleet Overview       | [LINK](https://grafana.com/grafana/dashboards/22269)  |       `22269`        |
| Node Overview             | [LINK](https://grafana.com/grafana/dashboards/22272)  |       `22272`        |
| Node CPU and System       |                                                       |                      |
| Node Disks and Filesystem |                                                       |                      |
| Node Memory               |                                                       |                      |
| Node Network              |                                                       |                      |
| Node Logs                 |                                                       |                      |
| SRE                       | [LINK](https://grafana.com/grafana/dashboards/22268/) |       `22268`        |

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
