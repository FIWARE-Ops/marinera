# Keycloak Helm Chart

A Helm chart for Kubernetes to deploy Keycloak using the official keycloak operator.

## Maintainers

| Name | Email |
| ---- | ------ |
| Felipe Roca | <felipe@hopu.org> |
| Alvaro Lopez | <alopezme@redhat.com> |

## Source Code

* <https://github.com/FIWARE-Ops/marinera>
* <https://github.com/keycloak/keycloak-operator>

## Install

To install the chart with the release name `my-release`:

```console
$ helm dependency up
$ helm install my-release {{ template "chart.name" . }}
```

## Values

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| keycloak.instances | int | 1 | Number of Keycloak replicas |

