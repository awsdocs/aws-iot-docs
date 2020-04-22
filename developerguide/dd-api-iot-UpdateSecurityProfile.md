# UpdateSecurityProfile<a name="dd-api-iot-UpdateSecurityProfile"></a>

Updates an AWS IoT Device Defender security profile\.

For more information, see [UpdateSecurityProfile](https://docs.aws.amazon.com/iot/latest/apireference/API_UpdateSecurityProfile.html) in the API reference\.

**Synopsis:**

```
aws iot  update-security-profile \
    --security-profile-name <value> \
    [--security-profile-description <value>] \
    [--behaviors <value>] \
    [--alert-targets <value>] \
    [--additional-metrics-to-retain <value>] \
    [--delete-behaviors | --no-delete-behaviors] \
    [--delete-alert-targets | --no-delete-alert-targets] \
    [--delete-additional-metrics-to-retain | --no-delete-additional-metrics-to-retain] \
    [--expected-version <value>]  \
    [--cli-input-json <value>] \
    [--generate-cli-skeleton]
```

 `cli-input-json` format:

```
{
  "securityProfileName": "string",
  "securityProfileDescription": "string",
  "behaviors": [
    {
      "name": "string",
      "metricDimension": [ "string" ],
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
    }
  ],
  "alertTargets": {
    "string": {
      "alertTargetArn": "string",
      "roleArn": "string"
    }
  },
  "additionalMetricsToRetain": [
    "string"
  ],
  "deleteBehaviors": "boolean",
  "deleteAlertTargets": "boolean",
  "deleteAdditionalMetricsToRetain": "boolean",
  "expectedVersion": "long"
}
```

**Output:**

```
{
  "securityProfileName": "string",
  "securityProfileArn": "string",
  "securityProfileDescription": "string",
  "behaviors": [
    {
      "name": "string",
      "metricDimension": [ "string" ],
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
    }
  ],
  "alertTargets": {
    "string": {
      "alertTargetArn": "string",
      "roleArn": "string"
    }
  },
  "additionalMetricsToRetain": [
    "string"
  ],
  "version": "long",
  "creationDate": "timestamp",
  "lastModifiedDate": "timestamp"
}
```