# Creating and configuring custom domains<a name="iot-custom-endpoints-configurable-custom"></a>

Domain configurations let you specify a custom fully qualified domain name \(FQDN\) to connect to AWS IoT\. Custom domains enable you to manage your own server certificates so that you can manage details, such as the root certificate authority \(CA\) used to sign the certificate, the signature algorithm, the certificate chain depth, and the lifecycle of the certificate\.

The workflow to set up a domain configuration with a custom domain consists of the following three stages\.

1. [Registering Server Certificates in AWS Certificate Manager](#iot-custom-endpoints-configurable-custom-register-certificate)

1. [Creating a Domain Configuration](#iot-custom-endpoints-configurable-custom-domain-config)

1. [Creating DNS Records](#iot-custom-endpoints-configurable-custom-dns)

## Registering server certificates in AWS certificate manager<a name="iot-custom-endpoints-configurable-custom-register-certificate"></a>

Before you create a domain configuration with a custom domain, you must register your server certificate chain in [AWS Certificate Manager \(ACM\)](https://docs.aws.amazon.com/acm/latest/userguide/acm-overview.html)\. You can use three types of server certificates\.
+ [ACM Generated Public Certificates](#iot-custom-endpoints-configurable-custom-register-certificate-acm)
+ [External Certificates Signed by a Public CA](#iot-custom-endpoints-configurable-custom-register-certificate-pubext)
+ [External Certificates Signed by a Private CA](#iot-custom-endpoints-configurable-custom-register-certificate-privext)

**Note**  
AWS IoT considers a certificate to be signed by a public CA if it's included in [Mozilla's trusted ca\-bundle](https://hg.mozilla.org/mozilla-central/raw-file/tip/security/nss/lib/ckfw/builtins/certdata.txt?raw=1)\.

**Certificate requirements**  
See [Prerequisites for Importing Certificates](https://docs.aws.amazon.com/acm/latest/userguide/import-certificate-prerequisites.html) for the requirements for importing certificates into ACM\. In addition to these requirements, AWS IoT Core adds the following requirements\.
+ The leaf certificate must include contain the **Extended Key Usage** x509 v3 extension with a value of **serverAuth** \(TLS Web Server Authentication\)\. If you request the certificate from ACM, this extension is automatically added\.
+ The maximum certificate chain depth is 5 certificates\.
+ The maximum certificate chain size is 16KB\.

**Using one certificate for multiple domains**  
If you plan to use one certificate to cover multiple subdomains, use a wildcard domain in the common name \(CN\) or Subject Alternative Names \(SAN\) field\. For example, use **\*\.iot\.example\.com** to cover dev\.iot\.example\.com, qa\.iot\.example\.com, and prod\.iot\.example\.com\. Each FQDN requires its own domain configuration, but more than one domain configuration can use the same wildcard value\. Either the CN or the SAN must cover the FQDN that you want to use as a custom domain\. This coverage can be an exact match or a wildcard match\. 

The following sections describe how to get each type of certificate\. Every certificate resource requires an Amazon Resource Name \(ARN\) registered with ACM that you use when you create your domain configuration\.

### ACM\-generated public certificates<a name="iot-custom-endpoints-configurable-custom-register-certificate-acm"></a>

You can generate a public certificate for your custom domain by using the [RequestCertificate](https://docs.aws.amazon.com/acm/latest/APIReference/API_RequestCertificate.html) API\. When you generate a certificate in this way, ACM validates your ownership of the custom domain\. For more information, see [Request a Public Certificate](https://docs.aws.amazon.com/acm/latest/userguide/gs-acm-request-public.html) in the *AWS Certificate Manager User Guide*\.

### External certificates signed by a public CA<a name="iot-custom-endpoints-configurable-custom-register-certificate-pubext"></a>

If you already have a server certificate that is signed by a public CA \(a CA that is included in Mozilla's trusted ca\-bundle\), you can import the certificate chain directly into ACM by using the [ImportCertificate](https://docs.aws.amazon.com/acm/latest/APIReference/API_ImportCertificate.html) API\. To learn more about this task and the prerequisites and certificate format requirements, see [Importing Certificates](https://docs.aws.amazon.com/acm/latest/userguide/import-certificate.html)\.

### External certificates signed by a private CA<a name="iot-custom-endpoints-configurable-custom-register-certificate-privext"></a>

If you already have a server certificate that is signed by a private CA or self\-signed, you can use the certificate to create your domain configuration, but you also must create an extra public certificate in ACM to validate ownership of your domain\. To do this, register your server certificate chain in ACM using the [ImportCertificate](https://docs.aws.amazon.com/acm/latest/APIReference/API_ImportCertificate.html) API\. To learn more about this task and the prerequisites and certificate format requirements, see [Importing Certificates](https://docs.aws.amazon.com/acm/latest/userguide/import-certificate.html)\. 

After you import your certificate to ACM, generate a public certificate for your custom domain by using the [RequestCertificate](https://docs.aws.amazon.com/acm/latest/APIReference/API_RequestCertificate.html) API\. When you generate a certificate in this way, ACM validates your ownership of the custom domain\. For more information, see [Request a Public Certificate](https://docs.aws.amazon.com/acm/latest/userguide/gs-acm-request-public.html)\. When you create your domain configuration, use this public certificate as your validation certificate\.

## Creating a domain configuration<a name="iot-custom-endpoints-configurable-custom-domain-config"></a>

You create a configurable endpoint on a custom domain by using the [CreateDomainConfiguration](https://docs.aws.amazon.com/iot/latest/apireference/API_CreateDomainConfiguration.html) API\. A domain configuration for a custom domain consists of the following:
+ `domainConfigurationName` – A user\-defined name that identifies the domain configuration\.
**Note**  
Domain configuration names starting with `IoT:` are reserved for default endpoints and can't be used\. Also, this value must be unique to your Region\.
+ `domainName` – The FQDN that your devices use to connect to AWS IoT\.
**Note**  
AWS IoT leverages the server name indication \(SNI\) TLS extension to apply domain configurations\. Devices must use this extension when connecting and pass a server name that is identical to the domain name that is specified in the domain configuration\.
+ `serverCertificateArns` – The ARN of the server certificate chain that you registered with ACM\. AWS IoT Core currently supports only one server certificate\.
+ `validationCertificateArn` – The ARN of the public certificate that you generated in ACM to validate ownership of your custom domain\. This argument isn't required if you use a publicly signed or ACM\-generated server certificate\.
+ `defaultAuthorizerName` \(optional\) – The name of the custom authorizer to use on the endpoint\.
+ `allowAuthorizerOverride` – A Boolean value that specifies whether devices can override the default authorizer by specifying a different authorizer in the HTTP header of the request\. This value is required if a value for `defaultAuthorizerName` is specified\.
+ `serviceType` – Possible values are `DATA`, `CREDENTIAL_PROVIDER`, and `JOB`\.
**Note**  
AWS IoT currently supports only the `DATA` service type\.

The following AWS CLI command creates a domain configuration for **iot\.example\.com**\.

```
aws iot create-domain-configuration --domain-configuration-name "myDomainConfigurationName" --service-type "DATA" 
--domain-name "iot.example.com" --server-certificate-arns serverCertARN --validation-certificate-arn validationCertArn
```

**Note**  
After you create your domain configuration, it might take up to 60 minutes until AWS IoT serves your custom server certificates\.

## Creating DNS records<a name="iot-custom-endpoints-configurable-custom-dns"></a>

After you register your server certificate chain and create your domain configuration, create a DNS record so that your custom domain points to an AWS IoT domain\. This record must point to an AWS IoT endpoint of type `iot:Data-ATS`\. You can get your endpoint by using the [DescribeEndpoint](https://docs.aws.amazon.com/iot/latest/apireference/API_DescribeEndpoint.html) API\. 

The following AWS CLI command shows how to get your endpoint\.

```
aws iot describe-endpoint --endpoint-type iot:Data-ATS
```

After you get your `iot:Data-ATS` endpoint, create a `CNAME` record from your custom domain to this AWS IoT endpoint\. If you create multiple custom domains in the same account, alias them to this same `iot:Data-ATS` endpoint\.

## Troubleshooting<a name="iot-custom-endpoints-configurable-troubleshoot"></a>

If you have trouble connecting devices to a custom domain, make sure that AWS IoT Core has accepted and applied your server certificate\. You can verify that AWS IoT Core has accepted your certificate by using either the AWS IoT Core console or the AWS CLI\.

To use the AWS IoT Core console, navigate to the **Settings** page and select the domain configuration name\. In the **Server certificate details** section, check the status and status details\. If the certificate is invalid, replace it in ACM with a certificate that meets the [certificate requirements](#certificate-requirements) listed in the previous section\. If the certificate has the same ARN, AWS IoT Core will be pick it up and apply it automatically\.

To check the certificate status by using the AWS CLI, call the [DescribeDomainConfiguration](https://docs.aws.amazon.com/iot/latest/apireference/API_DescribeDomainConfiguration.html) API and specify your domain configuration name\.

**Note**  
If your certificate is invalid, AWS IoT Core will continue to serve the last valid certificate\.

You can check which certificate is being served on your endpoint by using the following openssl command\.

`openssl s_client -connect custom-domain-name:8883 -showcerts -servername custom-domain-name`