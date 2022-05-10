# Orion-LD Helm Chart

A Helm chart for Kubernetes to deploy Orion-LD using the official Helm Chart.

## Source Code

* <https://github.com/FIWARE-Ops/marinera>
* <https://github.com/openshift-helm-charts/charts>

## Requirements

| Repository | Name | Version |
|------------|------|---------|
| https://charts.openshift.io/ | orion-ld | 1.0.3 |

**NOTE:** As of this moment (9th of May 2022) there is not a way to consume charts from OpenShift repository (Bugzilla [2081762](https://bugzilla.redhat.com/show_bug.cgi?id=2081762)), that is why the chart is located in this repo too.
 
## Install

To install the chart with the release name `my-release`:

```console
$ helm install my-release {{ template "chart.name" . }}
```

## Values

For the documentation of the original helm chart, go to the official helm chart at: https://github.com/openshift-helm-charts/charts/tree/main/charts/partners/fiware/orion-ld/1.0.3/src.