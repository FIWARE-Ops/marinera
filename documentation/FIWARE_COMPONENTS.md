# Components and configuration

All the applications of this repo are expected to be Helm charts. There are components used as dependencies, and others components have been imported and modified.

In order to modify options or parameters in the helm chart values, take note the following list to modify the proper files related to each component.

> **WARNING** : changing parameters can cause the platform to stop working as is. Be sure of the changes before applying them.

Currently the FIWARE Platform is composed by the following software components and subcomponents:


## FIWARE core

- **Orion LD**: Alternative NGSI-LD Context Broker written in C/C++.

|   |   |
|---|---|
| **Helm Chart Name / Application** | [orion-ld](../applications/orion-ld) |
| - **Source Code Origin**          | Imported and modified from [FIWARE/helm-charts](https://github.com/FIWARE/helm-charts/tree/main/charts) |
| - **Values' Documentation**       | [Default values](../applications/orion-ld/chart/orion-ld/values.yaml) |
| - **Applied values**              | [Applied values](../applications/orion-ld/chart/values.yaml) |
| **Marinera README**               | [README](../applications/orion-ld/chart/README.md)
| **Product Documentation**         | [Orion-LD documentation](https://fiware-academy.readthedocs.io/en/latest/core/orion-ld/index.html) |

- **MongoDB**: NoSQL database, persistence for Orion-LD.

|   |   |
|---|---|
| **Helm Chart Name / Application** | [mongodb](../applications/mongodb) |
| - **Source Code Origin**          | Added as dependency. Source code: [bitnami/mongodb](https://github.com/bitnami/charts/tree/master/bitnami/mongodb) |
| - **Source Code README**          | [MongoDB(R) packaged by Bitnami](https://github.com/bitnami/charts/blob/master/bitnami/mongodb/README.md) |
| - **Values' Documentation**       | [Default values](https://github.com/bitnami/charts/blob/master/bitnami/mongodb/values.yaml) |
| - **Applied values**              | [Applied values](../applications/mongodb/chart/values.yaml) |
| **Marinera README**               | [README](../applications/mongodb/chart/README.md) |
| **Product Documentation**         | [MongoDB docs](https://www.mongodb.com/docs/) |



## Historical Data Layer

- **QuantumLeap**: QuantumLeap is a REST service for storing, querying and retrieving NGSI v2 and NGSI-LD (experimental support) spatial-temporal data.

|   |   |
|---|---|
| **Helm Chart Name / Application** | [quantumleap](../applications/quantumleap) |
| - **Source Code Origin**          | Added as dependency. Source code: [orchestracities/quantumleap](https://github.com/orchestracities/charts/tree/master/charts/quantumleap) |
| - **Source Code README**          | [Quantum Leap Helm Chart by Orchestracities](https://github.com/orchestracities/charts/blob/master/charts/quantumleap/README.MD) |
| - **Values' Documentation**       | [Default values](https://github.com/orchestracities/charts/blob/master/charts/quantumleap/values.yaml) |
| - **Applied values**              | [Applied values](../applications/quantumleap/chart/values.yaml) |
| **Marinera README**               | [README](../applications/quantumleap/chart/README.md) |
| **Product Documentation**         | [QuantumLeap](https://quantumleap.readthedocs.io/en/latest/) |

- **TimescaleDB**: TimescaleDB is an open-source database designed to make SQL scalable for time-series data.

|   |   |
|---|---|
| **Helm Chart Name / Application** | [timescaledb-single](../applications/timescaledb-single) |
| - **Source Code Origin**          | Imported and modified from [timescale/timescaledb-kubernetes](https://github.com/timescale/timescaledb-kubernetes/tree/master/charts/timescaledb-single) |
| - **Values' Documentation**       | [Default values](../applications/timescaledb-single/chart/timescaledb-single/values.yaml) |
| - **Applied values**              | [Applied values](../applications/timescaledb-single/chart/values.yaml) |
| **Marinera README**               | [README](../applications/timescaledb-single/chart/README.md)
| **Product Documentation**         | [Timescale Docs](https://docs.timescale.com/timescaledb/latest/) |



## Air Quality App

- **Grafana**: Grafana deployment.

|   |   |
|---|---|
| **Helm Chart Name / Application** | [grafana](../applications/grafana) |
| - **Source Code Origin**          | Added as dependency. Source code: [grafana/helm-charts](https://github.com/grafana/helm-charts/tree/main/charts/grafana) |
| - **Source Code README**          | [Grafana Helm Chart](https://github.com/grafana/helm-charts/blob/main/charts/grafana/README.md) |
| - **Values' Documentation**       | [Default values](https://github.com/grafana/helm-charts/blob/main/charts/grafana/values.yaml) |
| - **Applied values**              | [Applied values](../applications/grafana/chart/values.yaml) |
| **Marinera README**               | [README](../applications/grafana/chart/README.md) |
| **Product Documentation**         | [Grafana documentation](https://grafana.com/docs/grafana/latest/) |

- **Air Quality App**: Customized dashboards for Grafana

|   |   |
|---|---|
| **Helm Chart Name / Application** | [air-quality-app](../applications/air-quality-app) |
| - **Source Code Origin**          | Own development |
| - **Values' Documentation**       | [Default values](../applications/air-quality-app/chart/values.yaml) |
| - **Applied values**              | [Applied values](../applications/air-quality-app/chart/values.yaml) |
| **README**                        | [README](../applications/air-quality-app/chart/README.md)



## Metrics Layer

- **Grafana Metrics**: Grafana deployment for metrics visualization.

|   |   |
|---|---|
| **Helm Chart Name / Application** | [grafana-metrics](../applications/grafana-metrics) |
| - **Source Code Origin**          | Added as dependency. Source code: [grafana/helm-charts](https://github.com/grafana/helm-charts/tree/main/charts/grafana) |
| - **Source Code README**          | [Grafana Helm Chart](https://github.com/grafana/helm-charts/blob/main/charts/grafana/README.md) |
| - **Values' Documentation**       | [Default values](https://github.com/grafana/helm-charts/blob/main/charts/grafana/values.yaml) |
| - **Applied values**              | [Applied values](../applications/grafana-metrics/chart/values.yaml) |
| **Marinera README**               | [README](../applications/grafana-metrics/chart/README.md) |
| **Product Documentation**         | [Grafana documentation](https://grafana.com/docs/grafana/latest/) |



## Security Layer

- **Keycloak Server**: Single-Sign On server.

|   |   |
|---|---|
| **Helm Chart Name / Application** | [keycloak](../applications/keycloak) |
| - **Source Code Origin**          | Added as dependency. Source code: [bitnami/keycloak](https://github.com/bitnami/charts/tree/master/bitnami/keycloak) |
| - **Source Code README**          | [Keycloak packaged by Bitnami](https://github.com/bitnami/charts/blob/master/bitnami/keycloak/README.md) |
| - **Values' Documentation**       | [Default values](https://github.com/bitnami/charts/blob/master/bitnami/keycloak/values.yaml) |
| - **Applied values**              | [Applied values](../applications/keycloak/chart/values.yaml) |
| **Marinera README**               | [README](../applications/keycloak/chart/README.md) |
| **Product Documentation**         | [Keycloak docs](https://www.keycloak.org/documentation) |


- **PostgreSQL**: SQL database for Keycloak.

> **NOTE** :  This component is installed by Keycloak component as dependency. Values applied are inside keycloak Marinera values.

|   |   |
|---|---|
| **Helm Chart Name / Application** | |
| - **Source Code Origin**          | It's a dependency of keycloak. Source code: [bitnami/postgresql](https://github.com/bitnami/charts/tree/master/bitnami/postgresql) |
| - **Source Code README**          | [PostgreSQL packaged by Bitnami](https://github.com/bitnami/charts/blob/master/bitnami/postgresql/README.md) |
| - **Values' Documentation**       | [Default values](https://github.com/bitnami/charts/blob/master/bitnami/postgresql/values.yaml) |
| - **Applied values**              | [Applied values](../applications/keycloak/chart/values.yaml) (Inside keycloak applied values) |
| **Product Documentation**         | [PostgreSQL docs](https://www.postgresql.org/docs/) |


- **PEP Proxy**: Policy Enforcement Point (PEP) proxy.

|   |   |
|---|---|
| **Helm Chart Name / Application** | [keycloak-pep](../applications/keycloak-pep) |
| - **Source Code Origin**          | Own development |
| - **Values' Documentation**       | [Default values](../applications/keycloak-pep/chart/values.yaml) |
| - **Applied values**              | [Applied values](../applications/keycloak-pep/chart/values.yaml) |
| **README**                        | [README](../applications/keycloak-pep/chart/README.md)


## Data Simulation Layer

- **Air Quality Simulator**: Air Quality Data (devices) simulator.

|   |   |
|---|---|
| **Helm Chart Name / Application** | [airquality-simulator](../applications/airquality-simulator) |
| - **Source Code Origin**          | Own development |
| - **Values' Documentation**       | [Default values](../applications/airquality-simulator/chart/values.yaml) |
| - **Applied values**              | [Applied values](../applications/airquality-simulator/chart/values.yaml) |
| **README**                        | [README](../applications/airquality-simulator/chart/README.md)
