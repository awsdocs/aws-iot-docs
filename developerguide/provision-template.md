# Provisioning Templates<a name="provision-template"></a>

A provisioning template is a JSON document that uses parameters to describe the resources your device must use to interact with AWS IoT\. A template contains two sections: `Parameters` and `Resources`\.

## Parameters Section<a name="parameters-section"></a>

The `Parameters` section declares the parameters used in the `Resources` section\. Each parameter declares a name, a type, and an optional default value\. The default value is used when the dictionary passed in with the template does not contain a value for the parameter\. The `Parameters` section of a template document looks like the following:

```
"Parameters" : {
     "ThingName" : {
       "Type" : "String",
     },
     "SerialNumber" : {
       "Type" : "String",
     },
     "Location" : {
       "Type" : "String",
       "Default" : "WA"
     },
     "CSR" : {
       "Type" : "String",    
     }
  }
```

This template snippet declares four parameters: `ThingName`, `SerialNumber`, `Location`, and `CSR`\. All of these parameters are of type `String`\. The `Location` parameter declares a default value of `"WA"`\.

## Resources Section<a name="resources-section"></a>

The `Resources` section of the template declares the resources required for your device to communicate with AWS IoT: a thing, a certificate, and one or more policies\. Each resource specifies a logical name, a type, and a set of properties\.

A logical name allows you to refer to a resource elsewhere in the template\.

The type specifies the kind of resource you are declaring\. Valid types are:

+ `AWS::IoT::Thing`

+ `AWS::IoT::Certificate`

+ `AWS::IoT::Policy`

The properties you specify depend on the type of resource you are declaring\.

### Thing Resources<a name="thing-resources"></a>

Thing resources are declared using the following properties:

+ `ThingName`: String\.

+ `AttributePayload`: Optional\. A list of name/value pairs\.

+ `ThingTypeName`: Optional\. String for an associated thing type for the thing\.

+ `ThingGroups`: Optional\. A list of groups to which the thing belongs\.

### Certificate Resources<a name="certificate-resources"></a>

Certificates can be specified in one of the following ways:

+ A certificate signing request \(CSR\)\.

+ A certificate ID of an existing device certificate\.

+ A device certificate created with a CA certificate registered with AWS IoT\. If you have more than one CA certificate registered with the same subject field, you must also pass in the CA certificate used to sign the device certificate\.

**Note**  
When you declare a certificate in a template, use only one of these methods\. For example, if you use a CSR, you cannot also specify a certificate ID or a device certificate\.

For more information, see [AWS IoT and Certificates](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/x509-certs.html)\. 

Certificate resources are declared using the following properties:

+ `CertificateSigningRequest`: String\.

+ `CertificateID`: String\.

+ `CertificatePem`: String\.

+ `CACertificatePem`: String\.

+ `Status`: Optional\. String that can be one of: ACTIVE, INACTIVE, PENDING\_ACTIVATION\. Defaults to ACTIVE\.

Examples:

+ Certificate specified with a CSR:

  ```
  "certificate" : {
        "Type" : "AWS::IoT::Certificate",
        "Properties" : {
          "CertificateSigningRequest": {"Ref" : "CSR"},
          "Status" : "ACTIVE"      
        }
      }
  ```

+ Certificate specified with an existing certificate ID:

  ```
  "certificate" : {
        "Type" : "AWS::IoT::Certificate",
        "Properties" : {
          "CertificateId": {"Ref" : "CertificateId"}
        }
      }
  ```

+ Certificate specified with an existing certificate \.pem and CA certificate \.pem:

  ```
  "certificate" : {
    "Type" : "AWS::IoT::Certificate",
    "Properties" : {
      "CACertificatePem": {"Ref" : "CACertificatePem"},
      "CertificatePem": {"Ref" : "CertificatePem"}
    }
  }
  ```

### Policy Resources<a name="policy-resources"></a>

Policy resources are declared using one of the following properties:

+ `PolicyName`: Optional\. String\. Defaults to a hash of the policy document\. If you are using an existing IoT policy, enter the name of the policy for the `PolicyName` property, and do not include the `PolicyDocument` property\.

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
"Resources" : {
    "thing" : {
      "Type" : "AWS::IoT::Thing",
      "Properties" : {
        "ThingName" : {"Ref" : "ThingName"},
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
      }, 
      "OverrideSettings" : {
        "Status" : "DO_NOTHING"
      }
    },
 
    "policy" : {
      "Type" : "AWS::IoT::Policy",
      "Properties" : {
        "PolicyDocument" : "{ \"Version\": \"2012-10-17\", \"Statement\": [{ \"Effect\": \"Allow\", \"Action\":[\"iot:Publish\"], \"Resource\": [\"arn:aws:iot:us-east-1:123456789012:topic/foo/bar\"] }] }"
      }
    }
  }
```

The thing is declared with:

+ The logical name `"thing"`\.

+ The type `AWS::IoT::Thing`\.

+  A set of thing properties\.

  The thing properties include the thing name, a set of attributes, an optional thing type name, and an optional list of thing groups to which the thing belongs\.

Parameters are referenced by `{"Ref": "<parameter-name>"}`\. When the template is evaluated, the parameters are replaced with the parameter's value from the dictionary passed in with the template\.

The certificate is declared with:

+ The logical name `"certificate"`\.

+ The type `AWS::IoT::Certificate`\.

+ A set of properties\.

  The properties include the CSR for the certificate, and setting the status to `ACTIVE`\. The CSR text is passed as a parameter in the dictionary passed with the template\.

The policy is declared with:

+ The logical name `"policy"`\.

+ The type `AWS::IoT::Policy`\.

+ Either the name of an existing policy or a policy document\. 

## Template Example<a name="template-example"></a>

The following JSON file is an example of a complete provisioning template that specifies the certificate with a CSR:

\(The `PolicyDocument` field value must be a JSON object specified as an escaped string\.\)

```
{
  "Parameters" : {
     "ThingName" : {
       "Type" : "String"
     },
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
        "ThingName" : {"Ref" : "ThingName"},
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
     "ThingName" : {
       "Type" : "String"
     },
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
        "ThingName" : {"Ref" : "ThingName"},
        "AttributePayload" : { "version" : "v1", "serialNumber" :  {"Ref" : "SerialNumber"}}, 
        "ThingTypeName" :  "lightBulb-versionA",
        "ThingGroups" : ["v1-lightbulbs", {"Ref" : "Location"}]
      }
    },
 
    "certificate" : {
      "Type" : "AWS::IoT::Certificate",
      "Properties" : {
        "CertificateId": {"Ref" : "CertificateId"}
      }, 
      "OverrideSettings" : {
        "Status" : "DO_NOTHING"
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