# Grafana Helm Chart

A Helm chart for Kubernetes to deploy FIWARE Air Quality app.

It contains a bunch Grafana dashboards, originally tested with the [official Grafana Helm Chart](https://grafana.github.io/helm-charts).

**NOTE:**
Influenced by this example: https://github.com/ezienecker/grafana-sidecar-folder-sample/blob/master/grafana-values.yaml

## Maintainers

| Name | Email |
| ---- | ------ |
| Felipe Roca | <felipe@hopu.org> |
| Alvaro Lopez | <alopezme@redhat.com> |

## Source Code

* <https://github.com/FIWARE-Ops/marinera>

## Requirements

| Repository | Name | Version |
|------------|------|---------|
| https://grafana.github.io/helm-charts | grafana | 6.28.0 |

## Install

To install the chart with the release name `my-release`:

```console
$ helm install my-release {{ template "chart.name" . }}
```

## Values

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| nameOverride | string | `"air-quality"` |  |
