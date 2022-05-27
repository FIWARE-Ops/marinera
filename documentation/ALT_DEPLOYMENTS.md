
# Alternative deployments

Apart from the [getting started](./GETTING_STARTED.md) guide to deploy the FIWARE Platform, there are more options in this repo to deploy it in alternative way.

- [Enable/Disable Components](#enabledisable-components)
- [With Sealed Secrets](#with-sealed-secrets)
- [With specific sizing](#with-specific-sizing)


## Enable/Disable Components

If you don't need an specific component it can be disabled. More info and examples in the section ['Decide which FIWARE applications to deploy'](./GETTING_STARTED.md#2-decide-which-fiware-applications-to-deploy).

## With Sealed Secrets

All secrets are in the repo as plain-text. If you are a company concerned about the secure vaulting of secrets maybe you are interested in deploy secrets as Sealed Secrets. You can find more info and steps in the section ['Secrets'](./SECRETS.md).

## With specific sizing

The most of the components have [requests and limits](https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/#requests-and-limits) values set in their resources by default. But maybe you need different sizing to support your amount of devices and workload.

The only you have to do is editing the `values.yaml` file from each component you'd like to change and deploy the changes. All `values.yaml` file are referenced in the section [Components and configuration](./FIWARE_COMPONENTS.md#components-and-configuration).

You can find different sizing and specifics requests and limits values in this section ['Load tests results'](./LOAD_TESTING.md#load-tests-results).
