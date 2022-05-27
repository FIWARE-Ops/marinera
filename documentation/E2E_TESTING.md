# E2E Testing

This End-2-End tests have been developed using Selenium. [Selenium](https://www.selenium.dev/documentation/) is an open-source automated testing framework used to validate web applications across different browsers and platforms. You can use multiple programming languages like create Selenium Test Scripts.

For that reason we need an instance of Selenium running in the cluster, and then we can run our test using it.

Inside the `tests/` folder in the repo there are two helm charts. The `e2e-test` helm chart and the `selenium-grid` helm chart.

## Usage

### 1. Deploy Selenium
To deploy the E2E tests it is important to have Selenium running and ready in the cluster first:

```shell
cd tests/selenium-grid
helm install --wait <my-selenium-release> . -n <PLATFORM_NAMESPACE>
```

### 2. Change `values.yaml`

Change `values.yaml` file in the folder `tests/e2e-test/` to adjust it to your deployment. By default, the variables match if you deploy the FIWARE platform as is.

### 3. Run e2e tests

You can run the e2e test the following way:

```shell
cd ../e2e-test
helm install <my-e2e-release> . -n <PLATFORM_NAMESPACE>
```

The e2e tests will spin up a pod called `marinera-e2e` and run the tests, to see the logs you could do the following:

```shell
oc logs -f marinera-e2e -n <PLATFORM_NAMESPACE>
```

If the test e2e works, the pod will get to a `Completed` status and will be deleted automatically. If it doesn't work, the helm installation will fail and the pod will remain in `Error` state.

### 4. Uninstall
After the test, remember to remove the helm charts. You can do the following:

```shell
helm uninstall <my-e2e-release> -n <PLATFORM_NAMESPACE>
helm uninstall <my-selenium-release> -n <PLATFORM_NAMESPACE>
```
