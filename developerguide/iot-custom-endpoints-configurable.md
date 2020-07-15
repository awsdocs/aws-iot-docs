# Configurable endpoints \(beta\)<a name="iot-custom-endpoints-configurable"></a>

****  
This feature is currently in public beta and is available only in the US East \(N\. Virginia\) Region\.

You can create multiple endpoints for connecting devices to AWS IoT\. You can also configure some details \(such as domain name\) by using domain configurations\. You can use domain configurations to simplify tasks such as the following\.
+ Migrate devices to AWS IoT\.
+ Support heterogeneous device fleets by maintaining separate domain configurations for separate device types\.
+ Maintain brand identity \(for example, through domain name\) while migrating application infrastructure to AWS IoT\.

In the beta release, you can configure a fully qualified domain name \(FQDN\) and the associated server certificate too\. You can also associate a custom authorizer or an enhanced custom authorizer\. For more information, see [Custom authorizers](custom-authorizer.md) and [Enhanced custom authentication \(beta\)](enhanced-custom-authentication.md)\.

**Note**  
AWS IoT uses the server name indication \(SNI\) TLS extension to apply domain configurations\. Devices must use this extension when connecting and pass a server name that is identical to the domain name that is specified in the domain configuration\. To test this service, use the v2 version of each [AWS IoT Device SDK in GitHub](https://github.com/aws)\.

**Topics**
+ [Creating and configuring AWS\-managed domains](iot-custom-endpoints-configurable-aws.md)
+ [Creating and configuring custom domains](iot-custom-endpoints-configurable-custom.md)
+ [Managing Domain configurations](iot-custom-endpoints-managing.md)