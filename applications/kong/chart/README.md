# Kong Helm Chart

A Helm chart for Kubernetes to deploy a Kong as a PEP-Proxy inside Marinera

## Maintainers

| Name | Email |
| ---- | ------ |
| Stefan Wiedemann | <stefan.wiedemann@fiware.org> |

## Source Code

* <https://github.com/FIWARE/kong-plugins-fiware>
* <https://github.com/Kong/kong>

## Install

To install the chart with the release name `my-release`:

```console
$ helm dependency up
$ helm install my-release {{ template "chart.name" . }}
```

## Values

For the documentation of the original helm chart, go to the Kong helm chart at: https://github.com/Kong/charts.
To better support OpenShift, additional configuration for Route-Objects is supported:

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| kong.proxy.route | boolean | true | Should the route be enabled? |
| kong.proxy.route.tls | yaml | [[see config](values.yaml) | TLS configuration for the route object. |
| kong.proxy.route.host | string | ' ' | Host to be used for the route |
| kong.proxy.route.annotations | array | [] | Annotations to be applied to the route |
| kong.proxy.route.certificate | boolean | true | Should the tls certificate be injected? |


