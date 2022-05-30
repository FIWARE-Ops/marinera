# How to persist data

The platform uses the QuantumLeap component linked to a TimescaleDB database to provide persistence of each of the
modifications in the entities represented in Orion-LD. Each entity modification is reported through the subscription
mechanism provided by Orion-LD.

A subscription has been created on the Air Quality data reported by the simulator with the following format:

```json
[
  {
    "id": "62909436c874d891a23a0a14",
    "description": "Subscription for simulated air-quality data",
    "status": "active",
    "subject": {
      "entities": [
        {
          "idPattern": ".*",
          "type": "AirQualityObserved"
        }
      ],
      "condition": {
        "attrs": []
      }
    },
    "notification": {
      "attrs": [
        "co",
        "so2",
        "no2",
        "o3",
        "pm1",
        "pm25",
        "pm10"
      ],
      "attrsFormat": "normalized",
      "httpCustom": {
        "url": "http://quantumleap-quantumleap:8668/v2/notify",
        "headers": {
          "fiware-service": "AirQuality",
          "fiware-servicepath": "/data"
        }
      }
    }
  }
]
```

Thus, every time a new entity is reported, a notification of this value is automatically sent to QuantumLeap.
QuantumLeap processes the reported entity and stores it in TimescaleDB.

For more information on the subscription process implemented by Orion-LD, please refer to the following links:

- [FIWARE NGSI APIv2 Walkthrough](https://fiware-orion.readthedocs.io/en/master/user/walkthrough_apiv2.html)
- [FIWARE Tutorials: Subscribing to Changes of State](https://github.com/FIWARE/tutorials.Subscriptions)
- [FIWARE NGSI v2 API](https://fiware.github.io/specifications/OpenAPI/ngsiv2)
