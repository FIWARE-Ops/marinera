# MongoDB Helm Chart

A Helm chart for Kubernetes to deploy MongoDB using the Bitnami MongoDB Helm Chart.

## Source Code

* <https://github.com/FIWARE-Ops/marinera>
* <https://github.com/bitnami/charts>

## Requirements

| Repository | Name | Version |
|------------|------|---------|
| https://charts.bitnami.com/bitnami | mongodb | 11.0.4 |

## Install

To install the chart with the release name `my-release`:

```console
$ helm dependency up
$ helm install my-release {{ template "chart.name" . }}
```

## Values

For the documentation of the original helm chart, go to the Bitnami helm chart at: https://github.com/bitnami/charts/tree/master/bitnami/mongodb.
