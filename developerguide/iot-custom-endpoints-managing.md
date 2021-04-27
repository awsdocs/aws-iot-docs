# Managing domain configurations<a name="iot-custom-endpoints-managing"></a>

You can manage the lifecycles of existing configurations by using the following APIs\.
+ [ListDomainConfigurations](https://docs.aws.amazon.com/iot/latest/apireference/API_ListDomainConfigurations.html)
+ [DescribeDomainConfiguration](https://docs.aws.amazon.com/iot/latest/apireference/API_DescribeDomainConfiguration.html)
+ [UpdateDomainConfiguration](https://docs.aws.amazon.com/iot/latest/apireference/API_UpdateDomainConfiguration.html)
+ [DeleteDomainConfiguration](https://docs.aws.amazon.com/iot/latest/apireference/API_DeleteDomainConfiguration.html)

## Viewing domain configurations<a name="iot-custom-endpoints-managing-view"></a>

Use the [ListDomainConfigurations](https://docs.aws.amazon.com/iot/latest/apireference/API_ListDomainConfigurations.html) API to return a paginated list of all domain configurations in your account\. You can see the details of a particular domain configuration using the [DescribeDomainConfiguration](https://docs.aws.amazon.com/iot/latest/apireference/API_DescribeDomainConfiguration.html) API\. This API takes a single `domainConfigurationName` parameter and returns the details of the specified configuration\.

## Updating domain configurations<a name="iot-custom-endpoints-managing-update"></a>

To update the status or the custom authorizer of your domain configuration, use the [UpdateDomainConfiguration](https://docs.aws.amazon.com/iot/latest/apireference/API_UpdateDomainConfiguration.html) API\. You can set the status to `ENABLED` or `DISABLED`\. If you disable the domain configuration, devices connected to that domain receive an authentication error\.

**Note**  
Currently you can't update the server certificate in your domain configuration\. To change the certificate of a domain configuration, you must delete and recreate it\.

## Deleting domain configurations<a name="iot-custom-endpoints-managing-delete"></a>

Before you delete a domain configuration, use the [UpdateDomainConfiguration](https://docs.aws.amazon.com/iot/latest/apireference/API_UpdateDomainConfiguration.html) API to set the status to `DISABLED`\. This helps you avoid accidentally deleting the endpoint\. After you disable the domain configuration, delete it by using the [DeleteDomainConfiguration](https://docs.aws.amazon.com/iot/latest/apireference/API_DeleteDomainConfiguration.html) API\.

**Note**  
You must place AWS\-managed domains in `DISABLED` status for 7 days before you can delete them\. You can place custom domains in `DISABLED` status and then immediately delete them\.

After you delete a domain configuration, AWS IoT Core no longer serves the server certificate associated with that custom domain\.

## Rotating certificates in custom domains<a name="iot-custom-endpoints-managing-certificates"></a>

You may need to periodically replace your server certificate with an updated certificate\. The rate at which you do this depends on the validity period of your certificate\. If you generated your server certificate by using AWS Certificate Manager \(ACM\), you can set the certificate to renew automatically\. When ACM renews your certificate, AWS IoT Core automatically picks up the new certificate\. You don't have to perform any additional action\. If you imported your server certificate from a different source, you can rotate it by r﻿eimporting it to ACM\. For information about reimporting certificates, see [Reimport a certificate](https://docs.aws.amazon.com/acm/latest/userguide/import-reimport.html)\.

**Note**  
AWS IoT Core only picks up certificate updates under the following conditions\.  
The new certificate has the same ARN as the old one\.
The new certificate has the same signing algorithm, common name, or subject alternative name as the old one\.