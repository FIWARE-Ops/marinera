# The local Marinera

In order to provide an easy way to start for developers, marinera can also be installed locally. To get as close to a real production environment, the local version will be served by [RedHat OpenShift Local](https://developers.redhat.com/products/openshift-local/overview). When following the guide, you will have an instance with the following components:

- [ArgoCD](https://argo-cd.readthedocs.io/en/stable/) to deploy the local components
- Integration of ArgoCD with the OpenShift-Authentication
- a [Vault](https://www.vaultproject.io/) to handle all secrets in a 
- a reduced installation of the Marinera, containing only [Orion-LD](https://github.com/FIWARE/context.Orion-LD) and its [MongoDB](https://www.mongodb.com/)

The setup uses [kustomize](https://kustomize.io/) to apply some local modifications and setup the system to serve Marinera.


## Installing OpenShift Local

- See Installation documentation: https://access.redhat.com/documentation/en-us/red_hat_openshift_local/2.4/html/getting_started_guide/installation_gsg#installing_gsg
- Download Latest OpenShift Local: https://console.redhat.com/openshift/create/local
- Extract the crc binary and add it to your local bin path

```bash
(cd ~/Downloads && tar xvf crc-linux-amd64.tar.xz)
mkdir -p ~/bin
cp ~/Downloads/crc-linux-*-amd64/crc ~/bin
```

## Installing Helm

- See Installation documentation: https://helm.sh/docs/intro/install/
- Download the latest Helm: https://github.com/helm/helm/releases
- Extract the helm binary and add it to your local bin path

```bash
(cd ~/Downloads && tar xvf helm-*-linux-amd64.tar.gz --strip-components=1)
mkdir -p ~/bin
cp ~/Downloads/helm ~/bin
```

## Setup and Start a new  OpenShift Local

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

## Setting up the OpenShift Environment for FIWARE using Kustomize GitOps

### Login to OpenShift

- Visit your OpenShift environment at the URL provided by the `crc start` command: https://console-openshift-console.apps-crc.testing
- Login with username "kubeadmin", and the password provided in the `crc start` command. 
- Login to the cli for your OpenShift Local cluster by clicking on "kubeadmin" in the top right corner and selecting "Copy login command". 
- Click "Display token"
- Copy the part that starts with `oc login --token=... --server=...` to the clipboard and paste it into the terminal and press [ Enter ] to login to the CLI. 

### Clone the fiware-marinera repo

```bash
git clone git@github.com:rh-impact/fiware-marinera.git ~/.local/src/fiware-marinera
cd ~/.local/src/fiware-marinera
# the approach allows to use helm charts as part of kustomize and deploy the full marinera
# if only orion-ld and mongodb are enabled "oc apply -k kustomize/overlays/openshift-local/" is sufficient
oc kustomize kustomize/overlays/openshift-local/ --enable-helm --load-restrictor=LoadRestrictionsNone | oc apply -f -
```

If ```oc kustomize``` does not succeed on first try with a message similar to:

```
Error from server (NotFound): error when creating "STDIN": the server could not find the requested resource (post applications.argoproj.io)
Error from server (NotFound): error when creating "STDIN": the server could not find the requested resource (post applications.argoproj.io)
Error from server (NotFound): error when creating "STDIN": the server could not find the requested resource (post applicationsets.argoproj.io)
Error from server (NotFound): error when creating "STDIN": the server could not find the requested resource (post argocds.argoproj.io)
Error from server (NotFound): error when creating "STDIN": the server could not find the requested resource (post clustersecretstores.external-secrets.io)
Error from server (NotFound): error when creating "STDIN": the server could not find the requested resource (post externalsecrets.external-secrets.io)
Error from server (NotFound): error when creating "STDIN": the server could not find the requested resource (post externalsecrets.external-secrets.io)
Error from server (NotFound): error when creating "STDIN": the server could not find the requested resource (post operatorconfigs.operator.external-secrets.io)
```
then you have to wait for a couple of seconds and repeat. The error happens, since the CRDs not created at the time oc tries to create such resources. 

### Setup a new Vault

- Visit the vault here: https://vault-ui-vault.apps-crc.testing
- For Raft Storage, select `Create a new Raft cluster`, then click [ Next ]
- Set Key shares: 1
- Set Key threshold: 1
- Click [ Initialize ]
- Setup a new vault and keep track of the `Initial Root Token`, and the vault keys (1 key is enough). 
- When asked for the Unseal Key Portion, enter the `Key 1` password
- When asked to Sign in to Vault, choose Method `Token`, enter the `Initial Root Token`

### Setup a new Vault Engine

- In the Vault, click Secrets
- Click [ Enable new engine + ]
- Select Generic -> KV, then click [ Next ]
- Path: fiware
- Click [ Enable Engine ]

WARNING: You can ingnore the following error message in case you receive it:
```yaml
Upgrading from non-versioned to versioned data. This backend will be unavailable for a brief period and will resume service shortly.
```

### Setup a Vault Read Policy

- In the Vault, click Policies
- Click [ Create ACL policy + ]
- Name: vault-secret-reader
- Here is the policy:

```json
path "/fiware/data/*" {
  capabilities = ["read"]
}
```

### Setup a new Access Authentication Method in Vault

- In the Vault, click Access
- In Auth Methods, click [ Enable new method + ]
- In "Infra", click "Kubernetes"
- Path: kubernetes
- Click [ Enable Method ]
- Kubernetes host: https://api.crc.testing:6443
- Click [ Save ]

### Setup a secret-reader Auth Role

- In the new `kubernetes/` Authentication Method, click [ Create role + ]
- Name: secret-reader
- Bound service account names: vault-secret-reader
- Bound service account namespaces: external-secrets-operator
- Click "Tokens"
- Generated token's policies: vault-secret-reader
- Click [ Save ]

### Setup new mongodb-secret in Vault

- In Vault, click Secrets
- In the "fiware" path, add a new secret with a path of "mongodb-secret"
- Add a password for the key: mongodb-password
- Add a password for the key: mongodb-replica-set-key
- Add a password for the key: mongodb-root-password
- Click [ Save ]


## Check

When all steps are done, you should have a running orion-ld with its mongo-db in namespace ```fiware```:

```oc get pods -n fiware ``` :

```
NAME                             READY   STATUS    RESTARTS   AGE
mongodb-orion-59fd8f47f9-84vvr   2/2     Running   0          34m
orion-ld-85cd7b4894-z8hg4        1/1     Running   0          6m29s
```

Note that both mongo-db and orion-ld pods won't come up untill the mongo-db `ExternalSecret` is `Synced`.
You can easily check the in the `fiware` namespace:

```oc -n fiware get externalsecret mongodb-secret``` :

```
NAME             STORE                    REFRESH INTERVAL   STATUS         READY
mongodb-secret   fiware-cluster-secrets   15s                SecretSynced   True
```

Once the `ExternalSecret` is `Synced`, both mongo-db and orion-ld should start soon.
Otherwise you can force a restart by deleting the Pods:

```oc -n fiware delete pod --all```


Once mongo-db and orion-ld are up, the [API](https://www.etsi.org/deliver/etsi_gs/CIM/001_099/009/01.06.01_60/gs_cim009v010601p.pdf) 
can be reached at ```http://orion-ld-fiware.apps-crc.testing```, f.e.:
```shell
curl --location --request GET 'http://orion-ld-fiware.apps-crc.testing/version'

{
  "orionld version": "1.0.1-PRE-468",
  "orion version":   "1.15.0-next",
  "uptime":          "0 d, 0 h, 10 m, 33 s",
  "git_hash":        "47989a0d865f0aeef6a3f7cf3bc0e314bc4f3fb5",
  "compile_time":    "Tue Jan 18 08:17:10 UTC 2022",
  "compiled_by":     "root",
  "compiled_in":     "",
  "release_date":    "Tue Jan 18 08:17:10 UTC 2022",
  "doc":             "https://fiware-orion.readthedocs.org/en/master/"
}

```

### ArgoCD

Orion-LD and MongoDB are installed using ArgoCD. You can check the status of the ArgoCD apps: 

- See: https://argocd-server-argocd.apps-crc.testing/applications
- Click [ Allow selected permissions ]
- The `fiware-mongodb-orion` and `fiware-orion-ld` should be `Healthy` and `Synced`

## Modification

Once the system is setup and you're used to it, the marinera-plattform can be configured through the typical way, by chaning the single [values.yaml](../clusters/in-cluster/marinera/values.yaml). Depending on the needs and available resources, additional components can be enabled or configured, as described in the [Alternative deployments guide](ALT_DEPLOYMENTS.md).
