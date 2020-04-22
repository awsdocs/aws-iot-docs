# CreateDimension<a name="dd-api-iot-CreateDimension"></a>

Create a dimension to control the scope for the behavior in a security profile for AWS IoT Device Defender\. The scope is defined by providing a value or pattern that is used as a filter\. TOPIC\_FILTER is the only dimension currently supported\. For TOPIC\_FILTER dimensions, this filter specifies the subset of MQTT topics to which the behavior is applied\. MQTT wildcards \# and \+, and also the $\{iot:ClientId\} variable are supported by dimensions\.

To add a dimension to a security profile, see [CreateSecurityProfile](dd-api-iot-CreateSecurityProfile.md)\.

For more information, see [CreateDimension](https://docs.aws.amazon.com/iot/latest/apireference/API_CreateDimension.html) in the API reference\.

**Synopsis:**

```
aws iot create-dimension \
    --name <value> \
    --type <value> \
    --string-values <value> \
    [--client-request-token <value>] \
    [--expected-version <value>]  \
    [--cli-input-json <value>] \
    [--generate-cli-skeleton]
```

**Note**  
If you omit the `clientRequestToken`, a unique client request token is generated automatically\.

**Example**  
This example creates a dimension named TopicFilterForAuthMessages that filters to MQTT topics that match the pattern device/\+/auth\. You apply the dimension to a behavior in a security profile\.  

```
aws iot create-dimension \
    --name TopicFilterForAuthMessages --type TOPIC_FILTER --string-values device/+/auth
```

The response resembles the following\.

```
{
    "name": "TopicFilterForAuthMessages",
    "arn": "arn:aws:iot:eu-west-2:123456789012:dimension/TopicFilterForAuthMessages"
}
```

**Example**  
This example creates a dimension named MyFavoriteTopics with multiple topics\.  

```
aws iot create-dimension \
    --name MyFavoriteTopics --type TOPIC_FILTER \ --string-values topic1 topic2
```

`cli-input-json` format:

```
{
   "clientRequestToken": "string",
   "tags": [ 
      { 
         "Key": "string",
         "Value": "string"
      }
   ],
   "type": "string",
   "string-values": "string"
}
```

Output:

```
{
   "arn": "string",
   "name": "string"
}
```