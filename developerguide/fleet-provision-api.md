# Fleet provisioning APIs<a name="fleet-provision-api"></a>

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
+ API used by devices during the provisioning process\. This means using a provisioning claim certificate embedded in a device, or one passed to it by a trusted user\. 

## Device provisioning MQTT API<a name="provision-mqtt-api"></a>

The Fleet Provisioning service supports two MQTT APIs: `CreateKeysAndCertificate`, `CreateCertificateFromCSR` and `RegisterThing`\.

### CreateCertificateFromCsr<a name="create-cert-csr"></a>

To create a certificate from a certificate signing request \(CSR\), publish a message on `$aws/certificates/create-from-csr/cbor` or `$aws/certificates/create-from-csr/json`\.

Request payload:

```
{
    "certificateSigningRequest": "string"
}
```

`certificateSigningRequest`  
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

`statusCode`  
The status code\.

`errorCode`  
The error code\.

`errorMessage`  
The error message\.

For more information, see [CreateCertificateFromCsr](https://docs.aws.amazon.com/iot/latest/apireference/API_CreateCertificateFromCsr) in the AWS IoT API Reference\.

### CreateKeysAndCertificate<a name="create-keys-cert"></a>

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

`certificateOwnershipToken`  
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

### RegisterThing<a name="register-thing"></a>

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

`templateName`  
The provisioning template name\.

`certificateOwnershipToken`  
The token to prove ownership of the certificate\. The token is generated by AWS IoT when you create a certificate over MQTT\.

`parameters`  
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