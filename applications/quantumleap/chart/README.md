# QuantumLeap Chart

A Helm chart for Kubernetes to deploy QuantumLeap using the official Helm Chart.

## Requirements

| Repository | Name | Version |
|------------|------|---------|
| https://orchestracities.github.io/charts/ | quantumleap | 0.1.18 |

## Installing the Chart

To install the chart with the release name `my-release`:

```console
$ helm dependency up
$ helm install my-release {{ template "chart.name" . }}
```

## Values

For the documentation of the original helm chart, go to the official helm chart at: https://github.com/orchestracities/charts/tree/master/charts/quantumleap
