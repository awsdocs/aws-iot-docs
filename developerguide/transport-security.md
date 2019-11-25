# Transport Security in AWS IoT<a name="transport-security"></a>

The AWS IoT message broker and Device Shadow service encrypt all communication with [TLS](https://en.wikipedia.org/wiki/Transport_Layer_Security) [version 1\.2](https://en.wikipedia.org/wiki/Transport_Layer_Security#TLS_1.2)\. TLS is used to ensure the confidentiality of the application protocols \(MQTT, HTTP\) supported by AWS IoT\. TLS is available in a number of programming languages and operating systems\.

For MQTT, TLS encrypts the connection between the device and the broker\. TLS client authentication is used by AWS IoT to identify devices\. For HTTP, TLS encrypts the connection between the device and the broker\. Authentication is delegated to AWS Signature Version 4\. 

## TLS Cipher Suite Support<a name="tls-cipher-suite-support"></a>

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