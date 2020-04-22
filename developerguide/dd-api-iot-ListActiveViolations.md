# ListActiveViolations<a name="dd-api-iot-ListActiveViolations"></a>

Lists the active violations for a given AWS IoT Device Defender security profile\.

For more information, see [ListActiveViolations](https://docs.aws.amazon.com/iot/latest/apireference/API_ListActiveViolations.html) in the API reference\.

**Synopsis:**

```
aws iot  list-active-violations \
    [--thing-name <value>] \
    [--security-profile-name <value>] \
    [--next-token <value>] \
    [--max-results <value>]  \
    [--cli-input-json <value>] \
    [--generate-cli-skeleton]
```

 `cli-input-json` format:

```
{
  "thingName": "string",
  "securityProfileName": "string",
  "nextToken": "string",
  "maxResults": "integer"
}
```

**Output:**

```
{
  "activeViolations": [
    {
      "violationId": "string",
      "thingName": "string",
      "securityProfileName": "string",
      "behavior": {
        "name": "string",
        "dimensionNames": [ "string" ],
        "metric": "string",
        "criteria": {
          "comparisonOperator": "string",
          "value": {
            "count": "long",
            "cidrs": [
              "string"
            ],
            "ports": [
              "integer"
            ]
          },
          "durationSeconds": "integer",
          "consecutiveDatapointsToAlarm": "integer",
          "consecutiveDatapointsToClear": "integer",
          "statisticalThreshold": {
            "statistic": "string"
          }
        }
      },
      "lastViolationValue": {
        "count": "long",
        "cidrs": [
          "string"
        ],
        "ports": [
          "integer"
        ]
      },
      "lastViolationTime": "timestamp",
      "violationStartTime": "timestamp"
    }
  ],
  "nextToken": "string"
}
```