# Secrets

> **NOTE:** Currently this feature is only compatible with **MongoDB**, **Orion-LD**, **Grafana**, **Grafana Metrics**, and **TimescaleDB** charts.

To manage the secrets in this repo we are using the [Sealed Secrets controller by Bitnami](https://github.com/bitnami-labs/sealed-secrets). Sealed Secrets allow you to store your password safely (encrypted) in a public repository. They can only be decrypted by the controller running in the target cluster.

The apps that are compatible with Sealed Secrets have three different values files:

* `values.yaml`: This is where all values not related to secrets are stored.
* `values-secured.yaml`: This file has the values that enable the secret management for that app.
* `values-unsecured.yaml`: This file has the unencrypted secrets if you prefer to not use this procedure.

Now, follow the procedure to activate the secret management:

## 1. Install Sealed Secrets

To be able to encrypt your secrets, you need to install the Sealed Secrets controller and the `kubeseal` binary. To do that, please follow [Bitnami documentation](https://github.com/bitnami-labs/sealed-secrets#installation).

## 2. Enable the secrets in the FIWARE Platform chart

Go to `fiware-platform/values.yaml` and set the `secretsEnabled` property to `true`. This will make the apps ready for secrets to use the `values-secured.yaml` file merged with the `values.yaml` file. For example:

```yaml
source: https://github.com/FIWARE-Ops/marinera
release: demo
destination_namespace: &destination demo
branch: &branch main
secretsEnabled: &secretsEnabled true
```

## 3. Encrypt your secrets

First of all, you need to decide if you want your secrets to be able to be decrypted cluster wide or by namespace, for that, change the `scope.namespace` value in the `secrets/values.yaml` file. `false` will mean `cluster-wide` and `true` will mean `namespace-wide`. To understand more about scope, see the [Bitnami documentation about secrets scope](https://github.com/bitnami-labs/sealed-secrets#scopes).

To encrypt your secrets you have to execute the following command for each value of the `secrets/values.yaml` file. 
**IMPORTANT:** in the `--scope` part of the command, you have to put the same scope as in the values file.

```bash
echo -n <secret> | kubeseal --controller-name=<name of the sealed secrets controller> --controller-namespace=<namespace where the controller is located> --raw --from-file=/dev/stdin --scope=<cluster-scope>
```

For example:

```bash
echo -n my-secret-password | kubeseal --controller-name=sealed-secrets --controller-namespace=my-controller-namespace --raw --from-file=/dev/stdin --scope=namespace-wide
```

## 4. Install the secrets helm chart

After editing the `secrets/values.yaml` file with all your secrets encrypted, you can deploy the helm chart by going to the `secrets` directory and execute:

```bash
helm install secrets .
```

## 5. Continue with the FIWARE Platform deployment

Now go back to the [Getting Started](./GETTING_STARTED.md#3-set-the-repo-url-in-the-valuesyaml) and continue from where you left.
