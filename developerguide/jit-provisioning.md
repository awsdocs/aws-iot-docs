# Just\-in\-time provisioning<a name="jit-provisioning"></a>

You can use just\-in\-time provisioning \(JITP\) to provision your devices when they first attempt to connect to AWS IoT\. To provision the device, you must enable automatic registration and associate a provisioning template with the CA certificate used to sign the device certificate\. Provisioning successes and errors are logged as [Device provisioning metrics](metrics_dimensions.md#provisioning-metrics) in Amazon CloudWatch\.

**Topics**
+ [JITP overview](#jit-provisioning-overview)
+ [Register CA using provisioning template](#jit-provisioning-registerCA-template)
+ [Register CA using provisioning template name](#jit-provisioning-registerCA-templateName)

## JITP overview<a name="jit-provisioning-overview"></a>

When a device attempts to connect to AWS IoT by using a certificate signed by a registered CA certificate, AWS IoT loads the template from the CA certificate and uses it to call [RegisterThing](fleet-provision-api.md#register-thing)\. The JITP workflow first registers a certificate with a status value of `PENDING_ACTIVATION`\. When the device provisioning flow is complete, the status of the certificate is changed to `ACTIVE`\.

AWS IoT defines the following parameters that you can declare and reference in provisioning templates:
+ `AWS::IoT::Certificate::Country`
+ `AWS::IoT::Certificate::Organization`
+ `AWS::IoT::Certificate::OrganizationalUnit`
+ `AWS::IoT::Certificate::DistinguishedNameQualifier`
+ `AWS::IoT::Certificate::StateName`
+ `AWS::IoT::Certificate::CommonName`
+ `AWS::IoT::Certificate::SerialNumber`
+ `AWS::IoT::Certificate::Id`

The values for these provisioning template parameters are limited to what JITP can extract from the subject field of the certificate of the device being provisioned\. The certificate must contain values for all of the parameters in the template body\. The `AWS::IoT::Certificate::Id` parameter refers to an internally generated ID, not an ID that is contained in the certificate\. You can get the value of this ID using the `principal()` function inside an AWS IoT rule\. 

**Note**  
You can provision devices using AWS IoT Core just\-in\-time provisioning \(JITP\) feature without having to send the entire trust chain on a device's first connection to AWS IoT Core\. Presenting the CA certificate is optional, but the device is required to send the [Server Name Indication \(SNI\)](https://datatracker.ietf.org/doc/html/rfc3546#section-3.1) extension when it connects to AWS IoT Core\.

### Example template body<a name="jit-provisioning-example-templatebody"></a>

The following JSON file is an example template body of a complete JITP template\. 

```
{
   "Parameters":{
      "AWS::IoT::Certificate::CommonName":{
         "Type":"String"
      },
      "AWS::IoT::Certificate::SerialNumber":{
         "Type":"String"
      },
      "AWS::IoT::Certificate::Country":{
         "Type":"String"
      },
      "AWS::IoT::Certificate::Id":{
         "Type":"String"
      }
   },
   "Resources":{
      "thing":{
         "Type":"AWS::IoT::Thing",
         "Properties":{
            "ThingName":{
               "Ref":"AWS::IoT::Certificate::CommonName"
            },
            "AttributePayload":{
               "version":"v1",
               "serialNumber":{
                  "Ref":"AWS::IoT::Certificate::SerialNumber"
               }
            },
            "ThingTypeName":"lightBulb-versionA",
            "ThingGroups":[
               "v1-lightbulbs",
               {
                  "Ref":"AWS::IoT::Certificate::Country"
               }
            ]
         },
         "OverrideSettings":{
            "AttributePayload":"MERGE",
            "ThingTypeName":"REPLACE",
            "ThingGroups":"DO_NOTHING"
         }
      },
      "certificate":{
         "Type":"AWS::IoT::Certificate",
         "Properties":{
            "CertificateId":{
               "Ref":"AWS::IoT::Certificate::Id"
            },
            "Status":"ACTIVE"
         },
         "OverrideSettings":{
            "Status":"DO_NOTHING"
         }
      },
      "policy":{
         "Type":"AWS::IoT::Policy",
         "Properties":{
            "PolicyDocument":"{ \"Version\": \"2012-10-17\", \"Statement\": [{ \"Effect\": \"Allow\", \"Action\":[\"iot:Publish\"], \"Resource\": [\"arn:aws:iot:us-east-1:123456789012:topic/foo/bar\"] }] }"
         }
      }
   }
}
```

This sample template declares values for the `AWS::IoT::Certificate::CommonName`, `AWS::IoT::Certificate::SerialNumber`, `AWS::IoT::Certificate::Country`, and `AWS::IoT::Certificate::Id` provisioning parameters that are extracted from the certificate and used in the `Resources` section\. The JITP workflow then uses this template to perform the following actions:
+ Register a certificate and set its status to PENDING\_ACTIVE\.
+ Create one thing resource\.
+ Create one policy resource\.
+ Attach the policy to the certificate\.
+ Attach the certificate to the thing\.
+ Update the certificate status to ACTIVE\.

Note that the device provisioning fails if the certificate doesn't have all of the properties mentioned in the `Parameters` section of the `templateBody`\. For example, if `AWS::IoT::Certificate::Country` is included in the template, but the certificate doesn't have a `Country` property, the device provisioning fails\.

You can also use CloudTrail to troubleshoot issues with your JITP template\. For information about the metrics that are logged in Amazon CloudWatch, see [Device provisioning metrics](metrics_dimensions.md#provisioning-metrics)\. For more information about provisioning templates, see [Provisioning templates](provision-template.md)\.

**Note**  
During the provisioning process, just\-in\-time provisioning \(JITP\) calls other AWS IoT control plane API operations\. These calls might exceed the [AWS IoT Throttling Quotas](https://docs.aws.amazon.com/general/latest/gr/iot-core.html#throttling-limits) set for your account and result in throttled calls\. Contact [AWS Customer Support](https://console.aws.amazon.com/support/home) to raise your throttling quotas if necessary\.

## Register CA using provisioning template<a name="jit-provisioning-registerCA-template"></a>

To register a CA by using a complete provisioning template, follow these steps: 

1. Save your provisioning template and the role ARN information like the following example as a JSON file:

   ```
   { 
        "templateBody" : "{\r\n    \"Parameters\" : {\r\n        \"AWS::IoT::Certificate::CommonName\": {\r\n            \"Type\": \"String\"\r\n        },\r\n        \"AWS::IoT::Certificate::SerialNumber\": {\r\n            \"Type\": \"String\"\r\n        },\r\n        \"AWS::IoT::Certificate::Country\": {\r\n            \"Type\": \"String\"\r\n        },\r\n        \"AWS::IoT::Certificate::Id\": {\r\n            \"Type\": \"String\"\r\n        }\r\n    },\r\n    \"Resources\": {\r\n        \"thing\": {\r\n            \"Type\": \"AWS::IoT::Thing\",\r\n            \"Properties\": {\r\n                \"ThingName\": {\r\n                    \"Ref\": \"AWS::IoT::Certificate::CommonName\"\r\n                },\r\n                \"AttributePayload\": {\r\n                    \"version\": \"v1\",\r\n                    \"serialNumber\": {\r\n                        \"Ref\": \"AWS::IoT::Certificate::SerialNumber\"\r\n                    }\r\n                },\r\n                \"ThingTypeName\": \"lightBulb-versionA\",\r\n                \"ThingGroups\": [\r\n                    \"v1-lightbulbs\",\r\n                    {\r\n                        \"Ref\": \"AWS::IoT::Certificate::Country\"\r\n                    }\r\n                ]\r\n            },\r\n            \"OverrideSettings\": {\r\n                \"AttributePayload\": \"MERGE\",\r\n                \"ThingTypeName\": \"REPLACE\",\r\n                \"ThingGroups\": \"DO_NOTHING\"\r\n            }\r\n        },\r\n        \"certificate\": {\r\n            \"Type\": \"AWS::IoT::Certificate\",\r\n            \"Properties\": {\r\n                \"CertificateId\": {\r\n                    \"Ref\": \"AWS::IoT::Certificate::Id\"\r\n                },\r\n                \"Status\": \"ACTIVE\"\r\n            },\r\n            \"OverrideSettings\": {\r\n                \"Status\": \"DO_NOTHING\"\r\n            }\r\n        },\r\n        \"policy\": {\r\n            \"Type\": \"AWS::IoT::Policy\",\r\n            \"Properties\": {\r\n                \"PolicyDocument\": \"{ \\\"Version\\\": \\\"2012-10-17\\\", \\\"Statement\\\": [{ \\\"Effect\\\": \\\"Allow\\\", \\\"Action\\\":[\\\"iot:Publish\\\"], \\\"Resource\\\": [\\\"arn:aws:iot:us-east-1:123456789012:topic\/foo\/bar\\\"] }] }\"\r\n            }\r\n        }\r\n    }\r\n}",
        "roleArn" : "arn:aws:iam::123456789012:role/JITPRole"
   }
   ```

   In this example, the value of the `templateBody` field must be a JSON object specified as an escaped string and can use only the values in the [preceding list](#jit-provisioning-overview)\. You can use a variety of tools to create the required JSON output, such as `json.dumps` \(Python\) or `JSON.stringify` \(Node\)\. The value of the `roleARN` field must be the ARN of a role that has the `AWSIoTThingsRegistration` attached to it\. Also, your template can use an existing `PolicyName` instead of the inline `PolicyDocument` in the example\. 

1. Register a CA certificate with the [RegisterCACertificate](https://docs.aws.amazon.com/iot/latest/apireference/API_RegisterCACertificate.html) API operation or the [https://docs.aws.amazon.com/cli/latest/reference/iot/register-ca-certificate.html](https://docs.aws.amazon.com/cli/latest/reference/iot/register-ca-certificate.html) CLI command\. You will specify the directory of the provisioning template and role ARN information that you saved in the previous step:

   The following shows an example of how to register a CA certificate in `DEFAULT` mode using the AWS CLI:

   ```
   aws iot register-ca-certificate --ca-certificate file://your-ca-cert --verification-cert file://your-verification-cert 
                   --set-as-active --allow-auto-registration --registration-config file://your-template
   ```

   The following shows an example of how to register a CA certificate in `SNI_ONLY` mode using the AWS CLI:

   ```
   aws iot register-ca-certificate --ca-certificate file://your-ca-cert --certificate-mode SNI_ONLY
                    --set-as-active --allow-auto-registration --registration-config file://your-template
   ```

   For more information, see [Register your CA Certificates](https://docs.aws.amazon.com/iot/latest/developerguide/register-CA-cert.html)\.

1.  \(Optional\) Update the settings for a CA certificate by using the [UpdateCACertificate](https://docs.aws.amazon.com/iot/latest/apireference/API_UpdateCACertificate.html) API operation or the [https://docs.aws.amazon.com/cli/latest/reference/iot/update-ca-certificate.html](https://docs.aws.amazon.com/cli/latest/reference/iot/update-ca-certificate.html) CLI command\. 

   The following shows an example of how to update a CA certificate using the AWS CLI:

   ```
   aws iot update-ca-certificate --certificate-id caCertificateId
                   --new-auto-registration-status ENABLE --registration-config file://your-template
   ```

## Register CA using provisioning template name<a name="jit-provisioning-registerCA-templateName"></a>

To register a CA by using a provisioning template name, follow these steps:

1. Save your provisioning template body as a JSON file\. You can find an example template body in [example template body](#jit-provisioning-example-templatebody)\.

1. To create a provisioning template, use the [CreateProvisioningTemplate](https://docs.aws.amazon.com/iot/latest/apireference/API_CreateProvisioningTemplate.html) API or the [https://docs.aws.amazon.com/cli/latest/reference/iot/create-provisioning-template.html](https://docs.aws.amazon.com/cli/latest/reference/iot/create-provisioning-template.html) CLI command:

   ```
   aws iot create-provisioning-template --template-name your-template-name \
           --template-body file://your-template-body.json --type JITP \
           --provisioning-role-arn arn:aws:iam::123456789012:role/test
   ```
**Note**  
For just\-in\-time provisioning \(JITP\), you must specify template type to be `JITP` when creating the provisioning template\. For more information about the template type, see [CreateProvisioningTemplate](https://docs.aws.amazon.com/iot/latest/apireference/API_CreateProvisioningTemplate.html) in the *AWS API Reference*\.

1. To register CA with template name, use the [RegisterCACertificate](https://docs.aws.amazon.com/iot/latest/apireference/API_RegisterCACertificate.html) API or the [https://docs.aws.amazon.com/cli/latest/reference/iot/register-ca-certificate.html](https://docs.aws.amazon.com/cli/latest/reference/iot/register-ca-certificate.html) CLI command:

   ```
   aws iot register-ca-certificate --ca-certificate file://your-ca-cert --verification-cert file://your-verification-cert \
           --set-as-active --allow-auto-registration --registration-config templateName=your-template-name
   ```