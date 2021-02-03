# Transport security in AWS IoT<a name="transport-security"></a>

The AWS IoT message broker and Device Shadow service encrypt all communication while in\-transit by using [TLS](https://en.wikipedia.org/wiki/Transport_Layer_Security) [version 1\.2](https://en.wikipedia.org/wiki/Transport_Layer_Security#TLS_1.2)\. TLS is used to ensure the confidentiality of the application protocols \(MQTT, HTTP, and WebSocket\) supported by AWS IoT\. TLS support is available in a number of programming languages and operating systems\. Data within AWS is encrypted by the specific AWS service\. For more information about data encryption on other AWS services, see the security documentation for that service\.

For MQTT, TLS encrypts the connection between the device and the broker\. TLS client authentication is used by AWS IoT to identify devices\. For HTTP, TLS encrypts the connection between the device and the broker\. Authentication is delegated to AWS Signature Version 4\.

AWS IoT requires devices to send the [Server Name Indication \(SNI\) extension](https://tools.ietf.org/html/rfc3546#section-3.1) to the Transport Layer Security \(TLS\) protocol and provide the complete endpoint address in the `host_name` field\. The `host_name` field must contain the endpoint you are calling, and it must be:
+ The `endpointAddress` returned by aws iot [describe\-endpoint](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/iot/describe-endpoint.html) \-\-endpoint\-type iot:Data\-ATS

  or
+ The `domainName` returned by aws iot [describe\-domain\-configuration](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/iot/describe-domain-configuration.html) –\-domain\-configuration\-name "*domain\_configuration\_name*"

Connections attempted by devices without the correct `host_name` value will be refused and logged in CloudWatch\.

AWS IoT does not support the SessionTicket TLS extension\.

## Transport security for LoRaWAN wireless devices<a name="tls-lorawan"></a>

LoRaWAN devices follow the security practices described in [LoRaWAN ™ SECURITY: A White Paper Prepared for the LoRa Alliance™ by Gemalto, Actility, and Semtech](https://lora-alliance.org/sites/default/files/2019-05/lorawan_security_whitepaper.pdf)\. 

For more information about transport security with LoRaWAN devices, see [Data security with AWS IoT Core for LoRaWAN](connect-iot-lorawan-security.md)\.

## TLS cipher suite support<a name="tls-cipher-suite-support"></a>

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
+ AES256\-SHA