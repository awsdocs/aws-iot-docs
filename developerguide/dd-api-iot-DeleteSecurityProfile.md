# DeleteSecurityProfile<a name="dd-api-iot-DeleteSecurityProfile"></a>

Deletes an AWS IoT Device Defender security profile\.

For more information, see [DeleteSecurityProfile](https://docs.aws.amazon.com/iot/latest/apireference/API_DeleteSecurityProfile.html) in the API reference\.

 **Synopsis:**

```
aws iot  delete-security-profile \
    --security-profile-name <value> \
    [--expected-version <value>]  \
    [--cli-input-json <value>] \
    [--generate-cli-skeleton]
```

 `cli-input-json` format:

```
{
  "securityProfileName": "string",
  "expectedVersion": "long"
}
```

Output:

None