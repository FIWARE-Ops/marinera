# Secrets

> **NOTE:** For this moment, this feature is only compatible with **MongoDB**, **Orion-LD**, **Grafana**, **Grafana Metrics**, and **TimescaleDB** charts.

To manage the secrets in this repo we are using the Sealed Secrets approach by Bitnami. Sealed Screts allow you to store your password in a safe way event in a public repository due to they only can be decrypted by the controller running in the target cluster. If you want to know more, go to the Sealed Secrets Github repo at https://github.com/bitnami-labs/sealed-secrets.

Now, follow the procedure to activate the secret management:

## 1. Install Sealed Secrets  

To be able to encrypt your secrets, you need to install the Sealed Secrets controller and the `kubeseal` binary. To do that, please follow the documentation from Bitnami at https://github.com/bitnami-labs/sealed-secrets#installation.

## 2. Change the values in FIWARE Platform chart

Go to `fiware-platform/values.yaml` and set the `helm-values` property to `values-secured.yaml` in the compatible apps (see the note at the beginning of this guide). For example:

```yaml
applications:
  - name: orion-ld
    enabled: true
    source_path: applications/orion-ld/chart
    source_ref: *branch
    destination: *destination
    helm_values:
    - values-secured.yaml
```

## 3. Encrypt your secrets

First of all, you need to decide if you want your secrets to be able to be decrypted cluster wide or by namespace, for that, change the `scope.namespace` value in the `secrets/values.yaml` file. `false` will mean `cluster-wide` and `true` will mean `namespace-wide`. To understand more about scope, see the Bitnami documentation at: https://github.com/bitnami-labs/sealed-secrets#scopes

To encrypt your secrets you have to execute the following command for each value of the `secrets/values.yaml` file. **IMPORTANT:** in the `--scope` part of the command, you have to put the same scope as in the values file.

```
echo -n <secret> | kubeseal --controller-name=<name of the sealed secrets controller> --controller-namespace=<namespace where the controller is located> --raw --from-file=/dev/stdin --scope=<cluster-scope>
```

For example: 

```
echo -n my-secret-password | kubeseal --controller-name=sealed-secrets --controller-namespace=my-controller-namespace --raw --from-file=/dev/stdin --scope=namespace-wide
```

## 4. Install the secrets helm chart

After editting the `values.yaml` file with all your secrets encrypted, you can deploy the helm chart by going to the `secrets` directory and execute:

```
helm install secrets .
```

## 5. Continue with the FIWARE Platform deployment

Now go back to the [Getting Started](./GETTING_STARTED.md#3-set-the-repo-url-in-the-valuesyaml) and continue from where you left.
