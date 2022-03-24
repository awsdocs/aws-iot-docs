# Pre\-provisioning hooks<a name="pre-provisioning-hook"></a>

AWS recommends using pre\-provisioning hook functions when creating provisioning templates to allow more control of which and how many devices your account onboards\. Pre\-provisioning hooks are Lambda functions that validate parameters passed from the device before allowing the device to be provisioned\. This Lambda function must exist in your account before you provision a device because it's called every time a device sends a request through [RegisterThing](fleet-provision-api.md#register-thing)\.

**Important**  
Be sure to include the `source-arn` or `source-account` in the global condition context keys of the policies attached to your Lambda action to prevent permission manipulation\. Fore more informataion about this, see [Cross\-service confused deputy prevention](cross-service-confused-deputy-prevention.md)\.

For devices to be provisioned, your Lambda function must accept the input object and return the output object described in this section\. The provisioning proceeds only if the Lambda function returns an object with `"allowProvisioning": True`\.

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
        "string" : "string",
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

`"parameterOverrides"` values will be added to `"parameters"` parameter of the [RegisterThing](fleet-provision-api.md#register-thing) request payload\.

**Note**  
If the Lambda function fails, the provisioning request fails with `ACCESS_DENIED` and an error is logged to CloudWatch Logs\.
If the Lambda function doesn't return `"allowProvisioning": "true"` in the response, the provisioning request fails with `ACCESS_DENIED`\.
The Lambda function must finish running and return within 5 seconds, otherwise the provisioning request fails\.

## Pre\-provisioning hook Lambda example<a name="pre-provisioning-example"></a>

------
#### [ Python ]

An example of a pre\-provisioning hook Lambda in Python\.

```
import json

def pre_provisioning_hook(event, context):
    print(event)

    return {
        'allowProvisioning': True,
        'parameterOverrides': {
            'DeviceLocation': 'Seattle'
        }
    }
```

------
#### [ Java ]

An example of a pre\-provisioning hook Lambda in Java\.

Handler class:

```
package example;

import java.util.Map;
import java.util.HashMap;
import com.amazonaws.services.lambda.runtime.Context;
import com.amazonaws.services.lambda.runtime.RequestHandler;

public class PreProvisioningHook implements RequestHandler<PreProvisioningHookRequest, PreProvisioningHookResponse> {

    public PreProvisioningHookResponse handleRequest(PreProvisioningHookRequest object, Context context) {
        Map<String, String> parameterOverrides = new HashMap<String, String>();
        parameterOverrides.put("DeviceLocation", "Seattle");

        PreProvisioningHookResponse response = PreProvisioningHookResponse.builder()
                .allowProvisioning(true)
                .parameterOverrides(parameterOverrides)
                .build();

        return response;
    }

}
```

Request class:

```
package example;

import java.util.Map;
import lombok.Builder;
import lombok.Data;
import lombok.AllArgsConstructor;
import lombok.NoArgsConstructor;

@Data
@Builder
@AllArgsConstructor
@NoArgsConstructor
public class PreProvisioningHookRequest {
    private String claimCertificateId;
    private String certificateId;
    private String certificatePem;
    private String templateArn;
    private String clientId;
    private Map<String, String> parameters;
}
```

Response class:

```
package example;

import java.util.Map;
import lombok.Builder;
import lombok.Data;
import lombok.AllArgsConstructor;
import lombok.NoArgsConstructor;


@Data
@Builder
@AllArgsConstructor
@NoArgsConstructor
public class PreProvisioningHookResponse {
    private boolean allowProvisioning;
    private Map<String, String> parameterOverrides;
}
```

------