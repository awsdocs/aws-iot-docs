# Device provisioning MQTT API<a name="fleet-provision-api"></a><a name="provision-mqtt-api"></a>

The Fleet Provisioning service supports these MQTT APIs:
+ `CreateCertificateFromCsr`
+ `CreateKeysAndCertificate`
+ `RegisterThing`

This API supports response buffers in Concise Binary Object Representation \(CBOR\) format and JavaScript Object Notation \(JSON\), depending on the *payload\-format* of the topic\. For the sake of clarity, however, the response and request examples in this section are shown in JSON format\.


| *payload\-format* | Response format data type | 
| --- | --- | 
| cbor | Concise Binary Object Representation \(CBOR\) | 
| json | JavaScript Object Notation \(JSON\) | 

**Important**  
Before publishing a request message topic, subscribe to the response topics to receive the response\. The messages used by this API use MQTT's publish/subscribe protocol to provide a request and response interaction\.   
If you do not subscribe to the response topics *before* you publish a request, you might not receive the results of that request\.

## CreateCertificateFromCsr<a name="create-cert-csr"></a>

Creates a certificate from a certificate signing request \(CSR\)\. AWS IoT provides client certificates that are signed by the Amazon Root certificate authority \(CA\)\. The new certificate has a `PENDING_ACTIVATION` status\. When you call `RegisterThing` to provision a thing with this certificate, the certificate status changes to `ACTIVE` or `INACTIVE` as described in the template\.

### CreateCertificateFromCsr request<a name="create-cert-csr-request"></a>

Publish a message with the `$aws/certificates/create-from-csr/payload-format` topic\.

`payload-format`  
The message payload format as `cbor` or `json`\.

#### CreateCertificateFromCsr request payload<a name="create-cert-csr-request-payload"></a>

```
{
    "certificateSigningRequest": "string"
}
```

`certificateSigningRequest`  
The CSR, in PEM format\.

### CreateCertificateFromCsr response<a name="create-cert-csr-response"></a>

Subscribe to `$aws/certificates/create-from-csr/payload-format/accepted`\.

`payload-format`  
The message payload format as `cbor` or `json`\.

#### CreateCertificateFromCsr response payload<a name="create-cert-csr-response-payload"></a>

```
{
    "certificateOwnershipToken": "string",
    "certificateId": "string",
    "certificatePem": "string"
}
```

`certificateOwnershipToken`  
The token to prove ownership of the certificate during provisioning\. 

`certificateId`  
The ID of the certificate\. Certificate management operations only take a certificateId\. 

`certificatePem`  
The certificate data, in PEM format\.

### CreateCertificateFromCsr error<a name="create-cert-csr-error"></a>

To receive error responses, subscribe to `$aws/certificates/create-from-csr/payload-format/rejected`\.

`payload-format`  
The message payload format as `cbor` or `json`\.

#### CreateCertificateFromCsr error payload<a name="create-cert-csr-error-payload"></a>

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

## CreateKeysAndCertificate<a name="create-keys-cert"></a>

Creates new keys and a certificate\. AWS IoT provides client certificates that are signed by the Amazon Root certificate authority \(CA\)\. The new certificate has a `PENDING_ACTIVATION` status\. When you call `RegisterThing` to provision a thing with this certificate, the certificate status changes to `ACTIVE` or `INACTIVE` as described in the template\.

### CreateKeysAndCertificate request<a name="create-keys-cert-request"></a>

Publish a message on `$aws/certificates/create/payload-format` with an empty message payload\.

`payload-format`  
The message payload format as `cbor` or `json`\.

### CreateKeysAndCertificate response<a name="create-keys-cert-response"></a>

Subscribe to `$aws/certificates/create/payload-format/accepted`\.

`payload-format`  
The message payload format as `cbor` or `json`\.

#### CreateKeysAndCertificate response<a name="create-keys-cert-response-payload"></a>

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

### CreateKeysAndCertificate error<a name="create-keys-cert-error"></a>

To receive error responses, subscribe to `$aws/certificates/create/payload-format/rejected`\.

`payload-format`  
The message payload format as `cbor` or `json`\.

#### CreateKeysAndCertificate error payload<a name="create-keys-cert-error-payload"></a>

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

## RegisterThing<a name="register-thing"></a>

Provisions a thing using a pre\-defined template\.

### RegisterThing request<a name="register-thing-request"></a>

Publish a message on `$aws/provisioning-templates/templateName/provision/payload-format`\.

`payload-format`  
The message payload format as `cbor` or `json`\.

`templateName`  
The provisioning template name\.

#### RegisterThing request payload<a name="register-thing-request-payload"></a>

```
{
    "certificateOwnershipToken": "string",
    "parameters": {
        "string": "string",
        ...
    }
}
```

`certificateOwnershipToken`  
The token to prove ownership of the certificate\. The token is generated by AWS IoT when you create a certificate over MQTT\.

`parameters`  
Optional\. Key\-value pairs from the device that are used by the [pre\-provisioning hooks](pre-provisioning-hook.md) to evaluate the registration request\.

### RegisterThing response<a name="register-thing-response"></a>

Subscribe to `$aws/provisioning-templates/templateName/provision/payload-format/accepted`\.

`payload-format`  
The message payload format as `cbor` or `json`\.

`templateName`  
The provisioning template name\.

#### RegisterThing response payload<a name="register-thing-response-payload"></a>

```
{
    "deviceConfiguration": {
        "string": "string",
        ...
    },
    "thingName": "string"
}
```

`deviceConfiguration`  
The device configuration defined in the template\.

`thingName`  
The name of the IoT thing created during provisioning\.

### RegisterThing error response<a name="register-thing-error"></a>

To receive error responses, subscribe to `$aws/provisioning-templates/templateName/provision/payload-format/rejected`\.

`payload-format`  
The message payload format as `cbor` or `json`\.

`templateName`  
The provisioning template name\.

#### RegisterThing error response payload<a name="register-thing-error-payload"></a>

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