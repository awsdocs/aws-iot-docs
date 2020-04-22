# ListSecurityProfiles<a name="dd-api-iot-ListSecurityProfiles"></a>

Lists the AWS IoT Device Defender security profiles for your AWS account\. You can use filters to list only those security profiles associated with a thing group or only those associated with your account\.

For more information, see [ListSecurityProfiles](https://docs.aws.amazon.com/iot/latest/apireference/API_ListSecurityProfiles.html) in the API reference\.

 **Synopsis:**

```
aws iot  list-security-profiles \
    [--next-token <value>] \
    [--max-results <value>]  \
    [--cli-input-json <value>] \
    [--generate-cli-skeleton]
```

**Example**  
The following example lists all security profiles that have behaviors that use the dimension named *TopicFilterForAuthMessages*\.  

```
aws iot list-security-profiles \
--dimension-name TopicFilterForAuthMessages
```
The response looks like the following\.  

```
{
    "securityProfileIdentifiers": [
        {
            "name": "ExpectedBehaviorsForSmartDevice",
            "arn": "arn:aws:iot:eu-west-2:123456789012:securityprofile/ExpectedBehaviorsForSmartDevice"
        }
    ]
}
```

`cli-input-json` format:

```
{
  "nextToken": "string",
  "maxResults": "integer"
}
```

**Output:**

```
{
  "securityProfileIdentifiers": [
    {
      "name": "string",
      "arn": "string"
    }
  ],
  "nextToken": "string"
}
```