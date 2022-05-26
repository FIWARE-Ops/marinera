# E2E Testing

Inside the `tests/` folder in the repo there are two helm charts. The `e2e-test` helm chart and the `selenium-grid` helm chart.

## Usage

To deploy the E2E tests it is important to have Selenium running and ready in the cluster first:

```shell
cd tests/selenium-grid
helm install --wait <my-selenium-release> . -n <PLATFORM_NAMESPACE>
```

After this, you can run the e2e test the following way:

```shell
cd ../e2e-test
helm install <my-e2e-release> . -n <PLATFORM_NAMESPACE>
```

The e2e tests will spin up a pod called `marinera-e2e` and run the tests, to see the logs you could do the following:

```shell
oc logs -f marinera-e2e -n <PLATFORM_NAMESPACE>
```

If the test e2e works, the pod will get to a `Completed` status and will be deleted automatically. If it doesn't work, the helm installation will failed and the pod will remain with an `Error` status.

After that, to remove the helm charts you can do the following:

```shell
helm uninstall <my-e2e-release> -n <PLATFORM_NAMESPACE>
helm uninstall <my-selenium-release> -n <PLATFORM_NAMESPACE>
```