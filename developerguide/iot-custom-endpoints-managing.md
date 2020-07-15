# Managing Domain configurations<a name="iot-custom-endpoints-managing"></a>

****  
This feature is currently in public beta and is available only in the US East \(N\. Virginia\) Region\.

You can manage the lifecycles of existing configurations by using the following APIs\.
+ [ListDomainConfigurations](https://docs.aws.amazon.com/iot/latest/apireference/API_ListDomainConfigurations.html)
+ [DescribeDomainConfiguration](https://docs.aws.amazon.com/iot/latest/apireference/API_DescribeDomainConfiguration.html)
+ [UpdateDomainConfiguration](https://docs.aws.amazon.com/iot/latest/apireference/API_UpdateDomainConfiguration.html)
+ [DeleteDomainConfiguration](https://docs.aws.amazon.com/iot/latest/apireference/API_DeleteDomainConfiguration.html)

## Viewing Domain configurations<a name="iot-custom-endpoints-managing-view"></a>

Use the [ListDomainConfigurations](https://docs.aws.amazon.com/iot/latest/apireference/API_ListDomainConfigurations.html) API to return a paginated list of all domain configurations in your account\. You can see the details of a particular domain configuration using the [DescribeDomainConfiguration](https://docs.aws.amazon.com/iot/latest/apireference/API_DescribeDomainConfiguration.html) API\. This API takes a single `domainConfigurationName` parameter and returns the details of the specified configuration\.

## Updating Domain configurations<a name="iot-custom-endpoints-managing-update"></a>

To update the status or the custom authorizer of your domain configuration, use the [UpdateDomainConfiguration](https://docs.aws.amazon.com/iot/latest/apireference/API_UpdateDomainConfiguration.html) API\. You can set the status to `ENABLED` or `DISABLED`\. If you disable the domain configuration, devices connected to that domain receive an authentication error\.

**Note**  
Currently you can't update the server certificate in your domain configuration\. To change the certificate of a domain configuration, you must delete and recreate it\.

## Deleting Domain configurations<a name="iot-custom-endpoints-managing-delete"></a>

Before you delete a domain configuration, use the [UpdateDomainConfiguration](https://docs.aws.amazon.com/iot/latest/apireference/API_UpdateDomainConfiguration.html) API to set the status to `DISABLED`\. This helps you avoid accidentally deleting the endpoint\. After you disable the domain configuration, delete it by using the [DeleteDomainConfiguration](https://docs.aws.amazon.com/iot/latest/apireference/API_DeleteDomainConfiguration.html) API\.

After you delete a domain configuration, AWS IoT no longer serves the server certificate associated with that custom domain\.