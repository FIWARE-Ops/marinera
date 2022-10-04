
# Setting up the OpenShift Environment for FIWARE using Kustomize GitOps

Login to the cli for your OpenShift Local cluster

```bash
git clone git@github.com:FIWARE-Ops/marinera.git ~/.local/src/fiware-marinera
cd ~/.local/src/fiware-marinera
oc apply -k kustomize/overlays/openshift-local/
```

## Setup a new vault

- Visit the vault here: https://vault-ui-vault.apps-crc.testing
- Setup a new vault and keep track of the Initial Root Token, and the vault keys (1 key is enough). 
- Click [ Enable new engine + ]
- Select Generic -> KV, then click [ Next ]
- Path: fiware
- Click [ Enable Engine ]

## Setup a read policy

- Click Policies
- Click [ Create ACL policy + ]
- Name: vault-secret-reader
- Policy: 

## Setup a new Access Authentication Method in vault

# Setup Kubernetes authentication for Vault, see: https://learn.hashicorp.com/tutorials/vault/kubernetes-external-vault
# Visit: https://vault-ui-vault.apps-crc.testing
# VAULT_HELM_SECRET_NAME: oc -n vault get secrets | grep vault-token
# TOKEN_REVIEW_JWT: oc -n vault get secret $VAULT_HELM_SECRET_NAME --output='go-template={{ .data.token }}' | base64 --decode
# KUBE_HOST: oc config view --raw --minify --flatten --output='jsonpath={.clusters[].cluster.server}'
# KUBE CA CERT: oc config view --raw --minify --flatten --output='jsonpath={.clusters[].cluster.certificate-authority-data}' | base64 --decode
# Role Name: secret-reader
# Bound service account names: vault-secret-reader
# Bound service account namespaces: external-secrets-operator

- Path: kubernetes
- Click [ Enable Method ]
- Kubernetes host: https://api.crc.testing:6443 `oc config view --raw --minify --flatten --output='jsonpath={.clusters[].cluster.server}'`
  
- Kubernetes CA Certificate: `oc config view --raw --minify --flatten --output='jsonpath={.clusters[].cluster.certificate-authority-data}' | base64 --decode`
- Click [ Save ]

### Setup a secret-reader Auth Role

- In the new `kubernetes/openshift-local' Authentication Method, click [ Create role + ]
- Name: secret-reader
- Bound service account names: vault-secret-reader
- Bound service account namespaces: external-secrets-operator
- Click "Tokens"
- Generated token's policies: vault-secret-reader
- Click [ Save ]

vault write auth/kubernetes/role/secret-reader bound_service_account_names=vault-secret-reader bound_service_account_namespaces=external-secrets-operator policies=vault-secret-reader ttl=24h

```
path "*" {
  capabilities = ["read"]
}
```

- Click [ Create policy ]

## Setup new mongodb-secret in vault

- Inside the "fiware" path, add a new secret with a path of "mongodb-secret"
- Add a password for: mongodb-password
- Add a password for: mongodb-replica-set-key
- Add a password for: mongodb-root-password
- Click [ Save ]
