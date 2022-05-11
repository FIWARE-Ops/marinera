# Grafana Helm Chart

A Helm chart for Kubernetes to deploy Grafana using the official Helm Chart.

## Maintainers

| Name | Email |
| ---- | ------ |
| Felipe Roca | <felipe@hopu.org> |
| Alvaro Lopez | <alopezme@redhat.com> |

## Source Code

* <https://github.com/FIWARE-Ops/marinera>
* <https://github.com/grafana/helm-charts>

## Requirements

| Repository | Name | Version |
|------------|------|---------|
| https://grafana.github.io/helm-charts | grafana | 6.28.0 |

## Install

To install the chart with the release name `my-release`:

```console
$ helm dependency up
$ helm install my-release {{ template "chart.name" . }}
```

## Values

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| admin.password | string | `"fiwareAdmin"` | Admin password |
| admin.username | string | `"fiwareAdmin"` | Admin username |
| datasources.orion.url | string | `"https//orion"` | Orion URL |
| datasources.timescale.credentials.password | string | `"admin"` | Timescale database password |
| datasources.timescale.credentials.username | string | `"admin"` | Timescale database username |
| datasources.timescale.database | string | `"postgres"` | Timescale database name |
| datasources.timescale.url | string | `"tsdb"` | Timescale URL |
| nameOverride | string | `"grafana"` |  |
| route.enabled | bool | `true` | To enable the OpenShift route |
| tests.enabled | bool | `true` | To enable default tests |

For the rest of the documentation of the original helm chart, go to the official helm chart at: https://github.com/grafana/helm-charts.
