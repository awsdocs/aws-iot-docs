# Data encryption in AWS IoT<a name="data-encryption"></a>

Data protection refers to protecting data while in\-transit \(as it travels to and from AWS IoT\) and at rest \(while it is stored on devices or by other AWS services\)\. All data sent to AWS IoT is sent over an TLS connection using MQTT, HTTPS, and WebSocket protocols, making it secure by default while in transit\. AWS IoT devices collect data and then send it to other AWS services for further processing\. For more information about data encryption on other AWS services, see the security documentation for that service\.

FreeRTOS provides a PKCS\#11 library that abstracts key storage, accessing cryptographic objects and managing sessions\. It is your responsibility to use this library to encrypt data at rest on your devices\. For more information, see [ FreeRTOS Public Key Cryptography Standard \(PKCS\) \#11 Library](https://docs.aws.amazon.com/freertos/latest/userguide/security-pkcs.html)\.

## Device Advisor<a name="device-advisor-encryption"></a>

### Encryption in transit<a name="device-advisor-encryption-transit"></a>

Data sent to and from Device Advisor is encrypted in transit\. All data sent to and from the service when using the Device Advisor APIs is encrypted using Signature Version 4\. For more information about how AWS API requests are signed, see [Signing AWS API requests](https://docs.aws.amazon.com/general/latest/gr/signing_aws_api_requests.html)\. All data sent from your test devices to your Device Advisor test endpoint is sent over a TLS connection so it is secure by default in transit\. 