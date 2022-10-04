# Hashicorp Vault
This Vault deployment was derived from the official Hashicorp Vault Helm release at: [Hashicorp](https://github.com/hashicorp/vault-helm)

There are some differences to the out of the box release however, for instance the vault agent injector is not utilized in the OCP-on-NERC configuration as we are using External Secrets to fetch secrets from the Vault.

The base of this Vault deployment was produced after adding the helm repository with: `helm repo add hashicorp https://helm.releases.hashicorp.com`

Then the underlying yalm manifests were generated with `helm template vault hashicorp/vault`

# Setting up a new vault

- Visit the vault route: https://vault-ui-vault.apps-crc.testing/ui/vault/init
- Select "Create a new Raft cluster" and click [ Next ]
- Key shares: 1
- Key threshold: 1
