# Pre\-provisioning hooks<a name="pre-provisioning-hook"></a>

When using AWS IoT fleet provisioning, you can set up a Lambda function to validate parameters passed from the device before allowing the device to be provisioned\. This Lambda function must exist in your account before you provision a device because it's called every time a device sends a request through [RegisterThing](fleet-provision-api.md#register-thing)\. For devices to be provisioned, your Lambda function must accept the input object and return the output object described in this section\. The provisioning proceeds only if the Lambda function returns an object with `"allowProvisioning": True`\.

**Important**  
AWS recommends using pre\-provisioning hooks when creating provisioning templates to allow more control of which and how many devices your account onboards\.

## Pre\-provision hook input<a name="pre-provisioning-hook-input"></a>

AWS IoT sends this object to the Lambda function when a device registers with AWS IoT\.

```
{
    "claimCertificateId" : "string",
    "certificateId" : "string",
    "certificatePem" : "string",
    "templateArn" : "arn:aws:iot:us-east-1:1234567890:provisioningtemplate/MyTemplate",
    "clientId" : "221a6d10-9c7f-42f1-9153-e52e6fc869c1",
    "parameters" : {
        "key" : "value",
        ...
    }
}
```

The `parameters` object passed to the Lambda function contains the properties in the `parameters` argument passed in the [RegisterThing](fleet-provision-api.md#register-thing) request payload\. 

## Pre\-provision hook return value<a name="pre-provisioning-hook-output"></a>

The Lambda function must return a response that indicates whether it has authorized the provisioning request and the values of any properties to override\.

The following is an example of a successful response from the pre\-provisioning function\.

```
{
    "allowProvisioning": true,
    "parameterOverrides" : {
        "Key": "newCustomValue",
        ...
    }
}
```

`"parameterOverrides"` values will be added to `"parameters"` parameter in the [RegisterThing](fleet-provision-api.md#register-thing) response payload\.

**Note**  
If the Lambda function fails or doesn't return the `"allowProvisioning"` parameter in the response, the provisioning request will fail and the error will be returned in the response\.
The Lambda function must finish running and return within 5 seconds, otherwise the provisioning request fails\.