# Just\-in\-time provisioning<a name="jit-provisioning"></a>

You can have your devices provisioned when they first attempt to connect to AWS IoT\. Just\-in\-time provisioning \(JITP\) settings are made on CA certificates\. To provision the device, you must enable automatic registration and associate a provisioning template with the CA certificate used to sign the device certificate\. Provisioning successes and errors are logged as [Device provisioning metrics](metrics_dimensions.md#provisioning-metrics) in Amazon CloudWatch\.

You can make these settings when you register a CA certificate with the [RegisterCACertificate](https://docs.aws.amazon.com/iot/latest/apireference/API_RegisterCACertificate.html) API or the `register-ca-certificate` CLI command:

```
aws iot register-ca-certificate --ca-certificate file://your-ca-cert --verification-cert 
                file://your-verification-cert --set-as-active --allow-auto-registration --registration-config file://your-template
```

For more information, see [Registering a CA Certificate](device-certs-your-own.html#register-CA-cert)\.

You can also use the [UpdateCACertificate](https://docs.aws.amazon.com/iot/latest/apireference/API_UpdateCACertificate.html) API or the `update-ca-certificate` CLI command to update the settings for a CA certificate:

```
aws iot update-ca-certificate --certificate-id caCertificateId --new-auto-registration-status ENABLE --registration-config file://your-template
```

**Note**  
JITP calls other AWS IoT control plane APIs during the provisioning process\. These calls might exceed the [ AWS IoT Throttling Quotas](https://docs.aws.amazon.com/general/latest/gr/iot-core.html#throttling-limits) set for your account and result in throttled calls\. Contact [AWS Customer Support](https://console.aws.amazon.com/support/home) to raise your throttling quotas, if necessary\.

When a device attempts to connect to AWS IoT by using a certificate signed by a registered CA certificate, AWS IoT loads the template from the CA certificate and uses it to call [RegisterThing](fleet-provision-api.md#register-thing)\. The JITP workflow first registers a certificate with a status value of PENDING\_ACTIVATION\. When the device provisioning flow is complete, the status of the certificate is changed to ACTIVE\.

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

The following JSON file is an example of a complete JITP template\. The value of the `templateBody` field must be a JSON object specified as an escaped string and can use only the values in the preceding list\. You can use a variety of tools to create the required JSON output, such as `json.dumps` \(Python\) or `JSON.stringify` \(Node\)\. The value of the `roleARN` field must be the ARN of a role that has the `AWSIoTThingsRegistration` attached to it\. Also, your template can use an existing `PolicyName` instead of the inline `PolicyDocument` in the example\. \(The first example adds line breaks for readability, but you can copy and paste the template that appears directly below it\.\)

```
        { 
            "templateBody" : "{
                \r\n    \"Parameters\" : {\r\n        
                            \"AWS::IoT::Certificate::CommonName\": {\r\n    \"Type\": \"String\"\r\n        },\r\n        
                            \"AWS::IoT::Certificate::SerialNumber\": {\r\n  \"Type\": \"String\"\r\n        },\r\n        
                            \"AWS::IoT::Certificate::Country\": {\r\n   \"Type\": \"String\"\r\n        },\r\n        
                            \"AWS::IoT::Certificate::Id\": {\r\n    \"Type\": \"String\"\r\n        }\r\n    },\r\n    
                        \"Resources\": {\r\n        
                            \"thing\": {\r\n            
                                \"Type\": \"AWS::IoT::Thing\",\r\n            
                                \"Properties\": {\r\n                
                                    \"ThingName\": {\r\n    \"Ref\": \"AWS::IoT::Certificate::CommonName\"\r\n  },\r\n                
                                    \"AttributePayload\": {\r\n                    
                                        \"version\": \"v1\",\r\n                    
                                        \"serialNumber\": {\r\n                        
                                            \"Ref\": \"AWS::IoT::Certificate::SerialNumber\"\r\n    }\r\n   },\r\n                
                                    \"ThingTypeName\": \"lightBulb-versionA\",\r\n                
                                    \"ThingGroups\": [\r\n                    
                                        \"v1-lightbulbs\",\r\n  {\r\n                        
                                            \"Ref\": \"AWS::IoT::Certificate::Country\"\r\n }\r\n   ]\r\n   },\r\n            
                            \"OverrideSettings\": {\r\n                
                                \"AttributePayload\": \"MERGE\",\r\n                
                                \"ThingTypeName\": \"REPLACE\",\r\n                
                                \"ThingGroups\": \"DO_NOTHING\"\r\n }\r\n   },\r\n        
                        \"certificate\": {\r\n            
                            \"Type\": \"AWS::IoT::Certificate\",\r\n            
                            \"Properties\": {\r\n                
                                \"CertificateId\": {\r\n    \"Ref\": \"AWS::IoT::Certificate::Id\"\r\n  },\r\n                
                                \"Status\": \"ACTIVE\"\r\n  },\r\n            
                            \"OverrideSettings\": {\r\n                
                                \"Status\": \"DO_NOTHING\"\r\n  }\r\n   },\r\n        
                        \"policy\": {\r\n            
                            \"Type\": \"AWS::IoT::Policy\",\r\n            
                            \"Properties\": {\r\n                
                                \"PolicyDocument\": \"{ 
                                    \\\"Version\\\": \\\"2012-10-17\\\", 
                                    \\\"Statement\\\": [{ 
                                    \\\"Effect\\\": \\\"Allow\\\", 
                                    \\\"Action\\\":[\\\"iot:Publish\\\"], 
                                    \\\"Resource\\\": [\\\"arn:aws:iot:us-east-1:123456789012:topic\/sample\/topic\\\"] }] }\"\r\n   }\r\n   }\r\n    
                              }\r\n}",
            "roleArn" : "arn:aws:iam::123456789012:role/Provisioning-JITP"
        }
```

Here is a version you can copy and paste:

```
    { 
      "templateBody" : "{\r\n    \"Parameters\" : {\r\n        \"AWS::IoT::Certificate::CommonName\": {\r\n            \"Type\": \"String\"\r\n        },\r\n        \"AWS::IoT::Certificate::SerialNumber\": {\r\n            \"Type\": \"String\"\r\n        },\r\n        \"AWS::IoT::Certificate::Country\": {\r\n            \"Type\": \"String\"\r\n        },\r\n        \"AWS::IoT::Certificate::Id\": {\r\n            \"Type\": \"String\"\r\n        }\r\n    },\r\n    \"Resources\": {\r\n        \"thing\": {\r\n            \"Type\": \"AWS::IoT::Thing\",\r\n            \"Properties\": {\r\n                \"ThingName\": {\r\n                    \"Ref\": \"AWS::IoT::Certificate::CommonName\"\r\n                },\r\n                \"AttributePayload\": {\r\n                    \"version\": \"v1\",\r\n                    \"serialNumber\": {\r\n                        \"Ref\": \"AWS::IoT::Certificate::SerialNumber\"\r\n                    }\r\n                },\r\n                \"ThingTypeName\": \"lightBulb-versionA\",\r\n                \"ThingGroups\": [\r\n                    \"v1-lightbulbs\",\r\n                    {\r\n                        \"Ref\": \"AWS::IoT::Certificate::Country\"\r\n                    }\r\n                ]\r\n            },\r\n            \"OverrideSettings\": {\r\n                \"AttributePayload\": \"MERGE\",\r\n                \"ThingTypeName\": \"REPLACE\",\r\n                \"ThingGroups\": \"DO_NOTHING\"\r\n            }\r\n        },\r\n        \"certificate\": {\r\n            \"Type\": \"AWS::IoT::Certificate\",\r\n            \"Properties\": {\r\n                \"CertificateId\": {\r\n                    \"Ref\": \"AWS::IoT::Certificate::Id\"\r\n                },\r\n                \"Status\": \"ACTIVE\"\r\n            },\r\n            \"OverrideSettings\": {\r\n                \"Status\": \"DO_NOTHING\"\r\n            }\r\n        },\r\n        \"policy\": {\r\n            \"Type\": \"AWS::IoT::Policy\",\r\n            \"Properties\": {\r\n                \"PolicyDocument\": \"{ \\\"Version\\\": \\\"2012-10-17\\\", \\\"Statement\\\": [{ \\\"Effect\\\": \\\"Allow\\\", \\\"Action\\\":[\\\"iot:Publish\\\"], \\\"Resource\\\": [\\\"arn:aws:iot:us-east-1:123456789012:topic\/foo\/bar\\\"] }] }\"\r\n            }\r\n        }\r\n    }\r\n}",
      "roleArn" : "arn:aws:iam::123456789012:role/JITPRole"
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

You can also use CloudTrail to troubleshoot issues with your JITP template\. For information about the metrics that are logged in Amazon CloudWatch, see [Device provisioning metrics](metrics_dimensions.md#provisioning-metrics)\.