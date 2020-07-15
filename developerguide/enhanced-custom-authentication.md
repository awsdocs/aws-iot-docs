# Enhanced custom authentication \(beta\)<a name="enhanced-custom-authentication"></a>

****  
This feature is currently in public beta and is available only in the US East \(N\. Virginia\) Region\.

Enhanced custom authentication provides greater flexibility in the authentication methods that your custom authorizers can use when the custom authorizers are implemented by configurable endpoints\. This set of preview features enables you to pass tokens to your authorizers by using HTTP query strings in addition to headers\. It also makes token signing optional\. These features also enable authentication by passing credentials in the `username` and `password` fields of an MQTT CONNECT message\. This topic describes how you can use these enhancements and how to add custom authorizers to your configurable endpoints\. For more information, see [Configurable Endpoints \(Beta\)](iot-custom-endpoints-configurable.html)\.

Enhanced custom authentication sends client credentials to your custom authorizers by using one of the following methods:
+ HTTP Publish \(header fields\)
+ HTTP Publish \(query string\)
+ MQTT Connect \(user name and password fields\)
+ HTTP Upgrade to MQTT \(HTTP headers or query string\)
+ HTTP Upgrade to MQTT \(MQTT user name and password

**Note**  
To pass unsigned tokens or use MQTT user name/password authentication, clients must send the Server Name Indication \(SNI\) TLS extension with a fully qualified domain name that corresponds to the name specified in the domain configuration\. For more information on configuring the SNI extension, see [Transport security in AWS IoT](transport-security.md)\. To test this service, use the v2 version of each [AWS IoT device SDK in GitHub](https://github.com/aws)\.

**Topics**
+ [Creating a custom authorizer for enhanced custom authentication](enhanced-custom-auth-create.md)
+ [Invoking a custom authorizer with enhanced custom authentication](enhanced-custom-auth-using.md)