# ValidateSecurityProfileBehaviors<a name="dd-api-iot-ValidateSecurityProfileBehaviors"></a>

Validates an AWS IoT Device Defender security profile behaviors specification\.

For more information, see [ValidateSecurityProfileBehaviors](https://docs.aws.amazon.com/iot/latest/apireference/API_ValidateSecurityProfileBehaviors.html) in the API reference\.

**Synopsis:**

```
aws iot  validate-security-profile-behaviors \
    --behaviors <value>  \
    [--cli-input-json <value>] \
    [--generate-cli-skeleton]
```

 `cli-input-json` format:

```
{
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
  ]
}
```

Output:

```
{
  "valid": "boolean",
  "validationErrors": [
    {
      "errorMessage": "string"
    }
  ]
}
```