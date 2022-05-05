# Application setup

For the CI to work properly, the applications have to follow some rules:

## Folder structure

The CI expects a certain structure of folders, this structure has been thought to work with both ArgoCD and/or Helm
independently.

All the applications of this repo are expected to be Helm charts. That's why inside every application folder there must be a
`chart` folder containing the chart.

So the structure is as follows: `applications/<APP>/chart/`.
```bash                                                                                                               
├── applications                                                                                                
│   ├── mongodb
│   │   ├── app.yaml
│   │   └── chart
│   │       ├── Chart.yaml
│   │       └── values.yaml
│   └── orion-ld
│       ├── app.yaml
│       └── chart
│           ├── Chart.yaml
│           ├── orion-ld
│           └── values.yaml
```

* `applications/<AAP>/`: Parent folder for the application, containing both the Helm chart definition and the ArgoCD app definition.
  * `chart` Parent folder for the Helm chart.
  * `app.yaml` ArgoCD application definition.


* `applications/<AAP>/chart/`: Parent folder for the Helm chart.
  * `Chart.yaml` contains the Helm chart definition using the umbrella helm chart pattern, pointing to an external chart as a dependency.
  * `values.yaml` default Helm values file for the app to work. This very opinionated defaults should work most of the time without any changes.

## Naming convention

The yaml file holding ArgoCD application's has to be namend  ```app.yaml```. It will be picked up automatically by the CI pipeline.


## Required fields

The default target environment for all applications inside this repository is the demo-environment inside the namespace ```demo```. This has to be set at the key:
```yaml
    spec:
      destination:
        namespace: demo
```

For ArgoCD to be able to follow the changes inside the repository, the ```repoURL``` and the ```targetRevision``` have to be set to this repo and the main-branch:
```yaml
    spec:
      source:
        repoURL: https://github.com/FIWARE-Ops/marinera
        targetRevision: main    
```

The CI also requires the ```helm: releaseName``` to be correctly set. ArgoCD uses the application name as releaseName as long as nothing is configured. Since Helm charts use the releasename as part of the  [service](https://kubernetes.io/docs/concepts/services-networking/service/) name most of the time, changing the application name inside the pipeline can break the connection between components. By setting a fixed ```releaseName``` the [service](https://kubernetes.io/docs/concepts/services-networking/service/) names will stay stable and the connection will work.
```yaml
   spec:
      source:
        helm:
          releaseName: <APPLICATION_NAME>
```

The target namespace for the application itself needs to be set to the namespace ArgoCD is living in:
```yaml
    metadata:
      namespace: <ARGO-CD-NAMESPACE>
```

A full example would be:
```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  # name of the argocd application - will be changed by the ci pipeline according to the # branch name - f.e. feature-1-orion-ld
  name: orion-ld
  # argocd namespace 
  namespace: labs-ci-cd
spec:
  destination:
    # default namespace for the main branch to be deployed to
    namespace: demo
    server: https://kubernetes.default.svc
  project: default
  source:
    helm:
      valueFiles:
      - values.yaml
      # name of the release should be set to the application name on the main branch. 
      # will be left untouched by the ci-pipeline
      releaseName: orion-ld
    # path to the chart inside this repo
    path: applications/orion-ld/chart
    # url of this repo
    repoURL: https://github.com/FIWARE-Ops/marinera
    # branch name of the default branch on this repo
    targetRevision: main
  syncPolicy:
    automated: {}
```