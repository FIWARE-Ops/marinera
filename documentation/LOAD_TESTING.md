# Load Testing

You can run load tests to calculate the maximum throughput for your deployment. This section shows how to run tests using the FIWARE load testing tool and some reports of tests run in a demo scenario.

## 1. Run your load tests

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

<!---


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
--->

### 2.3 Load tests results


| Scenario | numDevices | numUpdates | updateDelay | Duration | Req/s | OK/KO | Report |
|-----------|-----------|-------------|--------------|----------|-------|----------|----------|
| [#1](#231-scenario-1)        |100        | 300         | 1s           | 547s     | 55.109| 30200 / 0 | [Link](https://fiware-ops.github.io/marinera/documentation/load-test-reports/scenario1/report.html) <!--- entityupdatewithsinglesubscriptionsimulation-20220527092140490-->
| [#2](#232-scenario-2)        | 300       | 300         | 1s           |  1638s   | 55.104| 90315 / 285 | [Link](https://fiware-ops.github.io/marinera/documentation/load-test-reports/scenario2/report.html) <!--- entityupdatewithsinglesubscriptionsimulation-20220527094623925 -->
| [#3](#233-scenario-3)        | 500       | 50         | 1s           |  517s | 49.591  | 25688 / 312 | [Link](https://fiware-ops.github.io/marinera/documentation/load-test-reports/scenario3/report.html) <!--- entityupdatewithsinglesubscriptionsimulation-20220527102857129 -->

* **Scenario**: This is the scenario number. You can use this number to check resource consumption of the test in the tables below.
* **numDevices**: Number of simultaneous devices reporting data.
* **numUpdates**: Number of reports per device during the load test.
* **updateDelay**: Time between reports.
* **Duration**: This value is part of the Gatling report and shows the number of seconds that the scenario took to execute.  
* **Req/s**: This value is part of the Gatling report and shows the number of successful requests from the Gatling application running locally.
* **OK/KO**: This value is part of the Gatling report and shows the number of correct and failed requests from the Gatling application running locally.
* **Report**: Link to the corresponding Gatling report page.

#### 2.3.1 Scenario 1

| Component            | Replicas | CPU Min | CPU Max | CPU Avg | MEM Min | MEM Max | MEM Avg | 
|----------------------|---|------:|------:|------:|------------:|------------:|------------:|
| Orion-LD             | 2 | 0.02 | 0.09 | 0.05 | 31.16 MiB  | 34.40 MiB  | 32.46 MiB  |
| MongoDB              | 2 | 0.08 | 0.20 | 0.15 | 264.72 MiB | 294.98 MiB | 283.47 MiB |
| QuantumLeap          | 2 | 0.00 | 0.33 | 0.23 | 69.56 MiB  | 94.17 MiB  | 87.32 MiB  |
| TimeScale            | 3 | 0.03 | 0.10 | 0.07 | 323.75 MiB | 347.16 MiB | 336.56 MiB |
| Keycloak             | 1 | 1.88 | 4.71 | 3.49 | 833.83 MiB | 924.51 MiB | 891.65 MiB |
| Keycloak PostgreSQL  | 2 | 0.01 | 0.03 | 0.02 | 134.27 MiB | 137.59 MiB | 135.41 MiB |
| PEP Proxy            | 2 | 0.08 | 0.37 | 0.17 | 351.36 MiB | 371.70 MiB | 362.94 MiB |


#### 2.3.2 Scenario 2

| Component            | Replicas | CPU Min | CPU Max | CPU Avg | MEM Min | MEM Max | MEM Avg |
|----------------------|---|------:|------:|------:|------------:|------------:|------------:|
| Orion-LD             | 2 | 0.04 | 0.09 | 0.06 | 28.45 MiB | 34.88 MiB  | 31.84 MiB  | 
| MongoDB              | 2 | 0.12 | 0.32 | 0.23 | 301.05 MiB| 430.62 MiB | 362.06 MiB | 
| QuantumLeap          | 2 | 0.05 | 0.34 | 0.23 | 93.90 MiB | 94.41 MiB  | 94.07 MiB  | 
| TimeScale            | 3 | 0.03 | 0.12 | 0.08 | 345.18 MiB| 439.01 MiB | 390.26 MiB | 
| Keycloak             | 1 | 2.24 | 4.94 | 3.83 | 739.38 MiB| 1.05 GiB   | 961.84 MiB | 
| Keycloak PostgreSQL  | 2 | 0.01 | 0.03 | 0.02 | 143.84 MiB| 147.87 MiB | 145.74 MiB | 
| PEP Proxy            | 2 | 0.08 | 0.31 | 0.16 | 299.29 MiB| 585.83 MiB | 343.41 MiB | 



#### 2.3.3 Scenario 3

| Component            | Replicas | CPU Min | CPU Max | CPU Avg | MEM Min | MEM Max | MEM Avg |
|----------------------|---|------:|------:|------:|------------:|------------:|------------:|
| Orion-LD             | 2 | 0.03 | 0.08 | 0.06 | 28.45 MiB  | 32.63 MiB  | 29.91 MiB  |
| MongoDB              | 2 | 0.15 | 0.32 | 0.23 | 360.81 MiB | 459.20 MiB | 400.15 MiB |
| QuantumLeap          | 2 | 0.06 | 0.23 | 0.19 | 93.90 MiB  | 94.17 MiB  | 94.00 MiB  |
| TimeScale            | 3 | 0.02 | 0.13 | 0.07 | 443.46 MiB | 521.72 MiB | 458.30 MiB |
| Keycloak             | 1 | 1.67 | 4.87 | 3.39 | 1.08 GiB   | 1.09 GiB   | 1.09 GiB   |
| Keycloak PostgreSQL  | 2 | 0.01 | 0.03 | 0.02 | 132.92 MiB | 136.04 MiB | 134.06 MiB |
| PEP Proxy            | 2 | 0.08 | 0.24 | 0.16 | 323.41 MiB | 332.26 MiB | 328.28 MiB |


## 3. Limits and Requests

Load tests help us to estimate the resources (CPU and Memory) that each component will demand when they run under high pressure. To get a good QoS for our pods, we are going to set CPU and memory requests and limits for all the components of our deployment:


| Component            | Replicas | CPU Req | CPU Lim | MEM Req | MEM Lim |
|----------------------|----------|---------|---------|---------|---------|
| Orion-LD             | 2        | 10m     | 250m    | 50Mi    | 1GB     |
| MongoDB              | 2        | 50m     | 500m    | 300Mi   | 1.5GB   |
| MongoDB - Arbiter    | 1        | 50m     | 500m    | 300Mi   | 1.5GB   |
| MongoDB - Metrics    | 2        | 50m     | 200m    | 50Mi    | 500Mi   |
| QuantumLeap          | 2        | 10m     | 500m    | 50Mi    | 250Mi   |
| TimeScale            | 3        | 10m     | 250m    | 300Mi   | 800Mi   |
| Keycloak             | 1        | 100m    | 6       | 400Mi   | 2GB     |
| Keycloak PostgreSQL  | 2        | 10m     | 250m    | 50Mi    | 250Mi   |
| PEP Proxy            | 2        | 50m     | 1       | 300Mi   | 800Mi   |
| Grafana              | 1        | 10m     | 1       | 100Mi   | 1GB     |
| Grafana Metrics      | 1        | 10m     | 250m    | 50Mi    | 250Mi   |
| AirQuality Simulator | 1        | 10m     | 1       | 300Mi   | 800Mi   |

