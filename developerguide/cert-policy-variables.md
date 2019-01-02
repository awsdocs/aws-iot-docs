# X\.509 Certificate Policy Variables<a name="cert-policy-variables"></a>

X\.509 certificate policy variables allow you to write AWS IoT policies that grant permissions based on X\.509 certificate attributes\. The following sections describe how you can use these certificate policy variables\.

## Issuer Attributes<a name="issuer-attributes"></a>

The following AWS IoT policy variables allow you to allow or deny permissions based on certificate attributes set by the certificate issuer\.

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

## Subject Attributes<a name="subject-attributes"></a>

The following AWS IoT policy variables allow you to grant or deny permissions based on certificate subject attributes set by the certificate issuer\.

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

X\.509 certificates allow these attributes to contain one or more values\. By default, the policy variables for each multi\-value attribute return the first value\. For example, the `Certificate.Subject.Country` attribute might contain a list of country names\. When evaluated in a policy, `iot:Certificate.Subject.Country` is replaced by the first country name\. You can request a specific attribute value using a one\-based index\. For example, `iot:Certificate.Subject.Country.1` is replaced by the second country name in the `Certificate.Subject.Country` attribute\. If you specify an attribute value that does not exist \(for example, if you ask for a third value when there are only two values assigned to the attribute\), no substitution is made and authorization fails\. You can use the `.List` suffix on the policy variable name to specify all values of the attribute\. The following example policy allows any client to connect to AWS IoT, but restricts publishing rights to those clients with certificates whose `Certificate.Subject.Organization` attribute is set to `"Example Corp"` or `"AnyCompany"`\. This is done through the use of a `"Condition"` attribute that specifies a condition for the preceding action\. The condition in this case is that the `Certificate.Subject.Organization` attribute of the certificate must include one of the listed values\.

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
                "*"
            ]
        },
        {
            "Effect":"Allow",
            "Action":[
                "iot:Publish"
            ],
            "Resource":[
                "*"
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

## Issuer Alternate Name Attributes<a name="issuer-alternate-name-attributes"></a>

The following AWS IoT policy variables allow you to grant or deny permissions based on issuer alternate name attributes set by the certificate issuer\.

+ `iot:Certificate.Issuer.AlternativeName.RFC822Name`

+ `iot:Certificate.Issuer.AlternativeName.DNSName`

+ `iot:Certificate.Issuer.AlternativeName.DirectoryName`

+ `iot:Certificate.Issuer.AlternativeName.UniformResourceIdentifier`

+ `iot:Certificate.Issuer.AlternativeName.IPAddress`

## Subject Alternate Name Attributes<a name="subject-alternate-name-attributes"></a>

The following AWS IoT policy variables allow you to grant or deny permissions based on subject alternate name attributes set by the certificate issuer\.

+ `iot:Certificate.Subject.AlternativeName.RFC822Name`

+ `iot:Certificate.Subject.AlternativeName.DNSName`

+ `iot:Certificate.Subject.AlternativeName.DirectoryName`

+ `iot:Certificate.Subject.AlternativeName.UniformResourceIdentifier`

+ `iot:Certificate.Subject.AlternativeName.IPAddress`

## Other Attributes<a name="other-attributes"></a>

You can use `iot:Certificate.SerialNumber` to allow or deny access to AWS IoT resources based on the serial number of a certificate\. The `iot:Certificate.AvailableKeys` policy variable contains the name of all certificate policy variables that contain values\.

## X\.509 Certificate Policy Variable Limitations<a name="policy-limits"></a>

The following limitations apply to X\.509 certificate policy variables:

Wildcards  
If wildcard characters are present in certificate attributes, the policy variable is not replaced by the certificate attribute value, leaving the `${policy-variable}` text in the policy document\. This might cause authorization failure\. 

Array fields  
Certificate attributes that contain arrays are limited to five items\. Additional items are ignored\.

String length  
All string values are limited to 1024 characters\. If a certificate attribute contains a string longer than 1024 characters, the policy variable is not replaced by the certificate attribute value, leaving the `${policy-variable}` in the policy document\. This might cause authorization failure\.