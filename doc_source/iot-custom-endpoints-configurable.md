# Configurable endpoints<a name="iot-custom-endpoints-configurable"></a>

**Note**  
This feature is currently unavailable in the China and GovCloud regions\.

With AWS IoT Core, you can configure and manage the behaviors of your data endpoints by using domain configurations\. You can generate multiple AWS IoT Core data endpoints, and also customize data endpoints with your own fully qualified domain names \(and associated server certificates\) and authorizers\. For more information about custom authorizers, see [Custom authentication](custom-authentication.md)\. 

AWS IoT Core uses the server name indication \(SNI\) TLS extension to apply domain configurations\. Devices must use this extension when they connect\. They also must pass a server name that is identical to the domain name that you specify in the domain configuration\.\* To test this service, use the v2 version of the [AWS IoT Device SDKs](https://github.com/aws) in GitHub\. 

**Note**  
If you create multiple data endpoints in your account, they will share AWS IoT Core resources such as MQTT topics, device shadows, and rules\.

You can use domain configurations to simplify tasks such as the following\.
+ Migrate devices to AWS IoT\.
+ Support heterogeneous device fleets by maintaining separate domain configurations for separate device types\.
+ Maintain brand identity \(for example, through domain name\) while migrating application infrastructure to AWS IoT\.

You can configure a fully qualified domain name \(FQDN\) and the associated server certificate too\. You can also associate a custom authorizer\. For more information, see [Custom authentication](custom-authentication.md)\.

**Note**  
AWS IoT uses the server name indication \(SNI\) TLS extension to apply domain configurations\. Devices must use this extension when connecting and pass a server name that is identical to the domain name that is specified in the domain configuration\. To test this service, use the v2 version of each [AWS IoT Device SDK in GitHub](https://github.com/aws)\.

**Topics**
+ [Creating and configuring AWS\-managed domains](iot-custom-endpoints-configurable-aws.md)
+ [Creating and configuring custom domains](iot-custom-endpoints-configurable-custom.md)
+ [Managing domain configurations](iot-custom-endpoints-managing.md)