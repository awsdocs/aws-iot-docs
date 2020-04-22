# DeleteDimension<a name="dd-api-iot-DeleteDimension"></a>

Deletes an AWS IoT Device Defender dimension\. You can delete only the dimensions that are not being used in any security profile\.

To delete a dimension, you must first detach the dimension from the behavior in the security profile\. For information, see [UpdateSecurityProfile](dd-api-iot-UpdateSecurityProfile.md)\.

For more information, see [DeleteDimension](https://docs.aws.amazon.com/iot/latest/apireference/API_DeleteDimension.html) in the API reference\.

 **Synopsis:**

```
aws iot  delete-dimension \
    --name <value> \
    [--cli-input-json <value>] \
    [--generate-cli-skeleton]
```

**Example**

This example deletes the dimension named TopicFilterForAuthMessages\.

```
aws iot delete-dimension --name TopicFilterForAuthMessages
```

This command returns no response\.

`cli-input-json` format:

```
{
  "name": "string",
  "arn": "string",
  "type": "TOPIC_FILTER",
  "description": "string"
}
```

Output:

None