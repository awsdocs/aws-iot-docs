# ListTargetsForSecurityProfile<a name="dd-api-iot-ListTargetsForSecurityProfile"></a>

Lists the targets \(thing groups\) associated with a given AWS IoT Device Defender security profile\.

For more information, see [ListTargetsForSecurityProfile](https://docs.aws.amazon.com/iot/latest/apireference/API_ListTargetsForSecurityProfile.html) in the API reference\.

**Synopsis:**

```
aws iot  list-targets-for-security-profile \
    --security-profile-name <value> \
    [--next-token <value>] \
    [--max-results <value>]  \
    [--cli-input-json <value>] \
    [--generate-cli-skeleton]
```

 `cli-input-json` format:

```
{
  "securityProfileName": "string",
  "nextToken": "string",
  "maxResults": "integer"
}
```

**Output:**

```
{
  "securityProfileTargets": [
    {
      "arn": "string"
    }
  ],
  "nextToken": "string"
}
```