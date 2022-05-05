# Getting started

This repository contains all scripts required for deploying a FIWARE platform to run on an OpenShift cluster.

## Preconditions

Since this repository concentrates on deploying the platform, we require the underlying infrastructure to be already setup in a defined way. The listed preconditions have to be met and are constantly tested, but it might also work on comparable alternative setups.

<B>The following preconditions need to be fulfilled before starting :warning: :</B>

- [RedHat OpenShift](https://www.redhat.com/en/technologies/cloud-computing/openshift) in >= 4.x installed - see [official documentation](https://docs.openshift.com/container-platform/latest/welcome/index.html) for installing it
- [ArgoCD](https://argo-cd.readthedocs.io/en/stable/) >=2.3.x installed - multiple options are available:
    - [install ArgoCD documentation](https://argo-cd.readthedocs.io/en/stable/getting_started/#1-install-argo-cd)
    - [use OpenShift operator](https://argocd-operator-helm.readthedocs.io/en/latest/ocp/ocp4.html)
    - [FIWARE installation documentation](https://github.com/FIWARE-Ops/fiware-gitops#4-install-argocd)
- [OpenShift CLI](https://docs.openshift.com/container-platform/4.10/cli_reference/openshift_cli/getting-started-cli.html) installed - see [installation documentation](https://docs.openshift.com/container-platform/4.10/cli_reference/openshift_cli/getting-started-cli.html#installing-openshift-cli)

## Installation steps


### 1. Fork the repo

To build and configure the platform using the [GitOps-Pattern](https://www.gitops.tech/), you should fork this repository to your own git. 
On [github](github.com), follow the [fork-a-repo tutorial](https://docs.github.com/en/get-started/quickstart/fork-a-repo). For other git-installations, see the corresponding documentation

### 2. Set the repo url in the app.yaml

Since ArgoCD should now follow the new fork, instead of the original repository, you have to replace the ```repoURL``` in all app.yaml files.(see [Application setup](APP_SETUP.md) on their locations). 
This can be done for all files at once with: 
```shell
find . -type f -name "app.yaml" -exec sed -i'' -e 's,repoURL: https://github.com/FIWARE-Ops/marinera,repoURL: <FORK_URL>,g' {} +
```

### 3. Create the target namespace inside your cluster

> :warning: Make sure for the next steps that you are logged in to the cluster via ```oc login``` 
> and that the account has enough permissions to create namespaces and create resources 


The platform should be deployed to a namespace. Create the namespace via:
```shell
oc new-project <PLATFORM_NAMESPACE>
```

### 4. Set the target namespace for the argo apps

Similar to the ```repoURL```, the ```destinition: namespace``` has to be replaced in all files.This can be done for all files at once with: 

```shell
find . -type f -name "app.yaml" -exec sed -i'' -e 's,namespace: demo,namespace: <PLATFORM_NAMESPACE>,g' {} +
```

### 5. Apply the applications to the cluster

Now that everything is prepared, the app.yaml files can be applied to the cluster. 
Use: 
```shell
find . -name 'app.yaml' -exec oc apply -f {} \;
```
It is convenient to apply some labels to the applications to support filtering:
```shell
find . -name 'app.yaml' -exec oc label <LABEL_KEY>=<LABEL_VALUE> -f {} \;

```