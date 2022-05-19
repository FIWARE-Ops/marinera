# Keycloak Helm Chart

A Helm chart for Kubernetes to deploy Keycloak using the Bitnami Helm Chart.

## Maintainers

| Name | Email |
| ---- | ------ |
| Felipe Roca | <felipe@hopu.org> |
| Alvaro Lopez | <alopezme@redhat.com> |
| Stefan Wiedemann | <stefan.wiedemann@fiware.org> |

## Source Code

* <https://github.com/FIWARE-Ops/marinera>
* <https://github.com/bitnami/charts/tree/master/bitnami/keycloak>

## Install

To install the chart with the release name `my-release`:

```console
$ helm dependency up
$ helm install my-release {{ template "chart.name" . }}
```

## Values

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| realm.grafana.rootUrl | string | `https://grafana` | Grafana root URL |
| realm.grafana.adminURL | string | `https://grafana` | Grafana admin URL |
| realm.grafana.redirectUris | list | `[{"https://grafana/", "https://grafana-2/"}]` | Grafana Redirect URIs |
| realm.grafana.webOrigins | list | `[{"https://grafana/", "https://grafana-2/"}]` | Grafana Web Origins |
| realm.orionPep.baseUrl | string | `https://orion-ld` | Orion base URL |
| realm.orionPep.adminUrl | string | `https://orion-ld` | Orion admin URL |
| realm.orionPep.redirectUris | list | `[{"https://orion-ld/", "https://orion-ld-2/"}]` | Orion Redirect URIs |
| realm.orionPep.webOrigins | list | `[{"https://orion-ld/", "https://orion-ld-2/"}]` | Orion Web Origins |
| route.enabled | bool | `true` | Enable OpenShift route |
| tests.enabled | bool | `true` | Enable Helm Chart tests to check that the Realm is properly configured |

For the documentation of the original helm chart, go to the official helm chart at: https://github.com/bitnami/charts/tree/master/bitnami/keycloak 
