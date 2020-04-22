# CreateSecurityProfile<a name="dd-api-iot-CreateSecurityProfile"></a>

Creates an AWS IoT Device Defender security profile\.

For more information, see [CreateSecurityProfile](https://docs.aws.amazon.com/iot/latest/apireference/API_CreateSecurityProfile.html) in the API reference\.

**Synopsis:**

```
aws iot  create-security-profile \
    --security-profile-name <value> \
    [--security-profile-description <value>] \
    [--behaviors <value>] \
    [--alert-targets <value>] \
    [--additional-metrics-to-retain <value>] \
    [--tags <value>]  \
    [--cli-input-json <value>] \
    [--generate-cli-skeleton]
```

**Example**  
To define a behavior with a dimension, use the `MetricDimension` field next to the metric\. To retain a metric with a dimension, use the `AdditionalMetricsToRetainV2` field\. `MetricDimension` has an operator field that allows inverting topic filters\.  
The following example shows a definition that there should be no messages sent to dimensions other than `FavoriteTopics`\. Also, this definition enables retaining metrics for the number of messages sent to the auth topic\.  

```
aws iot create-security-profile \
--security-profile-name security-profile-for-smart-lights \
--behaviors '[{
"name": "num-messages-sent-to-unexpected-topic",
"metric": "aws:num-messages-sent",
"metricDimension": {
    "dimensionName": "FavoriteTopics",
    "operator": "NOT_IN"
},
"criteria": {
    "comparisonOperator": "less-than",
    "value": {"count": 1},
    "durationSeconds": 300
}}]' \
--additional-metrics-to-retain-v2 '[{
    "metric": "aws:num-messages-sent",
    "metricDimension": {"dimensionName": "TopicFilterForAuthMessages"}
}]'
```

**Example**  
`cli-input-json` format:  

```
{
   "additionalMetricsToRetainV2": [ "string" ],
   "alertTargets": { 
      "string" : { 
         "alertTargetArn": "string",
         "roleArn": "string"
      }
   },
   "behaviors": [ 
      { 
         "criteria": { 
            "comparisonOperator": "string",
            "consecutiveDatapointsToAlarm": number,
            "consecutiveDatapointsToClear": number,
            "durationSeconds": number,
            "statisticalThreshold": { 
               "statistic": "string"
            },
            "value": { 
               "cidrs": [ "string" ],
               "count": number,
               "ports": [ number ]
            }
         },
         "metricDimension": [ "string" ],
         "metric": "string",
         "name": "string"
      }
   ],
   "securityProfileDescription": "string",
   "tags": [ 
      { 
         "Key": "string",
         "Value": "string"
      }
   ]
}
```

Output:

```
{
  "securityProfileName": "string",
  "securityProfileArn": "string"
}
```