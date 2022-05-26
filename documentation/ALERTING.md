# Alerting

In order to make potential problems inside the platform visible, [alerts](https://docs.openshift.com/container-platform/4.10/monitoring/managing-alerts.html) can be configured, based on the available metrics.

## Approach

Since applications should know best about there failure indicators, the alerts will be configured as a part of the appliction. 
For that, create a [PrometheusRule](../applications/keycloak/chart/templates/alert.yaml) resource:
```yaml
    apiVersion: monitoring.coreos.com/v1
    kind: PrometheusRule
    metadata:
        name: prom-rule
    spec:
        groups:
        - name: group.alerts
            rules:
            - alert: AlertName
              expr: |
                <alert experession>
              labels:
                severity: <alert-severtiy>
```
Please check the general [documentation of alerts](https://docs.openshift.com/container-platform/4.10/monitoring/managing-alerts.html) and the initial [keycloak-alert](../applications/keycloak/chart/templates/alert.yaml).