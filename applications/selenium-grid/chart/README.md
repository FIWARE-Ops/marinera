# Selenium-Grid Helm Chart

This chart is based on the official selenium-grid chart available at https://github.com/SeleniumHQ/docker-selenium.

## Source Code

* <https://github.com/FIWARE-Ops/marinera>
* <https://github.com/SeleniumHQ/docker-selenium>

## Installing

To install the selenium-grid helm chart, you can run:

```bash
helm install selenium .
```

## Values

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| testValues.remoteDriverUrl | string | `http://selenium-hub:4444` | Remote Driver URL for the test |
| testValues.grafanaUrl | string | `http://grafana` | Grafana URL for the test |
| testValues.brokerUrl | string | `http://orion-ld:1026` | Broker URL for the test |
| testValues.quantumLeapUrl | string | `http://quantumleap-quantumleap:8668` | Quantumleap URL for the test |
| testValues.testTenant | string | `test-tenant` | Test tenant for the test |
| testValues.grafanaUsername | string | `fiwareAdmin` | Grafana username for the test |
| testValues.grafanaPassword | string | `fiwareAdmin` | Grafana password for the test |

For the documentation of the original helm chart, go to the official helm chart at: https://github.com/SeleniumHQ/docker-selenium.