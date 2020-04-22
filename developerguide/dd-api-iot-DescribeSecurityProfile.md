# DescribeSecurityProfile<a name="dd-api-iot-DescribeSecurityProfile"></a>

Gets information about an AWS IoT Device Defender security profile\.

For more information, see [DescribeSecurityProfile](https://docs.aws.amazon.com/iot/latest/apireference/API_DescribeSecurityProfile.html) in the API reference\.

**Synopsis:**

```
aws iot  describe-security-profile \
    --security-profile-name <value>  \
    [--cli-input-json <value>] \
    [--generate-cli-skeleton]
```

`cli-input-json` format:

```
{
  "securityProfileName": "string"
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
      "metricDimension": [ "string ],
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