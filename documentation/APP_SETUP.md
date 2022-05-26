# Application setup

There are some structure and naming conventions in this repo for the applications to work and also expected by the Continuous Integration (CI) tool, in this case [Github CI workflows](./GITHUB_CI.md).

## Folder structure

It is expected a certain structure of folders, this structure has been thought to work with both ArgoCD and/or Helm
independently.

All the applications of this repo are expected to be Helm charts. That's why inside every application folder there must be a
`chart` folder containing the chart.

So the structure is as follows: `applications/<APPLICATION_NAME>/chart/`.
```bash
├── applications
│   ├── grafana
│   │   └── chart
│   ├── mongodb
│   │   └── chart
│   ├── orion-ld
│   │   └── chart
│   ├── quantumleap
│   │   └── chart
│   └── timescaledb-single
│       └── chart
├── fiware-platform
│   ├── Chart.yaml
│   ├── templates
│   └── values.yaml
```

* `applications/<APPLICATION_NAME>/`: Parent folder for the application, containing both the Helm chart definition and the ArgoCD app definition.
  * `chart` Parent folder for the Helm chart.

* `applications/<APPLICATION_NAME>/chart/`: Parent folder for the Helm chart.
  * `Chart.yaml` contains the Helm chart definition using the umbrella helm chart pattern, pointing to an external chart as a dependency.
  * `values.yaml` default Helm values file for the app to work. This very opinionated defaults should work most of the time without any changes.

Additionally the folder `fiware-platform` has to exist as well, with the `fiware-platform/values.yaml` being the entry-point to add and/or remove applications from being deployed.

## Add a new app to the repo

In order to add a new application to the repo, the following steps are required.

1. Creates folder structure

```bash
mkdir -p applications/<APPLICATION_NAME>/chart/
```

2. Populate `applications/<APPLICATION_NAME>/chart/` folder with your Helm chart definition and a sane `values.yaml` file.

3. Add your app to the `fiware-platform/values.yaml`, to the `appplications:` list as follows:

```yaml
applications:
  - name: <APPLICATION_NAME>
    enabled: true
    source_path: applications/<APPLICATION_NAME>/chart
    source_ref: *branch
    destination: *destination
    helm_values:
    - values.yaml
```

Ideally only `name` and `source_path` should be changed.

4. Now apply the new application. The following command will process the list of applications and create an ArgoCD app manifest for each one.

```bash
helm template . | oc -n <ARGOCD_NAMESPACE> apply -f -
```

> *NOTE:* Note the new apps are labeled with the label `destination-namespace=<DESTINATION_NAMESPACE>` meaning you can filter them easily by executing the following command.
>```bash
>oc -n <ARGOCD_NAMESPACE> get app -l destination-namespace=<DESTINATION_NAMESPACE>
>```