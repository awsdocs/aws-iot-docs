# Provisioning devices that don't have device certificates using fleet provisioning<a name="provision-wo-cert"></a>

When you use AWS IoT fleet provisioning, AWS IoT can generate and securely deliver device certificates and private keys to your devices when they connect to AWS IoT for the first time\. Device certificates are registered for day\-to\-day use\. There are two ways to use fleet provisioning:
+ By claim\.
+ By trusted user\.

## Provisioning by claim<a name="claim-based"></a>

Devices can be manufactured with a provisioning claim certificate and private key \(which are special purpose credentials\) embedded in them\. If these certificates are registered with AWS IoT, the service can exchange them for unique device certificates that the device can use for regular operations\. This process includes the following steps:

1. Use the `CreateProvisioningTemplate` API to create a provisioning template\. This API returns a template ARN\. For more information, see [Fleet provisioning APIs](#fleet-provision-api)\. You can also create a fleet provisioning template in the AWS IoT console\. 

   1. From the navigation pane, choose **Onboard**,then choose **Fleet provisioning templates**\.

   1. Choose **Create** and follow the prompts\.

1. Create certificates and associated private keys to be used as provisioning claims\.

1. Register these certificates with AWS IoT and associate an IoT policy that restricts the use of the certificates\. The following example IoT policy restricts the use of the certificate associated with this policy to provisioning devices\.

   ```
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Effect": "Allow",
               "Action": ["iot:Connect"],
               "Resource": "*"
           },
           {
               "Effect": "Allow",
               "Action": ["iot:Publish","iot:Receive"],
               "Resource": [
                   "arn:aws:iot:us-east-1:123456789012:topic/$aws/certificates/create/*",
                   "arn:aws:iot:us-east-1:123456789012:topic/$aws/provisioning-templates/templateName/provision/*"
               ]
           },
           {
               "Effect": "Allow",
               "Action": "iot:Subscribe",
               "Resource": [
                   "arn:aws:iot:us-east-1:123456789012:topicfilter/$aws/certificates/create/*",
                   "arn:aws:iot:us-east-1:123456789012:topicfilter/$aws/provisioning-templates/templateName/provision/*"
               ]
           }
       ]
   }
   ```

1. Give the AWS IoT service permission to create or update IoT resources such as things and certificates in your account when provisioning devices\. Do this by attaching the `AWSIoTThingsRegistration` managed policy to an IAM role \(called the provisioning role\) that trusts the AWS IoT service principal\.

1. The device is manufactured with the provisioning claim securely embedded in it\.

1. The device uses the provisioning claim to authenticate with AWS IoT using the AWS IoT Device SDK\.

1. You can choose to receive a unique certificate and private key or you can use a CSR to generate a certificate that keeps its private key secure\. The device will use them for future authentication with AWS IoT\. At the same time, the Fleet Provisioning service creates cloud resources such as IoT things, thing groups, and attributes, as defined in the provisioning template\.

**Important**  
Provisioning claim private keys should be secured at all times, including on the device\. We recommend that you use AWS IoT CloudWatch metrics and logs to monitor for indications of misuse\. If you detect misuse, disable the provisioning claim certificate so it cannot be used for device provisioning\.

## Provisioning by trusted user<a name="trusted-user"></a>

In many cases, a device connects to AWS IoT for the first time when an end user or installation technician uses a mobile app to configure the device in its deployed location\. This process includes the following steps:

1. Use the `CreateProvisioningTemplate` API to create a provisioning template\. This API returns a template ARN\.

1. Create an IAM role that is used by a trusted user to initiate the provisioning process\. The provisioning template allows only that user to provision a device\. For example:

   ```
   {
       "Effect": "Allow",
       "Action": [
           "iot:CreateProvisioningClaim",
       ],
       "Resource": [
           "arn:aws:iot:us-west-2:123456789012:provisioningtemplate/templateName"
       ]
   }
   ```

1. Give the AWS IoT service permission to create or update IoT resources, such as things and certificates in your account when provisioning devices\. You do this by attaching the `AWSIoTThingsRegistration` managed policy to an IAM role \(called the *provisioning role*\) that trusts the AWS IoT service principal\.

1. The trusted user signs in to your provisioning mobile app or web service\.

1. The mobile app or web application uses the IAM role and the `CreateProvisioningClaim` API to obtain a temporary provisioning claim from AWS IoT\.

1. The mobile app or web application supplies the provisioning claim to the device along with configuration information, such as Wi\-Fi credentials\.

1. The device uses the temporary provisioning claim to connect to AWS IoT using the [AWS IoT device and mobile SDKs ](iot-sdks.md)\.

1. You can choose to receive a unique certificate and private key or you can use a CSR to generate a certificate that keeps its private key secure\. The device will use them for future authentication with AWS IoT\. At the same time, the Fleet Provisioning service creates cloud resources such as IoT things, thing groups, and attributes, as defined in the provisioning template\.

## Pre\-provisioning hooks<a name="pre-provisioning-hook"></a>

When using AWS IoT fleet provisioning, you can set up a Lambda function to validate device parameters before allowing the device to be provisioned\. This Lambda function must exist in your account before you provision a device because it's called every time a device sends a request through [RegisterThing](https://docs.aws.amazon.com/iot/latest/apireference/API_RegisterThing.html)\. Your Lambda function must be able to receive an input and return an output, otherwise your device won't be provisioned\. The provisioning proceeds only if the Lambda function returns the parameter `"allowProvisioning": "SUCCEED"`\.

**Important**  
AWS recommends using pre\-provisioning hooks when creating provisioning templates to allow more control of which and how many devices onboard to your account\.

The following is an example payload that AWS IoT sends to the Lambda function\.

```
{
 "claimId" : "..................",
 "certificatePem" : "........................",
 "templateArn" : "arn:aws:iot:us-east-1:1234567890:provisioningtemplate/MyTemplate",
 "mqttClientId" : "221a6d10-9c7f-42f1-9153-e52e6fc869c1",
 "incomingParameters" : {
   "customKey" : "customValue",
   // more parameters incoming from device ....
 }
}
```

`"incomingParameters"` have the content present in "parameters" from the [RegisterThing](https://docs.aws.amazon.com/iot/latest/apireference/API_RegisterThing.html) request payload\.

The Lambda function must return a response that indicates whether or not it has authorized the provisioning request\. The following is an example of a successful response\.

```
{
 "allowProvisioning": "SUCCEED",
 "parameterOverrides" : {
   "customKeysAndValues": "sentToDevice",
   // more parameters that will be returned to device ....
 }
}
```

`"parameterOverrides"` will be added to `"deviceConfiguration"` parameter in [RegisterThing](https://docs.aws.amazon.com/iot/latest/apireference/API_RegisterThing.html) response payload\.

**Note**  
If the Lambda function fails or doesn't return the `"allowProvisioning"` parameter in the response, the provisioning request will fail and the error will be returned in the response\.
The Lambda function must finish executing and return within 5 seconds, otherwise the provisioning request fails\.

### How to use pre\-provisioning hooks on the AWS CLI<a name="hooks-cli-instruc"></a>

The following procedure creates a provisioning template with pre\-provisioning hooks\. The Lambda function used here is an example that can be modified\. 

**To create and apply a pre\-provisioning hook to a provisioning template**

1. Create a Lambda function that has an a defined input and output\. Lambda functions are highly customizable the `allowProvisioning` and `parameterOverrides` are required for creating pre\-provisioning hooks\. For more information about creating Lambda functions, see [Using AWS Lambda with the AWS Command Line Interface](https://docs.aws.amazon.com/lambda/latest/dg/gettingstarted-awscli.html)\.

   The following is an example of a Lambda function output:

   ```
   {
     "allowProvisioning": "SUCCEED",
     "parameterOverrides": {
       "incomingKey0": "incomingValue0",
       "incomingKey1": "incomingValue1"
     }
   }
   ```

1. AWS IoT uses resource\-based policies to call Lambda, so you will need to give AWS IoT permission to call your Lambda function\.

   The following is an example using [add\-permission](https://docs.aws.amazon.com/cli/latest/reference/lambda/add-permission.html)give IoT permission to your Lambda\.

   ```
   aws lambda add-permission /
       --function-name myLambdaFunction /
       --statement-id iot-permission /
       --action lambda:InvokeFunction /
       --principal iot.amazon.aws
   ```

1. Add a pre\-provisioning hook to a template using either the [create\-provisioning\-template](https://docs.aws.amazon.com/cli/latest/reference/iot/create-provisioning-template.html) or [update\-provisioning\-template](https://docs.aws.amazon.com/cli/latest/reference/iot/update-provisioning-template.html) command\.

   The following CLI example uses the [create\-provisioning\-template](https://docs.aws.amazon.com/cli/latest/reference/iot/create-provisioning-template.html) to create a provisioning template that has pre\-provisioning hooks:

   ```
   aws iot create-provisioning template /
       --template-name myTemplate /
       --provisioning-role-arn arn:aws:iam:us-east-1:1234564789012:role/myRole /
       --template-body file://template.json /
       --pre-provisioning-hook file://hooks.json
   ```

   The output of this command looks like the following:

   ```
   {
       "templateArn": "arn:aws:iot:us-east-1:1234564789012:provisioningtemplate/myTemplate",
       "defaultVersionId": 1,
       "templateName": myTemplate
   }
   ```

   You can also load a parameter from a file instead of typing it all as a command line parameter value to save time\. For more information, see [Loading AWS CLI Parameters from a File](https://docs.aws.amazon.com/cli/latest/userguide/cli-usage-parameters-file.html)\. The following shows the `template` parameter in expanded JSON format:

   ```
   {
       "Parameters" : {
           "DeviceLocation": {
               "Type": "String"
           }
       },
       "Mappings": {
           "LocationTable": {
               "Seattle": {
                   "LocationUrl": "https://example.aws"
               }
           }
       },
       "Resources" : {
           "thing" : {
               "Type" : "AWS::IoT::Thing",
               "Properties" : {
                   "AttributePayload" : {
                       "version" : "v1",
                       "serialNumber" : "serialNumber"
                   },
                   "ThingName" : {"Fn::Join":["",["ThingPrefix_",{"Ref":"SerialNumber"}]]},
                   "ThingTypeName" : {"Fn::Join":["",["ThingTypePrefix_",{"Ref":"SerialNumber"}]]},
                   "ThingGroups" : ["widgets", "WA"],
                   "BillingGroup": "BillingGroup"
               },
               "OverrideSettings" : {
                   "AttributePayload" : "MERGE",
                   "ThingTypeName" : "REPLACE",
                   "ThingGroups" : "DO_NOTHING"
               }
           },
           "certificate" : {
               "Type" : "AWS::IoT::Certificate",
               "Properties" : {
                   "CertificateId": {"Ref": "AWS::IoT::Certificate::Id"},
                   "Status" : "Active"
               }
           },
           "policy" : {
               "Type" : "AWS::IoT::Policy",
               "Properties" : {
                   "PolicyDocument" : {
                       "Version": "2012-10-17",
                       "Statement": [{
                           "Effect": "Allow",
                           "Action":["iot:Publish"],
                           "Resource": ["arn:aws:iot:us-east-1:504350838278:topic/foo/bar"]
                       }]
                   }
               }
           }
       },
       "DeviceConfiguration": {
           "FallbackUrl": "https://www.example.com/test-site",
           "LocationUrl": {
               "Fn::FindInMap": ["LocationTable",{"Ref": "DeviceLocation"}, "LocationUrl"]}
       }
   }
   ```

   The following shows the `pre-provisioning-hook` parameter in expanded JSON format:

   ```
   {
        "targetArn" : "arn:aws:lambda:us-east-1:765219403047:function:pre_provisioning_test",
        "payloadVersion" : "2020-04-30"
   }
   ```

## Fleet provisioning APIs<a name="fleet-provision-api"></a>

There are three categories of API used in fleet provisioning:
+ Control plane API used to create and manage fleet provisioning templates and to configure trusted user policies\.
  + [CreateProvisioningTemplate](https://docs.aws.amazon.com/iot/latest/apireference/API_CreateProvisioningTemplate.html)
  + [ CreateProvisioningTemplateVersion](https://docs.aws.amazon.com/iot/latest/apireference/API_CreateProvisioningTemplateVersion.html)
  + [DeleteProvisioningTemplate](https://docs.aws.amazon.com/iot/latest/apireference/API_DeleteProvisioningTemplate.html)
  + [DeleteProvisioningTemplateVersion](https://docs.aws.amazon.com/iot/latest/apireference/API_DeleteProvisioningTemplateVersion.html)
  + [DescribeProvisioningTemplate](https://docs.aws.amazon.com/iot/latest/apireference/API_DescribeProvisioningTemplate.html)
  + [DescribeProvisioningTemplateVersion](https://docs.aws.amazon.com/iot/latest/apireference/API_DescribeProvisioningTemplateVersion.html)
  + [ListProvisioningTemplates](https://docs.aws.amazon.com/iot/latest/apireference/API_ListProvisioningTemplates.html)
  + [ListProvisioningTemplateVersions](https://docs.aws.amazon.com/iot/latest/apireference/API_ListProvisioningTemplateVersions.html)
  + [UpdateProvisioningTemplate](https://docs.aws.amazon.com/iot/latest/apireference/API_UpdateProvisioningTemplate.html)
+ Control plane API used by a trusted user to generate a temporary onboarding claim\. This claim is passed to the device during Wi\-Fi config or similar method\. There is one API in this category: [CreateProvisioningClaim](https://docs.aws.amazon.com/iot/latest/apireference/API_CreateProvisioningClaim.html)\.
+ API used by devices during the provisioning process\. This means using a provisioning claim embedded in a device, or one passed to it by a trusted user\. 

### Device provisioning MQTT API<a name="provision-mqtt-api"></a>

The Fleet Provisioning service supports two MQTT APIs: `CreateKeysAndCertificate`, `CreateCertificateFromCSR` and `RegisterThing`\.

#### CreateCertificateFromCsr<a name="create-cert-csr"></a>

To create a certificate from a certificate signing request \(CSR\), publish a message on `$aws/certificates/create-from-csr/cbor` or `$aws/certificates/create-from-csr/json`\.

Request payload:

```
{
    "certificateSigningRequest": "string"
}
```

certificateSigningRequest  
The CSR, in PEM format\.

To receive error responses, subscribe to `$aws/certificates/create-from-csr/cbor/rejected` or `$aws/certificates/create-from-csr/json/rejected`\.

Error payload:

```
{
    "statusCode": int,
    "errorCode": "string",
    "errorMessage": "string"
}
```

statusCode  
The status code\.

errorCode  
The error code\.

errorMessage  
The error message\.

For more information, see [CreateCertificateFromCsr](https://docs.aws.amazon.com/iot/latest/apireference/API_CreateCertificateFromCsr) in the AWS IoT API Reference\.

#### CreateKeysAndCertificate<a name="create-keys-cert"></a>

Examples in this document use JSON for readability, but you can also use the Concise Binary Object Representation \(CBOR\) format\. The new certificate will have the `PENDING_ACTIVATION` status\. When you use the `RegisterThing` API to provision a thing, the certificate status changes to `ACTIVE` or `INACTIVE` based on the template definition\.

To create a certificate, publish a message on `$aws/certificates/create/cbor` or `$aws/certificates/create/json`\.

Request payload:

```
{}
```

Response payload:

To receive the response, subscribe to `$aws/certificates/create/cbor/accepted` or `$aws/certificates/create/json/accepted`\. For more information about connecting to the message broker and using AWS IoT reserved topics, see [Lifecycle events](life-cycle-events.md) and [Reserved topics](reserved-topics.md)\.

```
{
            "certificateId": "string",
            "certificatePem": "string",
            "privateKey": "string",
            "certificateOwnershipToken": "string"
        }
```

`certificateId`  
The certificate ID\.

`certificatePem`  
The certificate data, in PEM format\.

`privateKey`  
The private key\.

certificateOwnershipToken  
The token to prove ownership of the certificate during provisioning\.

To receive error responses, subscribe to `$aws/certificates/create/cbor/rejected` or `$aws/certificates/create/json/rejected`\.

Error payload:

```
{
    "statusCode": int,
    "errorCode": "string",
    "errorMessage": "string"
}
```

`statusCode`  
The status code\.

`errorCode`  
The error code\.

`errorMessage`  
The error message\.

For more information, see [CreateKeysAndCertificate](https://docs.aws.amazon.com/iot/latest/apireference/API_CreateKeysAndCertificate) in the AWS IoT API Reference\.

#### RegisterThing<a name="register-thing"></a>

To provision a thing, publish a message on `$aws/provisioning-templates/templateName/provision/cbor` or `$aws/provisioning-templates/templateName/provision/json`\.

Request payload:

```
{
    "certificateOwnershipToken": "string",
    "parameters": {
        "string": "string"
    },
}
```

templateName  
The provisioning template name\.

certificateOwnershipToken  
The token to prove ownership of the certificate\. The token is generated by AWS IoT when you create a certificate over MQTT\.

parameters  
Optional\. Name\-value pairs to send to devices during provisioning\.

To receive the response, subscribe to `$aws/provisioning-templates/templateName/provision/cbor/accepted ` or `$aws/provisioning-templates/templateName/provision/json/accepted`\.

Response payload:

```
{
    "deviceConfiguration": {
        "string": "string"
    },
    "thingName": "string"
}
```

`thingName`  
The name of the IoT thing created during provisioning\.

`deviceConfiguration`  
The device configuration defined in the template\.

To receive error responses, subscribe to `$aws/provisioning-templates/templateName/provision/cbor/rejected` or `$aws/provisioning-templates/templateName/provision/json/rejected`\.

Error payload:

```
{
    "statusCode": int,
    "errorCode": "string",
    "errorMessage": "string"
}
```

`statusCode`  
The status code\.

`errorCode`  
The error code\.

`errorMessage`  
The error message\.

For more information, see [RegisterThing](https://docs.aws.amazon.com/iot/latest/apireference/API_RegisterThing) in the AWS IoT API Reference\.