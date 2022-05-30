# How to connect devices

## Orion-LD

The platform provides an endpoint for accessing the [Orion-LD](https://github.com/FIWARE/context.Orion-LD) API.

> [Orion-LD](https://github.com/FIWARE/context.Orion-LD) is a Context Broker and [CEF](https://ec.europa.eu/cefdigital/wiki/display/CEFDIGITAL/CEF+Digital+Home)
[building block](https://ec.europa.eu/cefdigital/wiki/display/CEFDIGITAL/What+is+a+building+Block) for context data management which supports both the [NGSI-LD](https://en.wikipedia.org/wiki/NGSI-LD) and the
[NGSI-v2](https://fiware.github.io/specifications/OpenAPI/ngsiv2) APIs.

Through the Orion-LD API, CRUD operations are performed on entities that represent contextual data of a smart city.

For more information about Orion-LD, please refer to the following links:

| [Orion-LD Documentation](https://github.com/FIWARE/context.Orion-LD/tree/develop/doc/manuals-ld) | [Orion-LD Academy](https://fiware-academy.readthedocs.io/en/latest/core/orion-ld) |
| ----------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------- |

For additional information on API v2, please refer to the following links:

| [Orion Context Broker Documentation](https://fiware-orion.rtfd.io) | [Orion Context Broker Academy](https://fiware-academy.readthedocs.io/en/latest/core/orion) |
|---|---|

## Security layer: Authentication and authorization flow

This project includes a security layer that provides authenticated and authorized access over Orion-LD. Any component's
API request will be evaluated to determine whether or not the user making the request can perform that action. Thus, any
request must contain an *Authorization* header that identifies the user.

More information about the security layer and the authentication process can be found
in [Security Layer](./documentation/SECURITY_LAYER.md)

## Step-by-step tutorial: Create and retrieve an entity in Orion-LD using v2 API

This tutorial aims to provide a small collection of curl requests for the creation of an entity in Orion-LD.

### Create user access token

```bash
curl --location --request POST 'https://<keycloak-endpoint>/realms/fiware-server/protocol/openid-connect/token' \
--header 'Content-Type: application/x-www-form-urlencoded' \
--data-urlencode 'username=admin-user' \
--data-urlencode 'password=admin-user' \
--data-urlencode 'grant_type=password' \
--data-urlencode 'client_id=orion-pep' \
--data-urlencode 'client_secret=978ad148-d99b-406d-83fc-578597290a79' \
```

The response must be a 200 OK with a JSON object that includes an *access_token* attribute. This attribute needs to be
added as a header to the requests made to Orion-LD.

### Create entity

```bash
curl --location --request POST 'https://<orionld-endpoint>/v2/entities' \
--header 'fiware-service: AirQuality' \
--header 'fiware-servicepath: /data' \
--header 'Authorization: Bearer <access-token>' \
--header 'Content-Type: application/json' \
--data-raw '{
  "id": "urn:ngsi:AirQualityObserved:step-by-step-tutorial",
  "type": "AirQualityObserved",
  "no2": {
    "type": "Number",
    "value": 980.491484395
  },
  "o3": {
    "type": "Number",
    "value": 242.418263041
  },
  "co": {
    "type": "Number",
    "value": 39028.230234928
  },
  "so2": {
    "type": "Number",
    "value": 1771.798236043
  },
  "humidity": {
    "type": "Number",
    "value": 0.561590432
  },
  "temperature": {
    "type": "Number",
    "value": 21.905335231
  },
  "pm1": {
    "type": "Number",
    "value": 94.957508471
  },
  "pm10": {
    "type": "Number",
    "value": 39.940119756
  },
  "pm25": {
    "type": "Number",
    "value": 167.020927107
  },
  "name": {
    "type": "Text",
    "value": "HOP Ubiquitous"
  },
  "location": {
    "type": "geo:json",
    "value": {
      "type": "Point",
      "coordinates": [
        -0.5075435,
        38.3579029
      ]
    }
  },
  "operationalStatus": {
    "type": "Text",
    "value": "CONNECTED"
  }
'
```

### Retrieve created entity

```bash
curl --location -g --request GET 'https://<orionld-endpoint>/v2/entities/urn:ngsi:AirQualityObserved:step-by-step-tutorial' \
--header 'fiware-service: AirQuality' \
--header 'fiware-servicepath: /data' \
--header 'Authorization: Bearer <access-token>'
```

If the Air Quality App has been enabled, this example will have added an entity that can be displayed on the Air Quality
Dashboards in Grafana.



