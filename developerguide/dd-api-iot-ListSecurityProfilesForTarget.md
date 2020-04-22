# ListSecurityProfilesForTarget<a name="dd-api-iot-ListSecurityProfilesForTarget"></a>

Lists the AWS IoT Device Defender security profiles attached to a target \(thing group\)\.

For more information, see [ListSecurityProfilesForTarget](https://docs.aws.amazon.com/iot/latest/apireference/API_ListSecurityProfilesForTarget.html) in the API reference\.

**Synopsis:**

```
aws iot  list-security-profiles-for-target \
    [--next-token <value>] \
    [--max-results <value>] \
    [--recursive | --no-recursive] \
    --security-profile-target-arn <value>  \
    [--cli-input-json <value>] \
    [--generate-cli-skeleton]
```

 `cli-input-json` format:

```
{
  "nextToken": "string",
  "maxResults": "integer",
  "recursive": "boolean",
  "securityProfileTargetArn": "string"
}
```

**Output:**

```
{
  "securityProfileTargetMappings": [
    {
      "securityProfileIdentifier": {
        "name": "string",
        "arn": "string"
      },
      "target": {
        "arn": "string"
      }
    }
  ],
  "nextToken": "string"
}
```