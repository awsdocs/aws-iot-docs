# UpdateDimension<a name="dd-api-iot-UpdateDimension"></a>

Update a dimension to change the scope for the metric in a security profile for AWS IoT Device Defender\. The scope is defined by providing a value or pattern that is used as a filter\. For TOPIC\_FILTER dimensions, this filter specifies the subset of MQTT topics to which the behavior is applied\.

For more information, see [UpdateDimension](https://docs.aws.amazon.com/iot/latest/apireference/API_UpdateDimension.html) in the API reference\.

**Synopsis:**

```
aws iot update-dimension \
    --name <value> \
    --string-values <value> \
    [--client-request-token <value>] \
    [--expected-version <value>]  \
    [--cli-input-json <value>] \
    [--generate-cli-skeleton]
```

**Example**  
This example updates a dimension named TopicFilterForAuthMessages, setting the pattern to that filters to MQTT topics that match the pattern device/\+/auth\. You apply the dimension to a behavior in a security profile\.  

```
aws iot update-dimension \
    --name TopicFilterForAuthMessages --string-values device/+/auth
```

The response resembles the following\.

```
{
    "arn": "arn:aws:iot:eu-west-2:123456789012:dimension/TopicFilterForAuthMessages",
    "creationDate": 1578620223.255,
    "lastModifiedDate": 1578620223.255,
    "name": "TopicFilterForAuthMessages",
    "type": "TOPIC_FILTER",
    "string-values": "device/+/auth"
}
```

`cli-input-json` format:

```
{
   "string-values": "string"
}
```

Output:

```
{
    "arn": "string",
    "creationDate": number,
    "lastModifiedDate": number,
    "name": "string",
    "type": "string",
    "string-values": "string"
}
```