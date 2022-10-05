
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

## Setup a Vault read policy

- In the Vault, click Policies
- Click [ Create ACL policy + ]
- Name: vault-secret-reader
- Here is the policy: 

```
path "/fiware/data/*" {
  capabilities = ["read"]
}
```

## Setup a new Access Authentication Method in vault

- In the Vault, click Access
- In Auth Methods, click [ Enable new method + ]
- In "Infra", click "Kubernetes"
- Path: kubernetes
- Click [ Enable Method ]

### Setup a secret-reader Auth Role

- In the new `kubernetes/openshift-local' Authentication Method, click [ Create role + ]
- Name: secret-reader
- Bound service account names: vault-secret-reader
- Bound service account namespaces: external-secrets-operator
- Click "Tokens"
- Generated token's policies: vault-secret-reader
- Click [ Save ]

## Setup new mongodb-secret in vault

- In Vault, click Secrets
- In the "fiware" path, add a new secret with a path of "mongodb-secret"
- Add a password for the key: mongodb-password
- Add a password for the key: mongodb-replica-set-key
- Add a password for the key: mongodb-root-password
- Click [ Save ]

## Setup new argocd-auth-secret in vault

- In Vault, click Secrets
- In the "fiware" path, add a new secret with a path of "argocd-auth-secret"
- issuer: https://sso.computate.org/auth/realms/RH-IMPACT
- name: Keycloak
- requestedScopes: ["openid", "profile", "email", "groups"]
- clientID: fiware-hackathon
- Add a given client secret for the key: clientSecret
- Click [ Save ]

## Setup namespaces in the in-cluster in ArgoCD

- Visit https://argocd-server-argocd.apps-crc.testing/settings/clusters/https%3A%2F%2Fkubernetes.default.svc
- Click [ Edit ]
- Update the NAMESPACES to the namespaces argocd will manage: argocd,fiware
