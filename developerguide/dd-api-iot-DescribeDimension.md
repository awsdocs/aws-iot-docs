# DescribeDimension<a name="dd-api-iot-DescribeDimension"></a>

Gets information about a dimension that can be used within behaviors in an AWS IoT Device Defender security profile\.

For more information, see [DescribeDimension](https://docs.aws.amazon.com/iot/latest/apireference/API_DescribeDimension.html) in the API reference\.

 **Synopsis:**

```
aws iot  describe-dimension \
    --name <value>  \
    [--cli-input-json <value>] \
    [--generate-cli-skeleton]
```

**Example**  
This example gets the definition for the dimension named *TopicFilterForAuthMessages*\.  

```
aws iot describe-dimension --name TopicFilterForAuthMessages
```
The response resembles the following,  

```
{
    "name": "TopicFilterForAuthMessages",
    "arn": "arn:aws:iot:eu-west-2:1123456789012:dimension/TopicFilterForAuthMessages",
    "type": "TOPIC_FILTER",
    "stringValues": [
        "device/+/auth"
    ],
    "creationDate": 1578620223.255,
    "lastModifiedDate": 1578620223.255
}
```
 `cli-input-json` format:  

```
{
  "name": "string"
}
```

**Output:**

```
{
   "arn": "string",
   "creationDate": number,
   "lastModifiedDate": number,
   "name": "string",
   "type": "string",
   "string-values": "string",
    "additional-metrics-to-retain-v2": [ "string" ]
}
```

**Example**  

```
aws iot describe-dimension --name myAdminTopicDimension
```
The response looks like the following\.  

```
{
    "name": "TopicFilterForAuthMessages",
    "arn": "arn:aws:iot:eu-west-2:123456789012:dimension/TopicFilterForAuthMessages",
    "type": "TOPIC_FILTER",
    "string-values": "device/+/auth",
    "creationDate": 1578620223.255,
    "lastModifiedDate": 1578620223.255
}
```