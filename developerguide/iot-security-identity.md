# Security and Identity for AWS IoT<a name="iot-security-identity"></a>

Each connected device must have a credential to access the message broker or the Device Shadow service\. All traffic to and from AWS IoT must be encrypted over Transport Layer Security \(TLS\)\. Device credentials must be kept safe in order to send data securely to the message broker\. AWS Cloud security mechanisms protect data as it moves between AWS IoT and other devices or AWS services\.

![\[Security and Identity Overview\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/thunderball-overview.png)
+ You are responsible for managing device credentials \(X\.509 certificates, AWS credentials\) on your devices and policies in AWS IoT\. You are responsible for assigning unique identities to each device and managing the permissions for a device or group of devices\.
+ Your devices should connect using X\.509 certificates or Amazon Cognito identities over a secure connection according to the AWS IoT connection model\. During research and development, and for some applications that make API calls or use WebSockets you can also use IAM users and groups, or custom authentication tokens\. 
+ When using AWS IoT authentication, the message broker authenticates and authorizes all actions in your account\. The message broker is responsible for authenticating your devices, securely ingesting device data, and adhering to the access permissions you place on devices using policies\.
+ When using custom authentication, a custom authorizer is responsible for authenticating your devices and providing an AWS IoT/IAM policy to authorize actions in your account\.
+ The AWS IoT rules engine forwards device data to other devices and other AWS services according to rules you define\. It uses AWS access management systems to securely transfer data to its final destination\.

## Transport Security<a name="transport-security"></a>

The AWS IoT message broker and Device Shadow service encrypt all communication with [TLS](https://en.wikipedia.org/wiki/Transport_Layer_Security) [version 1\.2](https://en.wikipedia.org/wiki/Transport_Layer_Security#TLS_1.2)\. TLS is used to ensure the confidentiality of the application protocols \(MQTT, HTTP\) supported by AWS IoT\. TLS is available in a number of programming languages and operating systems\.

For MQTT, TLS encrypts the connection between the device and the broker\. TLS client authentication is used by AWS IoT to identify devices\. For HTTP, TLS encrypts the connection between the device and the broker\. Authentication is delegated to AWS Signature Version 4\. 

### TLS Cipher Suite Support<a name="tls-cipher-suite-support"></a>

AWS IoT supports the following cipher suites: 
+ ECDHE\-ECDSA\-AES128\-GCM\-SHA256 \(recommended\)
+ ECDHE\-RSA\-AES128\-GCM\-SHA256 \(recommended\)
+ ECDHE\-ECDSA\-AES128\-SHA256
+ ECDHE\-RSA\-AES128\-SHA256
+ ECDHE\-ECDSA\-AES128\-SHA
+ ECDHE\-RSA\-AES128\-SHA
+ ECDHE\-ECDSA\-AES256\-GCM\-SHA384
+ ECDHE\-RSA\-AES256\-GCM\-SHA384
+ ECDHE\-ECDSA\-AES256\-SHA384
+ ECDHE\-RSA\-AES256\-SHA384
+ ECDHE\-RSA\-AES256\-SHA
+ ECDHE\-ECDSA\-AES256\-SHA
+ AES128\-GCM\-SHA256
+ AES128\-SHA256
+ AES128\-SHA
+ AES256\-GCM\-SHA384
+ AES256\-SHA256
+ AES256\-SHA​