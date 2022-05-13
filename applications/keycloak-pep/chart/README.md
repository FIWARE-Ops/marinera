# PEP ProxyHelm Chart

A Helm chart for Kubernetes to deploy a Spring Boot application that uses the PEP Keycloak library

## Maintainers

| Name | Email |
| ---- | ------ |
| Felipe Roca | <felipe@hopu.org> |
| Alvaro Lopez | <alopezme@redhat.com> |

## Source Code

* <https://github.com/FIWARE-Ops/marinera>
* <https://spring.io/projects/spring-boot>

## Install

To install the chart with the release name `my-release`:

```console
$ helm dependency up
$ helm install my-release {{ template "chart.name" . }}
```

## Values

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| image.repository | string | "docker.io/hopu/keycloak-rest-pep" | Path to the container image |
| image.tag | string | "dev" | Tag of the container image |

