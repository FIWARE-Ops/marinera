
# Installing OpenShift Local
- See Installation documentation: https://access.redhat.com/documentation/en-us/red_hat_openshift_local/2.4/html/getting_started_guide/installation_gsg#installing_gsg
- Download Latest OpenShift Local: https://console.redhat.com/openshift/create/local
- Extract the crc binary and add it to your local bin path

```bash
(cd ~/Downloads && tar xvf crc-linux-amd64.tar.xz)
mkdir -p ~/bin
cp ~/Downloads/crc-linux-*-amd64/crc ~/bin
```

# Setup and Start a new  OpenShift Local

You can configure the amount of cpu cores, memory in MiB and disk size in GiB allocated to your OpenShift Local environment depending on the resources of your machine

```bash
crc setup
crc config --help
crc config set cpus 8
crc config set memory 12000
crc config set disk-size 200
crc start
```

If you need to upgrade your OpenShift Local because of some version error, delete the cluster and create it again like above. 

```bash
crc delete
```

# Setting up the OpenShift Environment for FIWARE using Kustomize GitOps

## Login to OpenShift

- Visit your OpenShift environment at the URL provided by the `crc start` command: https://console-openshift-console.apps-crc.testing
- Login with username "kubeadmin", and the password provided in the `crc start` command. 
- Login to the cli for your OpenShift Local cluster by clicking on "kubeadmin" in the top right corner and selecting "Copy login command". 
- Click "Display token"
- Copy the part that starts with `oc login --token=... --server=...` to the clipboard and paste it into the terminal and press [ Enter ] to login to the CLI. 

## Clone the fiware-marinera repo

```bash
git clone git@github.com:rh-impact/fiware-marinera.git ~/.local/src/fiware-marinera
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
