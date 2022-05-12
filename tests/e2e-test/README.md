# Marinera E2E Test Helm Chart

A Helm chart for Kubernetes to deploy the e2e tests for Marinera repository.

## Source Code

* <https://github.com/FIWARE-Ops/marinera>

## Install

To install the chart with the release name `my-release`:

```console
$ cd tests/e2e-test
$ helm install my-release .
```

## Values

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| image | string | `quay.io/fiware/marinera-e2e:0.0.1` | Marinera e2e image to deploy |
| imagePullPolicy | string | `IfNotPresent` | Image pull policy for the pod |
| remoteDriverUrl | string | `http://selenium-hub:4444` | Remote Driver URL for the test |
| grafanaUrl | string | `https://grafana-demo.apps.fiware-spain.emea-1.rht-labs.com` | Grafana URL for the test |
| brokerUrl | string | `http://orion-ld:1026` | Broker URL for the test |
| quantumLeapUrl | string | `http://quantumleap-quantumleap:8668` | Quantumleap URL for the test |
| testTenant | string | `test-tenant` | Test tenant for the test |
| grafanaUsername | string | `fiwareAdmin` | Grafana username for the test |
| grafanaPassword | string | `fiwareAdmin` | Grafana password for the test |
