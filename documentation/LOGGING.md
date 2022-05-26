# Logging

To be able to observe, discover and analize the platform logs we will need Openshift-Logging installed in our cluster.

The most of the pods/components has the next labels applied so it will easy us the log filtering:

- `marinera/platform`: always has the same value `fiware`
- `marinera/component`: can be `core`, `historical`, `visualization` or `security`
- `marinera/subcomponent`: is the subcomponent inside the component. for example: `broker` (inside `core` component)
- `marinera/product`: is the product used in the subcomponent. Example: `orion-ld`

The completed list:

|                 | platform  | component     | subcomponent | product     | 
|-----------------|-----------|---------------|--------------|-------------|
| mongodb         | fiware    | core          | persistence  | mongodb     |
| orion-ld        | fiware    | core          | broker       | orion-ld    |
| quantumleap     | fiware    | historical    | connector    | quantumleap |
| timescale       | fiware    | historical    | persistence  | timescaledb |
| grafana         | fiware    | visualization | ui           | grafana     |
| keycloak server | fiware    | security      | auth         | keycloak    |
| postgres        | fiware    | security      | persistence  | postgres    |
| pep-proxy       | fiware    | security      | api-gateway  | pep         |

The most of the pods/components brings logs in plain text format. However, some of them have JSON format support or even have been extended in order to be parsed by the collector. 

|                 | logs          | JSON format |
|-----------------|---------------|-------------|
| mongodb         | yes           | yes         |
| orion-ld        | yes           | yes         |
| quantumleap     | yes           | no          |
| timescale       | yes           | no          |
| grafana         | yes           | yes         |
| keycloak server | yes           | no          |
| postgres        | yes           | no          |
| pep-proxy       | yes           | no          |

## 1. Install Openshift-Logging

To deploy Openshift-Logging in the cluster follow the instructions in the [official documentation](https://docs.openshift.com/container-platform/latest/logging/cluster-logging-deploying.html)

## 2. Parsing JSON logs

The most of the platform components have been deployed to write logs in json format. Openshift Logging can parse JSON logs into a structured object and forward them to either OpenShift Logging-managed Elasticsearch.

You can do that using the Log Forwarding API. This is an examplet to do the parsing:

```yaml
apiVersion: logging.openshift.io/v1
kind: ClusterLogForwarder
metadata:
  name: instance
  namespace: openshift-logging
spec:
  inputs: <1>
    - application:
        selector:
          matchLabels:
            marinera/platform: fiware
      name: FiwareLogs
  outputDefaults:
    elasticsearch:
      structuredTypeKey: kubernetes.namespace_name
  pipelines:
    - inputRefs:
        - FiwareLogs
      name: pipeline-fiware-logs
      outputRefs:
        - default
      parse: json
    - inputRefs:
        - infrastructure
        - application
        - audit
      name: all-to-default
      outputRefs:
        - default
```
<1> Filter the logs which are parsed. All pods deployed has label `marinera/platform: fiware`