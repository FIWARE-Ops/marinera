# Load Testing

You can run load tests to calculate the maximum throughput for your deployment. This section shows how to run tests using the FIWARE load testing tool and some reports of tests run in a demo scenario.

## 1. Run your own load tests

1. Clone [load-tests](https://github.com/FIWARE/load-tests) repository locally.
2. Configure the load test on [/src/test/resources/test.conf]()
3. Execute the test locally:

    ```bash
    mvn install gatling:test -Dgatling.simulationClass=simulations.nosec.v2.EntityUpdateWithSingleSubscriptionSimulation
    ```



## 2. Load test example reports

In order to calculate a starting configuration, you can check the load test reports in this section.

### 2.1 Load tests set up

Bear in mind that all the tests have been executed with the following versions of the components:

| Component           | Chart Version | App Version | Release Date |
|---------------------|---------------|-------------|--------------|
| Orion-LD            | 1.0.2 | 1.0.1 | Jan, 2022 |
| MongoDB             | 11.0.4 | 4.4.12 | Feb, 2022 |
| QuantumLeap         | 0.1.18 | 0.9.0-dev | May, 2022 |
| TimeScale           | 0.11.0 | 2.6.1 | Apr, 2022 | 
| Keycloak            | 8.0.0 | 17.0.1 | Mar, 2022 |
| Keycloak PostgreSQL | 8.0.0 | 14.2.0 | Feb, 2022 |
| PEP Proxy           | 0.1.0 | 1.0.0 | May, 2022 |




### 2.2 Idle results

This table shows the idle consumption of all the components of the deployment:

<!---TO DO: Check that MongoDB arbiter does not count in the replica count) -->

<!--- CPU consumption obtained executing the following query: `sum(node_namespace_pod_container:container_cpu_usage_seconds_total:sum_irate{namespace="feature-lt"}) by (container)` 

[comment]: <> (MEM consumption obtained executing the following query: `sum(container_memory_working_set_bytes{namespace="feature-lt",container!="", image!=""}) by (container)`) 
-->

| Component            | Replicas | CPU  | Memory |
|----------------------|----------|------|------------|
| Orion-LD             | 2        | <0.1 | 70MB  |
| MongoDB              | 2        | <0.1 | 338MB |
| QuantumLeap          | 2        | <0.1 | 110MB |
| TimeScale            | 3        | <0.1 | 312MB |
| Keycloak             | 1        | <0.1 | 1GB |
| Keycloak PostgreSQL  | 2        | <0.1 | 170MB |
| PEP Proxy            | 2        | <0.1 | 420MB |
| Grafana              | 1        | <0.1 | 230MB |
| Grafana Metrics      | 1        | <0.1 | 200MB |
| AirQuality Simulator | 1        | <0.1 | 300MB |
| **TOTAL**            | 17       | 0.14 | 5GB |


### 2.3 Load tests results without limits and requests


| Component            | Replicas | CPU[Req,Lim] | Memory[Req,Lim] |
|----------------------|---|---|---|
| Orion-LD             |  |  |  |
| MongoDB              |  |  |  |
| QuantumLeap          |  |  |  |
| TimeScale            |  |  |  |
| Keycloak             |  |  |  |
| Keycloak PostgreSQL  |  |  |  |
| PEP Proxy            |  |  |  |


### 2.3 Load tests results

#### 2.3.1 Scenario 1

| # Devices            | #numUpdates | #updateDelay | Time To Complete | Req / min | Report |
|----------------------|---|---|---|---|---|
| 3,000                | 5 | 300s | 286s | 3,000 * 5  / 286  | Link


| Component            | Replicas | CPU[Min,Max] | Memory[Min,Max] |
|----------------------|---|---|---|
| Orion-LD             | | [,] | [,] |
| MongoDB              | | [,] | [,] |
| QuantumLeap          | | [,] | [,] |
| TimeScale            | | [,] | [,] |
| Keycloak             | | [,] | [,] |
| Keycloak PostgreSQL  | | [,] | [,] |
| PEP Proxy            | | [,] | [,] |




