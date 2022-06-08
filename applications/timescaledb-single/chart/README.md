# Timescale DB Helm Chart

A Helm chart for Kubernetes to deploy Timescale DB using the official Helm Chart.

## Source Code

* <https://github.com/FIWARE-Ops/marinera>
* <https://github.com/timescale/timescaledb-kubernetes>

This chart was ported from version 0.11.0 of the original TimescaleDB-single helm chart with a few modifications.

## Install

To install the chart with the release name `my-release`:

```console
$ helm install my-release {{ template "chart.name" . }}
```

## Values

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| rbac.openshift | bool | `false` | Creates anyuid Role & Rolebinding to run in OpenShift |

For the documentation of the original helm chart, go to the official helm chart at: https://github.com/timescale/timescaledb-kubernetes/tree/master/charts/timescaledb-single.
