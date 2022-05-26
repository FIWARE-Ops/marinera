# Monitoring

The monitoring functionality can be enabled to measure the different behavior across the platform components. It is optional to install Openshift-Monitoring in order to obtain this information from the platform services such as number of queries, number of subscriptions, inserts, etc.

The most of the pods/components brings Prometheus metrics. However, some of them have been extended with a JSON exporter.

|                 | metrics     |
|-----------------|-------------|
| mongodb         | native      |
| orion-ld        | json-exporter    |
| quantumleap     | not allowed |
| timescale       | native      |
| grafana         | native      |
| keycloak server | native      |
| postgres        | native      |
| pep-proxy       | not allowed |

## 1. Install Openshift-Monitoring

To deploy Openshift-Monitoring in the cluster follow the instructions in the [official documentation](https://docs.openshift.com/container-platform/latest/monitoring/monitoring-overview.html).

## 2. Enable Openshift User Workload Monitoring

Follow the instructions in the [official documentation](https://docs.openshift.com/container-platform/4.10/monitoring/enabling-monitoring-for-user-defined-projects.html).