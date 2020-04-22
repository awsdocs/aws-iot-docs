# DetachSecurityProfile<a name="dd-api-iot-DetachSecurityProfile"></a>

Disassociates an AWS IoT Device Defender security profile from a thing group or from this AWS account\.

For more information, see [DetachSecurityProfile](https://docs.aws.amazon.com/iot/latest/apireference/API_DetachSecurityProfile.html) in the API reference\.

 **Synopsis:**

```
aws iot  detach-security-profile \
    --security-profile-name <value> \
    --security-profile-target-arn <value>  \
    [--cli-input-json <value>] \
    [--generate-cli-skeleton]
```

 `cli-input-json` format:

```
{
  "securityProfileName": "string",
  "securityProfileTargetArn": "string"
}
```

**Output:**

None