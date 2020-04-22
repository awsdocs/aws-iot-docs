# AttachSecurityProfile<a name="dd-api-iot-AttachSecurityProfile"></a>

Associates an AWS IoT Device Defender security profile with one of the following target types: 
+ All devices
+ All registered devices \(things in the AWS IoT registry\)
+ All unregistered devices
+ Devices in a thing group

Each target type can have up to five security profiles associated with it\.

For more information, see [AttachSecurityProfile](https://docs.aws.amazon.com/iot/latest/apireference/API_AttachSecurityProfile.html) in the API reference\.

**Synopsis:**

```
aws iot attach-security-profile \
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

To attach a security profile to a group of devices, you must specify the ARN of the thing group that contains them\. A thing group ARN has the following format\. 

```
arn:aws:iot:region:account-id:thinggroup/thing-group-name
```

To attach a security profile to all of the registered things in an account \(ignoring unregistered things\), you must specify an ARN with the following format\.

```
arn:aws:iot:region:account-id:all/registered-things
```

To attach a security profile to all unregistered things, you must specify an ARN with the following format\.

```
arn:aws:iot:region:account-id:all/unregistered-things
```

To attach a security profile to all devices, you must specify an ARN with the following format\.

```
arn:aws:iot:region:accountid:all/things
```