# Getting started

This repository contains all scripts required for deploying a FIWARE platform to run on an OpenShift cluster.

## Preconditions

Since this repository concentrates on deploying the platform, we require the underlying infrastructure to be already setup in a defined way. The listed preconditions have to be met and are constantly tested, but it might also work on comparable alternative setups.

<B>The following preconditions need to be fulfilled before starting :warning: :</B>

- [Red Hat OpenShift](https://www.redhat.com/en/technologies/cloud-computing/openshift) in >= 4.x installed - see [official documentation](https://docs.openshift.com/container-platform/latest/welcome/index.html) for installing it
- [ArgoCD](https://argo-cd.readthedocs.io/en/stable/) >=2.3.x installed - multiple options are available:
    - [install ArgoCD documentation](https://argo-cd.readthedocs.io/en/stable/getting_started/#1-install-argo-cd)
    - [use OpenShift operator](https://argocd-operator-helm.readthedocs.io/en/latest/ocp/ocp4.html)
    - [FIWARE installation documentation](https://github.com/FIWARE-Ops/fiware-gitops#4-install-argocd)
- [OpenShift CLI](https://docs.openshift.com/container-platform/4.10/cli_reference/openshift_cli/getting-started-cli.html) installed - see [installation documentation](https://docs.openshift.com/container-platform/4.10/cli_reference/openshift_cli/getting-started-cli.html#installing-openshift-cli)
- [Helm](https://helm.sh/docs/intro/install/) installed.
- [Openshift-Logging](#logging) installed and configured

## Installation steps

### 1. Fork the repo

To build and configure the platform using the [GitOps-Pattern](https://www.gitops.tech/), you should fork this repository to your own git.
On [github](github.com), follow the [fork-a-repo tutorial](https://docs.github.com/en/get-started/quickstart/fork-a-repo). For other git-installations, see the corresponding documentation

### 2. Decide which FIWARE applications to deploy

Go to `fiware-platform/values.yaml` and set the `enabled` value to either `true` or `false`.
For example, you can enable Orion-LD but disable Quantum Leap:
```yaml
applications:
  - name: orion-ld
    enabled: true
    source_path: applications/orion-ld/chart
    source_ref: *branch
    destination: *destination
    helm_values:
    - values.yaml
    values:
      orion-ld:
        myvalue: XXX

  - name: quantumleap
    enabled: false
    source_path: applications/quantumleap/chart
    source_ref: *branch
    destination: *destination
    helm_values:
    - values.yaml
```

*NOTE:*

By default each application is deployed with a sane set of default values that have been tested to work in most cases.
But this does not mean they are the right fit for a production ready deployment.
Please verify each application potential values (as all of them are Helm charts). You can either directly change the `values.yaml` of individual apps,
or use the `values:` property directly in the app definition list in `fiware-platform/values.yaml` to override and/or set default values.

### 3. Set the repo url in the values.yaml

Since ArgoCD should now follow the new fork, instead of the original repository, you have to replace the ```source``` in `fiware-platform/values.yaml`.

This can be done with this one liner:
```shell
sed -i'' -e 's,source: https://github.com/FIWARE-Ops/marinera,source:  <FORK_URL>,g' fiware-platform/values.yaml
```

### 4. Create the target namespace inside your cluster

> :warning: Make sure for the next steps that you are logged in to the cluster via ```oc login```
> and that the account has enough permissions to create namespaces and create resources

The platform should be deployed to a namespace. Create the namespace via:
```shell
oc new-project <PLATFORM_NAMESPACE>
```

### 5. Set the target namespace in the values.yaml

Similar to the ```source```, the ```destination``` namespace can be replaced.
This can be done with the following command:

```shell
sed -i'' -e 's/destination_namespace: \&destination demo/destination_namespace: \&destination <PLATFORM_NAMESPACE>/g' fiware-platform/values.yaml
```

### 6. Set the target branch in the values.yaml

Similar to the ```source```,, the ```branch``` can be replaced, so the ArgoCD apps will sync to that branch.
This can be done with the following command:

```shell
sed -i'' -e 's/branch: \&branch main/branch: \&branch <FORK_BRANCH>/g' fiware-platform/values.yaml
```
### 7. Apply the applications to the cluster

Now that everything is prepared, we can deploy the FIWARE components chosen by creating the ArgoCD applications
and applying them to the cluster:
```bash
cd fiware-platform/
helm template . | oc -n <ARGOCD_NAMESPACE> apply -f -
```

This will create ArgoCD apps.

![FIWARE components deployed](./images/argocd-apps.png)

## E2E Testing

Inside the `tests/` folder in the repo there are two helm charts. The `e2e-test` helm chart and the `selenium-grid` helm chart.

### Usage

To deploy the E2E tests it is important to have Selenium running and ready in the cluster first:

```shell
cd tests/selenium-grid
helm install --wait <my-selenium-release> . -n <PLATFORM_NAMESPACE>
```

After this, you can run the e2e test the following way:

```shell
cd ../e2e-test
helm install <my-e2e-release> . -n <PLATFORM_NAMESPACE>
```

The e2e tests will spin up a pod called `marinera-e2e` and run the tests, to see the logs you could do the following:

```shell
oc logs -f marinera-e2e -n <PLATFORM_NAMESPACE>
```

If the test e2e works, the pod will get to a `Completed` status and will be deleted automatically. If it doesn't work, the helm installation will failed and the pod will remain with an `Error` status.

After that, to remove the helm charts you can do the following:

```shell
helm uninstall <my-e2e-release> -n <PLATFORM_NAMESPACE>
helm uninstall <my-selenium-release> -n <PLATFORM_NAMESPACE>
```

## Logging

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

### 1. Install Openshift-Logging

To deploy Openshift-Logging in the cluster follow the instructions in the [official documentation](https://docs.openshift.com/container-platform/latest/logging/cluster-logging-deploying.html)

### 2. Parsing JSON logs

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
