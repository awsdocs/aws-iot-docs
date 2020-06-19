# Provisioning Devices That Don't Have Device Certificates Using Fleet Provisioning<a name="provision-wo-cert"></a>


****  

|  | 
| --- |
| The fleet provisioning feature is in beta and is subject to change\. We recommend that you use this feature with test devices only\. | 

When you use AWS IoT fleet provisioning, AWS IoT can generate and securely deliver device certificates and private keys to your devices when they connect to AWS IoT for the first time\. Device certificates are registered for day\-to\-day use\. There are two ways to use fleet provisioning:
+ By claim\.
+ By trusted user\.

## Provisioning by Claim<a name="claim-based"></a>

Devices can be manufactured with a provisioning claim certificate and private key \(which are special purpose credentials\) embedded in them\. If these certificates are registered with AWS IoT, the service can exchange them for unique device certificates that the device can use for regular operations\. This process includes the following steps:

1. You use the `CreateProvisioningTemplate` API to create a provisioning template\. This API returns a template ARN\. For more information, see [Fleet Provisioning APIs](#fleet-provision-api)\. You can also create a fleet provisioning template in the AWS IoT console\. 

   1. From the navigation pane, choose **Onboard**, and then choose **Fleet provisioning templates**\.

   1. Choose **Create** and follow the prompts\.

1. You create certificates and associated private keys to be used as provisioning claims\.

1. You register these certificates with AWS IoT and associate an IoT policy that restricts the use of the certificates\. The following example IoT policy restricts the use of the certificate associated with this policy to provisioning devices\.

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

1. You give the AWS IoT service permission to create or update IoT resources such as things and certificates in your account when provisioning devices\. You do this by attaching the `AWSIoTThingsRegistration` managed policy to an IAM role \(called the provisioning role\) that trusts the AWS IoT service principal\.

1. The device is manufactured with the provisioning claim securely embedded in it\.

1. The device uses the provisioning claim to authenticate with AWS IoT using the AWS IoT Device SDK\.

1. The device receives a device certificate and private key that it securely stores and uses for future authentication with AWS IoT\. At the same time, the Fleet Provisioning service creates cloud resources such as IoT things, thing groups, and attributes, as defined in the provisioning template\.

**Important**  
Provisioning claim private keys should be secured at all times, including on the device\. We recommend that you use AWS IoT CloudWatch metrics and logs to monitor for indications of misuse\. If you detect misuse, disable the provisioning claim certificate so it cannot be used for device provisioning\.

## Provisioning by Trusted User<a name="trusted-user"></a>

In many cases, a device connects to AWS IoT for the first time when an end user or installation technician uses a mobile app to configure the device in its deployed location\. This process includes the following steps:

1. You use the `CreateProvisioningTemplate` API to create a provisioning template\. This API returns a template ARN\.

1. You create an IAM role that is used by a trusted user to initiate the provisioning process\. The provisioning template allows only that user to provision a device\. For example:

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

1. You give the AWS IoT service permission to create or update IoT resources, such as things and certificates in your account when provisioning devices\. You do this by attaching the `AWSIoTThingsRegistration` managed policy to an IAM role \(called the *provisioning role*\) that trusts the AWS IoT service principal\.

1. The trusted user signs in to your provisioning mobile app or web service\.

1. The mobile app or web application uses the IAM role and the `CreateProvisioningClaim` API to obtain a temporary provisioning claim from AWS IoT\.

1. The mobile app or web application supplies the provisioning claim to the device along with configuration information, such as Wi\-Fi credentials\.

1. The device uses the temporary provisioning claim to connect to AWS IoT using the [AWS IoT Device and Mobile SDKs ](iot-sdks.md)\.

1. The device receives a unique device certificate and private key that it securely stores and uses for future connections to AWS IoT\. At the same time, the Fleet Provisioning service creates cloud resources such as IoT things, thing groups, and attributes, as defined in the provisioning template\.

## Fleet Provisioning APIs<a name="fleet-provision-api"></a>

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
+ Control plane API are used by a trusted user to generate a temporary onboarding claim which is passed to the device during Wi\-Fi config or similar method\. There is one API in this category: [CreateProvisioningClaim](https://docs.aws.amazon.com/iot/latest/apireference/API_CreateProvisioningClaim.html)\.
+ API used by devices during the provisioning process \(using a provisioning claim embedded in a device or one passed to it by a trusted user\)\. 

### Device Provisioning MQTT API<a name="provision-mqtt-api"></a>

The Fleet Provisioning service supports two MQTT APIs: `CreateKeysAndCertificate` and `RegisterThing`\.

#### CreateKeysAndCertificate<a name="create-keys-cert"></a>

Examples in this document use JSON for readability, but you can also use the Concise Binary Object Representation \(CBOR\) format\. The new certificate will have the `PENDING_ACTIVATION` status\. When you use the `RegisterThing` API to provision a thing, the certificate status changes to `ACTIVE` or `INACTIVE` based on the template definition\.

To create a certificate, publish a message on `$aws/certificates/create/cbor` or `$aws/certificates/create/json`\.

Request payload:

```
{}
```

Response payload:

To receive the response, subscribe to `$aws/certificates/create/cbor/accepted` or `$aws/certificates/create/json/accepted`\. For more information about connecting to the message broker and using AWS IoT reserved topics, see [Lifecycle Events](life-cycle-events.md) and [Reserved Topics](reserved-topics.md)\.

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