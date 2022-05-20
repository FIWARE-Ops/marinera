#  airquality-simulator Helm Chart

A Helm chart for Kubernetes to deploy the Air Quality Simulatior Java application using a custom Helm Chart.


## Maintainers

| Name | Email | Url |
| ---- | ------ | --- |
| Stefan Wiedemann | <stefan.wiedemann@fiware.org> |  |

## Source Code

* <https://github.com/FIWARE-Ops/marinera>
* <https://github.com/FIWARE-Ops/aq-simulator>

## Install

To install the chart with the release name `my-release`:

```console
$ helm dependency up
$ helm install my-release {{ template "chart.name" . }}
```

## Values

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| additionalAnnotations | object | `{}` | additional annotations for the deployment, if required |
| additionalLabels | object | `{}` | additional labels for the deployment, if required |
| affinity | object | `{}` | affinity template ref: https://kubernetes.io/docs/concepts/configuration/assign-pod-node/#affinity-and-anti-affinity |
| config | object | `{}` | configuration to be used for the simulator. Follow https://github.com/FIWARE-Ops/aq-simulator |
| fullnameOverride | string | `""` | option to override the fullname config in the _helpers.tpl |
| healthPort | int | `9090` | port to request health information at |
| image.pullPolicy | string | `"IfNotPresent"` | specification of the image pull policy |
| image.repository | string | `"quay.io/fiware/airquality-simulator"` | airquality-simulator image name ref: https://quay.io/repository/fiware/airquality-simulator |
| image.tag | string | `"0.1.0"` | tag of the image to be used |
| init.keycloak.admin.password | string | `"fiwareAdmin"` | Keycloak admin password with permissions to create users |
| init.keycloak.admin.user | string | `"fiwareAdmin"` | Keycloak admin user with permissions to create users |
| init.keycloak.airqualitySimulator.password | string | `"airqualityapp"` | Password for the new username to create in Keycloak for the Air Quality app  |
| init.keycloak.airqualitySimulator.user | string | `"airqualityapp-user"` | New username to create in Keycloak for the Air Quality app  |
| init.keycloak.enabled | bool | `true` | should the init-container be enabled |
| init.keycloak.image.pullPolicy | string | `"IfNotPresent"` | specification of the image pull policy |
| init.keycloak.image.repository | string | `"docker.io/bitnami/keycloak-config-cli"` | subscription init image name ref: https://quay.io/fiware/v2-subscription-init |
| init.keycloak.image.tag | string | `"5.2.0-debian-10-r9"` | tag of the image to be used |
| init.keycloak.url | string | `"http://keycloak"` | URL of the keycloak that will be configured from the init container |
| init.subscription.enabled | bool | `false` | should the init-container be enabled |
| init.subscription.entityType | string | `"AirQualityObserved"` | type of the entities to be send |
| init.subscription.image.pullPolicy | string | `"IfNotPresent"` | specification of the image pull policy |
| init.subscription.image.repository | string | `"quay.io/fiware/v2-subscription-init"` | subscription init image name ref: https://quay.io/fiware/v2-subscription-init |
| init.subscription.image.tag | string | `"0.1.0"` | tag of the image to be used |
| init.subscription.quantumLeapUrl | string | `"http://quantumleap-quantumleap:8668"` | URL of the quantum leap instance to be notified |
| livenessProbe.initialDelaySeconds | int | `30` |  |
| livenessProbe.periodSeconds | int | `10` |  |
| livenessProbe.successThreshold | int | `1` |  |
| livenessProbe.timeoutSeconds | int | `30` |  |
| nameOverride | string | `""` | option to override the name config in the _helpers.tpl |
| nodeSelector | object | `{}` | selector template ref: https://kubernetes.io/docs/user-guide/node-selection/ |
| readinessProbe.initialDelaySeconds | int | `31` |  |
| readinessProbe.periodSeconds | int | `10` |  |
| readinessProbe.successThreshold | int | `1` |  |
| readinessProbe.timeoutSeconds | int | `30` |  |
| replicaCount | int | `1` | initial number of target replications |
| resources | object | `{}` |  |
| serviceAccount | object | `{"create":false}` | if a service account should be used, it can be configured here ref: https://kubernetes.io/docs/tasks/configure-pod-container/configure-service-account/ |
| serviceAccount.create | bool | `false` | specifies if the account should be created |
| tolerations | list | `[]` | tolerations template ref: ref: https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/ |

