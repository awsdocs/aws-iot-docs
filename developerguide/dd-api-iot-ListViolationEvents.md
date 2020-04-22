# ListViolationEvents<a name="dd-api-iot-ListViolationEvents"></a>

Lists the AWS IoT Device Defender security profile violations discovered during the given time period\. You can use filters to limit the results to those alerts issued for a particular security profile, behavior or thing \(device\)\.

For more information, see [ListViolationEvents](https://docs.aws.amazon.com/iot/latest/apireference/API_ListViolationEvents.html) in the API reference\.

**Synopsis:**

```
aws iot  list-violation-events \
    --start-time <value> \
    --end-time <value> \
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
  "startTime": "timestamp",
  "endTime": "timestamp",
  "thingName": "string",
  "securityProfileName": "string",
  "nextToken": "string",
  "maxResults": "integer"
}
```

**Output:**

```
{
  "violationEvents": [
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
      "metricValue": {
        "count": "long",
        "cidrs": [
          "string"
        ],
        "ports": [
          "integer"
        ]
      },
      "violationEventType": "string",
      "violationEventTime": "timestamp"
    }
  ],
  "nextToken": "string"
}
```