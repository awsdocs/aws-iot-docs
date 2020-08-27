# X\.509 Certificate AWS IoT Core policy variables<a name="cert-policy-variables"></a>

X\.509 certificate policy variables allow you to write AWS IoT Core policies that grant permissions based on X\.509 certificate attributes\. The following sections describe how you can use these certificate policy variables\.

## CertificateId<a name="cert-policy-variables-certid"></a>

In the [RegisterCertificate](https://docs.aws.amazon.com/iot/latest/apireference/API_RegisterCertificate.html) API, the `certificateId` appears in the response body\. To get information about your certificate, you can use the `certificateId` in [DescribeCertificate](https://docs.aws.amazon.com/iot/latest/apireference/API_DescribeCertificate.html)\.

## Issuer attributes<a name="issuer-attributes"></a>

The following AWS IoT Core policy variables allow you to allow or deny permissions based on certificate attributes set by the certificate issuer\.
+ `iot:Certificate.Issuer.DistinguishedNameQualifier`
+ `iot:Certificate.Issuer.Country`
+ `iot:Certificate.Issuer.Organization`
+ `iot:Certificate.Issuer.OrganizationalUnit`
+ `iot:Certificate.Issuer.State`
+ `iot:Certificate.Issuer.CommonName`
+ `iot:Certificate.Issuer.SerialNumber`
+ `iot:Certificate.Issuer.Title`
+ `iot:Certificate.Issuer.Surname`
+ `iot:Certificate.Issuer.GivenName`
+ `iot:Certificate.Issuer.Initials`
+ `iot:Certificate.Issuer.Pseudonym`
+ `iot:Certificate.Issuer.GenerationQualifier` 

## Subject attributes<a name="subject-attributes"></a>

The following AWS IoT Core policy variables allow you to grant or deny permissions based on certificate subject attributes set by the certificate issuer\.
+ `iot:Certificate.Subject.DistinguishedNameQualifier`
+ `iot:Certificate.Subject.Country`
+ `iot:Certificate.Subject.Organization`
+ `iot:Certificate.Subject.OrganizationalUnit`
+ `iot:Certificate.Subject.State`
+ `iot:Certificate.Subject.CommonName`
+ `iot:Certificate.Subject.SerialNumber`
+ `iot:Certificate.Subject.Title`
+ `iot:Certificate.Subject.Surname`
+ `iot:Certificate.Subject.GivenName`
+ `iot:Certificate.Subject.Initials`
+ `iot:Certificate.Subject.Pseudonym`
+ `iot:Certificate.Subject.GenerationQualifier` 

X\.509 certificates allow these attributes to contain one or more values\. By default, the policy variables for each multi\-value attribute return the first value\. For example, the `Certificate.Subject.Country` attribute might contain a list of country names, but when evaluated in a policy, `iot:Certificate.Subject.Country` is replaced by the first country name\. You can request a specific attribute value other than the first value by using a one\-based index\. For example, `iot:Certificate.Subject.Country.1` is replaced by the second country name in the `Certificate.Subject.Country` attribute\. If you specify an index value that does not exist \(for example, if you ask for a third value when there are only two values assigned to the attribute\), no substitution is made and authorization fails\. You can use the `.List` suffix on the policy variable name to specify all values of the attribute\.

------
#### [ Registered devices \(2\) ]

For devices registered as things in the AWS IoT Core registry, the following policy allows clients with a thing name registered in the AWS IoT Core registry to connect, but restricts the right to publish to a thing name specific topic to those clients with certificates whose `Certificate.Subject.Organization` attribute is set to `"Example Corp"` or `"AnyCompany"`\. This restriction is accomplished by using a `"Condition"` field that specifies a condition that must be met to allow the preceding action\. In this case the condition is that the `Certificate.Subject.Organization` attribute associated with the certificate must include one of the values listed:

```
{
    "Version":"2012-10-17",
    "Statement":[
      {
        "Effect":"Allow",
        "Action":[
          "iot:Connect"
        ],
        "Resource":[
          "arn:aws:iot:us-east-1:123456789012:client/${iot:Connection.Thing.ThingName}"
        ]
      },
      {
        "Effect":"Allow",
        "Action":[
          "iot:Publish"
        ],
        "Resource":[
          "arn:aws:iot:us-east-1:123456789012:topic/my/topic/${iot:Connection.Thing.ThingName}"
        ],
        "Condition":{
          "ForAllValues:StringEquals":{
            "iot:Certificate.Subject.Organization.List":[
              "Example Corp",
              "AnyCompany"
            ]
          }
        }
      }
    ]
}
```

------
#### [ Unregistered devices \(2\) ]

For devices not registered as things in the AWS IoT Core registry, the following policy grants permission to connect to AWS IoT Core with client IDs `client1`, `client2`, and `client3`, but restricts the right to publish to a client\-id specific topic to those clients with certificates whose `Certificate.Subject.Organization` attribute is set to `"Example Corp"` or `"AnyCompany"`\. This restriction is accomplished by using a `"Condition"` field that specifies a condition that must be met to allow the preceding action\. In this case the condition is that the `Certificate.Subject.Organization` attribute associated with the certificate must include one of the values listed:

```
{
    "Version":"2012-10-17",
    "Statement":[
      {
        "Effect":"Allow",
        "Action":[
          "iot:Connect"
        ],
        "Resource":[
          "arn:aws:iot:us-east-1:123456789012:client/client1",
          "arn:aws:iot:us-east-1:123456789012:client/client2",
          "arn:aws:iot:us-east-1:123456789012:client/client3"
        ]
      },
      {
        "Effect":"Allow",
        "Action":[
          "iot:Publish"
        ],
        "Resource":[
          "arn:aws:iot:us-east-1:123456789012:topic/my/topic/${iot:ClientId}"
        ],
        "Condition":{
          "ForAllValues:StringEquals":{
            "iot:Certificate.Subject.Organization.List":[
              "Example Corp",
              "AnyCompany"
            ]
          }
        }
      }
    ]
}
```

------

## Issuer alternate name attributes<a name="issuer-alternate-name-attributes"></a>

The following AWS IoT Core policy variables allow you to grant or deny permissions based on issuer alternate name attributes set by the certificate issuer\.
+ `iot:Certificate.Issuer.AlternativeName.RFC822Name`
+ `iot:Certificate.Issuer.AlternativeName.DNSName`
+ `iot:Certificate.Issuer.AlternativeName.DirectoryName`
+ `iot:Certificate.Issuer.AlternativeName.UniformResourceIdentifier`
+ `iot:Certificate.Issuer.AlternativeName.IPAddress`

## Subject alternate name attributes<a name="subject-alternate-name-attributes"></a>

The following AWS IoT Core policy variables allow you to grant or deny permissions based on subject alternate name attributes set by the certificate issuer\.
+ `iot:Certificate.Subject.AlternativeName.RFC822Name`
+ `iot:Certificate.Subject.AlternativeName.DNSName`
+ `iot:Certificate.Subject.AlternativeName.DirectoryName`
+ `iot:Certificate.Subject.AlternativeName.UniformResourceIdentifier`
+ `iot:Certificate.Subject.AlternativeName.IPAddress`

## Other attributes<a name="other-attributes"></a>

You can use `iot:Certificate.SerialNumber` to allow or deny access to AWS IoT Core resources based on the serial number of a certificate\. The `iot:Certificate.AvailableKeys` policy variable contains the name of all certificate policy variables that contain values\.

## X\.509 Certificate policy variable limitations<a name="policy-limits"></a>

The following limitations apply to X\.509 certificate policy variables:

Wildcards  
If wildcard characters are present in certificate attributes, the policy variable is not replaced by the certificate attribute value, leaving the `${policy-variable}` text in the policy document\. This might cause authorization failure\. The following wildcard characters can be used: `*`, `$`, `+`, `?`, and `#`\.

Array fields  
Certificate attributes that contain arrays are limited to five items\. Additional items are ignored\.

String length  
All string values are limited to 1024 characters\. If a certificate attribute contains a string longer than 1024 characters, the policy variable is not replaced by the certificate attribute value, leaving the `${policy-variable}` in the policy document\. This might cause authorization failure\.

Special Characters  
Any special character, such as `,`, `"`, `\`, `+`, `=`, `<`, `>` and `;` must be prefixed with a backslash \(`\`\) when used in a policy variable\. For example, `Amazon Web Services O=Amazon.com Inc. L=Seattle ST=Washington C=US` becomes `Amazon Web Service O=\Amazon.com Inc. L\=Seattle ST\=Washington C\=US`\.