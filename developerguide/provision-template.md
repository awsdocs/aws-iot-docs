# Provisioning Templates<a name="provision-template"></a>

A provisioning template is a JSON document that uses parameters to describe the resources your device must use to interact with AWS IoT\. A template contains two sections: `Parameters` and `Resources`\. There are two types of provisioning templates in AWS IoT\. One is used for JITP and bulk registration and the second is used for fleet provisioning\.

## Parameters Section<a name="parameters-section"></a>

The `Parameters` section declares the parameters used in the `Resources` section\. Each parameter declares a name, a type, and an optional default value\. The default value is used when the dictionary passed in with the template does not contain a value for the parameter\. The `Parameters` section of a template document looks like the following:

```
{
    "Parameters" : {
        "SerialNumber" : {
            "Type" : "String"
        },
        "Location" : {
            "Type" : "String",
            "Default" : "WA"
        },
        "CSR" : {
            "Type" : "String"    
        }
    }
}
```

This template snippet declares four parameters: `ThingName`, `SerialNumber`, `Location`, and `CSR`\. All of these parameters are of type `String`\. The `Location` parameter declares a default value of `"WA"`\.

## Resources Section<a name="resources-section"></a>

The `Resources` section of the template declares the resources required for your device to communicate with AWS IoT: a thing, a certificate, and one or more IoT policies\. Each resource specifies a logical name, a type, and a set of properties\.

A logical name allows you to refer to a resource elsewhere in the template\.

The type specifies the kind of resource you are declaring\. Valid types are:
+ `AWS::IoT::Thing`
+ `AWS::IoT::Certificate`
+ `AWS::IoT::Policy`

The properties you specify depend on the type of resource you are declaring\.

### Thing Resources<a name="thing-resources"></a>

Thing resources are declared using the following properties:
+ `ThingName`: String\.
+ `AttributePayload`: Optional\. A list of name\-value pairs\.
+ `ThingTypeName`: Optional\. String for an associated thing type for the thing\.
+ `ThingGroups`: Optional\. A list of groups to which the thing belongs\.

### Certificate Resources<a name="certificate-resources"></a>

You can specify certificates in one of the following ways:
+ A certificate signing request \(CSR\)\.
+ A certificate ID of an existing device certificate\. \(Only certificate IDs can be used with a fleet provisioning template\)
+ A device certificate created with a CA certificate registered with AWS IoT\. If you have more than one CA certificate registered with the same subject field, you must also pass in the CA certificate used to sign the device certificate\.

**Note**  
When you declare a certificate in a template, use only one of these methods\. For example, if you use a CSR, you cannot also specify a certificate ID or a device certificate\. For more information, see [AWS IoT and Certificates](https://docs.aws.amazon.com/iot/latest/developerguide/x509-certs.html)\. 

For more information, see [X\.509 Certificate Overview](authentication.md#x509-certificate-overview)\. 

Certificate resources are declared using the following properties:
+ `CertificateSigningRequest`: String\.
+ `CertificateID`: String\.
+ `CertificatePem`: String\.
+ `CACertificatePem`: String\.
+ `Status`: Optional\. String that can be `ACTIVE` or `INACTIVE`\. Defaults to ACTIVE\.

Examples:
+ Certificate specified with a CSR:

  ```
  {
      "certificate" : {
          "Type" : "AWS::IoT::Certificate",
          "Properties" : {
              "CertificateSigningRequest": {"Ref" : "CSR"},
              "Status" : "ACTIVE"      
          }
      }
  }
  ```
+ Certificate specified with an existing certificate ID:

  ```
  {
      "certificate" : {
          "Type" : "AWS::IoT::Certificate",
          "Properties" : {
              "CertificateId": {"Ref" : "CertificateId"}
          }
      }
  }
  ```
+ Certificate specified with an existing certificate \.pem and CA certificate \.pem:

  ```
  {
      "certificate" : {
          "Type" : "AWS::IoT::Certificate"
          "Properties" : {
              "CACertificatePem": {"Ref" : "CACertificatePem"},
              "CertificatePem": {"Ref" : "CertificatePem"}
          }
      }
  }
  ```

### Policy Resources<a name="policy-resources"></a>

Policy resources are declared using one of the following properties:
+ `PolicyName`: Optional\. String\. Defaults to a hash of the policy document\. If you are using an existing AWS IoT policy, for the `PolicyName` property, enter the name of the policy\. Do not include the `PolicyDocument` property\.
+ `PolicyDocument`: Optional\. A JSON object specified as an escaped string\. If `PolicyDocument` is not provided, the policy must already be created\.

**Note**  
If a `Policy` section is present, `PolicyName` or `PolicyDocument`, but not both, must be specified\.

### Override Settings<a name="override-settings"></a>

If a template specifies a resource that already exists, the `OverrideSettings` section allows you to specify the action to take:

`DO_NOTHING`  
Leave the resource as is\.

`REPLACE`  
Replace the resource with the resource specified in the template\.

`FAIL`  
Cause the request to fail with a `ResourceConflictsException`\.

`MERGE`  
Valid only for the `ThingGroups` and `AttributePayload` properties of a `thing`\. Merge the existing attributes or group memberships of the thing with those specified in the template\.

When you declare a thing resource, you can specify `OverrideSettings` for the following properties:
+ `ATTRIBUTE_PAYLOAD`
+ `THING_TYPE_NAME`
+ `THING_GROUPS`

When you declare a certificate resource, you can specify `OverrideSettings` for the `Status` property\.

`OverrideSettings` are not available for policy resources\.

### Resource Example<a name="resource-example"></a>

The following template snippet declares a thing, a certificate, and a policy:

```
{ 
    "Resources" : {
        "thing" : {
            "Type" : "AWS::IoT::Thing",
            "Properties" : {
                "AttributePayload" : { "version" : "v1", "serialNumber" :  {"Ref" : "SerialNumber"}}, 
                "ThingTypeName" :  "lightBulb-versionA",
                "ThingGroups" : ["v1-lightbulbs", {"Ref" : "Location"}]
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
                "CertificateSigningRequest": {"Ref" : "CSR"},
                "Status" : "ACTIVE"      
            }
        },
        "policy" : {
            "Type" : "AWS::IoT::Policy",
            "Properties" : {
                "PolicyDocument" : "{ \"Version\": \"2012-10-17\", \"Statement\": [{ \"Effect\": \"Allow\", \"Action\":[\"iot:Publish\"], \"Resource\": [\"arn:aws:iot:us-east-1:123456789012:topic/foo/bar\"] }] }"
            }
        }
    }
}
```

The thing is declared with:
+ The logical name `"thing"`\.
+ The type `AWS::IoT::Thing`\.
+  A set of thing properties\.

  The thing properties include the thing name, a set of attributes, an optional thing type name, and an optional list of thing groups to which the thing belongs\.

Parameters are referenced by `{"Ref":"<parameter-name>"}`\. When the template is evaluated, the parameters are replaced with the parameter's value from the dictionary passed in with the template\.

The certificate is declared with:
+ The logical name `"certificate"`\.
+ The type `AWS::IoT::Certificate`\.
+ A set of properties\.

  The properties include the CSR for the certificate, and setting the status to `ACTIVE`\. The CSR text is passed as a parameter in the dictionary passed with the template\.

The policy is declared with:
+ The logical name `"policy"`\.
+ The type `AWS::IoT::Policy`\.
+ Either the name of an existing policy or a policy document\.

## Template Example for JITP and Bulk Registration<a name="bulk-template-example"></a>

The following JSON file is an example of a complete provisioning template that specifies the certificate with a CSR:

\(The `PolicyDocument` field value must be a JSON object specified as an escaped string\.\)

```
{
    "Parameters" : {
        "SerialNumber" : {
            "Type" : "String"
        },
        "Location" : {
            "Type" : "String",
            "Default" : "WA"
        },
        "CSR" : {
            "Type" : "String"    
        }
    },
    "Resources" : {
        "thing" : {
            "Type" : "AWS::IoT::Thing",
            "Properties" : {
                "AttributePayload" : { "version" : "v1", "serialNumber" :  {"Ref" : "SerialNumber"}}, 
                "ThingTypeName" :  "lightBulb-versionA",
                "ThingGroups" : ["v1-lightbulbs", {"Ref" : "Location"}]
            }
        },
        "certificate" : {
            "Type" : "AWS::IoT::Certificate",
            "Properties" : {
                "CertificateSigningRequest": {"Ref" : "CSR"},
                "Status" : "ACTIVE"      
            }
        },
        "policy" : {
            "Type" : "AWS::IoT::Policy",
            "Properties" : {
                "PolicyDocument" : "{ \"Version\": \"2012-10-17\", \"Statement\": [{ \"Effect\": \"Allow\", \"Action\":[\"iot:Publish\"], \"Resource\": [\"arn:aws:iot:us-east-1:123456789012:topic/foo/bar\"] }] }"
            }
        }
    }
}
```

The following JSON file is an example of a complete provisioning template that specifies an existing certificate with a certificate ID:

```
{
    "Parameters" : {
        "SerialNumber" : {
            "Type" : "String"
        },
        "Location" : {
            "Type" : "String",
            "Default" : "WA"
        },
        "CertificateId" : {
            "Type" : "String"    
        }
    },
    "Resources" : {
        "thing" : {
            "Type" : "AWS::IoT::Thing",
            "Properties" : {
                "AttributePayload" : { "version" : "v1", "serialNumber" :  {"Ref" : "SerialNumber"}}, 
                "ThingTypeName" :  "lightBulb-versionA",
                "ThingGroups" : ["v1-lightbulbs", {"Ref" : "Location"}]
            }
        },
        "certificate" : {
            "Type" : "AWS::IoT::Certificate",
            "Properties" : {
                "CertificateId": {"Ref" : "CertificateId"}
            }
        },
        "policy" : {
            "Type" : "AWS::IoT::Policy",
            "Properties" : {
                "PolicyDocument" : "{ \"Version\": \"2012-10-17\", \"Statement\": [{ \"Effect\": \"Allow\", \"Action\":[\"iot:Publish\"], \"Resource\": [\"arn:aws:iot:us-east-1:123456789012:topic/foo/bar\"] }] }"
            }
        }
    }   
}
```

## Fleet Provisioning<a name="fleet-provision-template"></a>

Fleet provisioning templates are used by AWS IoT to set up cloud and device configuration\. These templates use the same parameters and resources as the JITP and bulk registration templates\. For more information, see [Provisioning Templates](#provision-template)\. Fleet provisioning templates can contain a `Mapping` section and a `DeviceConfiguration` section\. You can use intrinsic functions inside a fleet provisioning template to generate device specific configuration\. Fleet provisioning templates are named resources and are identified by ARNs \(for example, `arn:aws:iot:us-west-2:1234568788:provisioningtemplate/<template-name>`\)\.

### Mappings<a name="mappings"></a>

The optional `Mappings` section matches a key to a corresponding set of named values\. For example, if you want to set values based on an AWS Region, you can create a mapping that uses the AWS Region name as a key and contains the values you want to specify for each specific region\. You use the `Fn::FindInMap` intrinsic function to retrieve values in a map\.

You cannot include parameters, pseudo parameters, or call intrinsic functions in the `Mappings` section\.

### Device Configuration<a name="device-config"></a>

The device configuration section contains arbitrary data you want to send to your devices when provisioning\. For example: 

```
{
    "DeviceConfiguration": {
        "DeviceConfiguration": {
            "Foo":"Bar"
        }
    }
}
```

### Intrinsic Functions<a name="intrinsic-functions"></a>

Intrinsic functions are used in any section of the provisioning template except the `Mappings` section\.

`Fn:Join`  
Appends a set of values into a single value, separated by the specified delimiter\. If a delimiter is the empty string, the set of values are concatenated with no delimiter\.

`Fn:Select`  
Returns a single object from a list of objects by index\.  
`Fn::Select` does not check for `null` values or if the index is out of bounds of the array\. Both conditions result in a provisioning error, so you should ensure you chose a valid index value, and that the list contains non\-null values\.

`Fn:FindInMap`  
Returns the value corresponding to keys in a two\-level map that is declared in the `Mappings` section\.

`Fn:Split`  
Splits a string into a list of string values so you can select an element from the list of strings\. You specify a delimiter that determine where the string is split \(for example, a comma\)\. After you split a string, use `Fn::Select` to select an element\.  
For example, if a comma\-delimited string of subnet IDs is imported to your stack template, you can split the string at each comma\. From the list of subnet IDs, use `Fn::Select` to specify a subnet ID for a resource\.

`Fn:Sub`  
Substitutes variables in an input string with values that you specify\. You can use this function to construct commands or outputs that include values that aren't available until you create or update a stack\.

### Fleet Provisioning Template Example<a name="fleet-provisioning-example"></a>

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
                "ThingTypeName" : {"Fn::Join":["",["ThingPrefix_",{"Ref":"SerialNumber"}]]},
                "ThingGroups" : ["v1-lightbulbs", "WA"],
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
                        "Resource": ["arn:aws:iot:us-east-1:123456789012:topic/foo/bar"]
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
}
```