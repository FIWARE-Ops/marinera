# Related Issues

This page contains all issues that have been found and should be considered regarding this repository. All of them have been registered on their related repository. If some of those were fixed, the functionalities and configurations of the FIWARE platform could be updated.

## Logging Issues

These issues has been pushed in order to improve the logs of the platform.

| Issue | Component | Description |
|------------|------|---------|
| https://github.com/orchestracities/charts/pull/96 | quantumleap | Pull Request to be able to add pod labels in values.yaml |
| https://github.com/orchestracities/ngsi-timeseries-api/issues/647 | quantumleap | Requested JSON format |
| https://github.com/timescale/timescaledb-kubernetes/issues/368 | timescale | Requested JSON format |
| https://access.redhat.com/solutions/6610621 | Openshift Loggging | Send logs duplicated to the internal elastic |

## Security Issues

Solving the following issues would bring to the platform a better security as well as liveness and readiness.

| Issue | Component | Description |
|------------|------|---------|
| https://github.com/orchestracities/ngsi-timeseries-api/issues/377 | quantumleap | Fix health method to set liveness and readiness |
| https://github.com/orchestracities/ngsi-timeseries-api/issues/393 | quantumleap | Connect to Timescale over SSL |


## ArgoCD Issues

| Issue | Component | Description |
|------------|------|---------|
| https://github.com/argoproj/argo-cd/issues/2789 |  | Support remote helm values files |
| https://github.com/argoproj/argo-cd/issues/5065#issuecomment-745593317 |  | Helm Value files from different location |


## OpenShift Helm Charts Repo

| Issue | Component | Description |
|------------|------|---------|
| https://bugzilla.redhat.com/show_bug.cgi?id=2081762 |  | Umbrella helm charts do not work with the current |

## Monitoring Issues
| Issue | Component | Description |
|------------|------|---------|
| https://github.com/orchestracities/ngsi-timeseries-api/issues/539 | quantumleap | Metrics in prometheus format available |
