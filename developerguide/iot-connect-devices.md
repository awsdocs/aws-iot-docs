# Connecting devices to AWS IoT<a name="iot-connect-devices"></a>

Devices connect to AWS IoT and other services through AWS IoT Core\. Through AWS IoT Core, devices send and receive messages using device endpoints that are specific to your account\. The [AWS IoT Device SDKs](#iot-connect-device-sdks) support device communications using the MQTT and WSS protocols\. For more information about the protocols that devices can use, see [Device communication protocols](protocols.md)\.

**The message broker**  
AWS IoT manages device communication through a message broker\. Devices and clients publish messages to the message broker and also subscribe to messages that the message broker publishes\. Messages are identified by an application\-defined [*topic*](topics.md)\. When the message broker receives a message published by a device or client, it republishes that message to the devices and clients that have subscribed to the message's topic\. The message broker also forwards messages to the AWS IoT [rules](iot-rules.md) engine, which can act on the content of the message\.

**AWS IoT message security**  
Device connections to AWS IoT use [X\.509 client certificates](x509-client-certs.md) and [AWS signature V4](https://docs.aws.amazon.com/general/latest/gr/signing_aws_api_requests.html) for authentication\. Device communications are secured by TLS version 1\.2 and AWS IoT requires devices to send the [Server Name Indication \(SNI\) extension](https://tools.ietf.org/html/rfc3546#section-3.1) when they connect\. For more information, see [Transport Security in AWS IoT](transport-security.html)\.

## AWS IoT device data and service endpoints<a name="iot-connect-device-endpoints"></a>

Each account has several device endpoints that are unique to the account and support specific IoT functions\. The AWS IoT device data endpoints support a publish/subscribe protocol that is designed for the communication needs of IoT devices; however, other clients, such as apps and services, can also use this interface if their application requires the specialized features that these endpoints provide\. The AWS IoT device service endpoints support device\-centric access to security and management services\.

To learn your account's device endpoint for a specific purpose, use the describe\-endpoint CLI command shown here, or the `DescribeEndpoint` REST API, and provide the *`endpointType`* parameter value from the following table\.

```
aws iot describe-endpoint --endpoint-type endpointType
```

This command returns an endpoint in the following format: `account-specific-prefix.iot.aws-region.amazonaws.com`\.

Every customer has an `iot:Data-ATS` and an `iot:Data` endpoint\. Each endpoint uses an X\.509 certificate to authenticate the client\. We strongly recommend that customers use the newer `iot:Data-ATS` endpoint type to avoid issues related to the widespread distrust of Symantec certificate authorities\. We provide the `iot:Data` endpoint for devices to retrieve data from old endpoints that use VeriSign certificates for backward compatibility\. For more information, see [Server Authentication](server-authentication.html)\.


**AWS IoT endpoints for devices**  

|  Endpoint purpose  |  *`endpointType`* value  |  Description  | 
| --- | --- | --- | 
|  IoT data  |  `iot:Data-ATS`  |  Used to send and receive data to and from the message broker, [Device Shadow](iot-device-shadows.md), and [Rules Engine](iot-rules.md) components of AWS IoT\. `iot:Data-ATS` returns an ATS signed data endpoint\.  | 
| IoT data \(legacy\) |  `iot:Data`  | iot:Data returns a VeriSign signed data endpoint provided for backward compatibility\. | 
|  IoT credential access  |  `iot:CredentialProvider`  |  Used to exchange a device's built\-in X\.509 certificate for temporary credentials to connect directly with other AWS services\. For more information about connecting to other AWS services, see [Authorizing Direct Calls to AWS Services](authorizing-direct-aws.md)\.  | 
|  IoT job management  |  `iot:Jobs`  |  Used to enable devices to interact with the AWS IoT Jobs service using the [Jobs Device HTTPS APIs](jobs-api.md#jobs-mqtt-api)\.  | 

You can also use your own fully\-qualified domain name \(FQDN\), such as *example\.com*, and the associated server certificate to connect devices to AWS IoT by using [Configurable endpoints \(beta\)](iot-custom-endpoints-configurable.md), which is currently in public beta\.

## AWS IoT Device SDKs<a name="iot-connect-device-sdks"></a>

The AWS IoT Device SDKs help you connect your IoT devices to AWS IoT Core and they support MQTT and MQTT over WSS protocols\.

The AWS IoT Device SDKs differ from the AWS SDKs in that the AWS IoT Device SDKs support the specialized communications needs of IoT devices, but don't support all of the services supported by the AWS SDKs\. The AWS IoT Device SDKs are compatible with the AWS SDKs that support all of the AWS services; however, they use different authentication methods and connect to different endpoints, which could make using the AWS SDKs impractical on an IoT device\.

**Mobile devices**  
The [AWS Mobile SDKs](iot-connect-service.md#iot-connect-mobile-sdks) support both MQTT device communications, some of the AWS IoT service APIs, and the APIs of other AWS services\. If you're developing on a supported mobile device, review its SDK to see if it's the best option for developing your IoT solution\.

------
#### [ C\+\+ ]

**AWS IoT C\+\+ Device SDK**

The AWS IoT C\+\+ Device SDK allows developers to build connected applications using AWS and the APIs of the AWS IoT Core services\. Specifically, this SDK was designed for devices that are not resource constrained and require advanced features such as message queuing, multi\-threading support, and the latest language features\. For more information, see the following:
+ [AWS IoT C\+\+ Device SDK v2 on GitHub](https://github.com/aws/aws-iot-device-sdk-cpp-v2)
+ [AWS IoT C\+\+ Device SDK v2 Readme](https://github.com/aws/aws-iot-device-sdk-cpp-v2/blob/master/README.md)

------
#### [ Python ]

**AWS IoT Device SDK for Python**

The AWS IoT Device SDK for Python makes it possible for developers to write Python scripts to use their devices to access the AWS IoT platform through MQTT or MQTT over the WebSocket protocol\. By connecting their devices to the APIs of the AWS IoT Core services, users can securely work with the message broker, rules, and Device Shadow service that AWS IoT Core provides and with other AWS services like AWS Lambda, Amazon Kinesis, and Amazon S3, and more\.
+ [AWS IoT Device SDK for Python v2 on GitHub](https://github.com/aws/aws-iot-device-sdk-python-v2)
+ [AWS IoT Device SDK for Python v2 Readme](https://github.com/aws/aws-iot-device-sdk-python-v2/blob/master/README.md)

------
#### [ JavaScript ]

**AWS IoT Device SDK for JavaScript**

The AWS IoT Device SDK for JavaScript makes it possible for developers to write JavaScript applications that access APIs of the AWS IoT Core using MQTT or MQTT over the WebSocket protocol\. It can be used in Node\.js environments and browser applications\. For more information, see the following:
+ [AWS IoT Device SDK for JavaScript v2 on GitHub](https://github.com/aws/aws-iot-device-sdk-js-v2)
+ [AWS IoT Device SDK for JavaScript v2 API documentation](https://aws.github.io/aws-iot-device-sdk-js-v2/index.html)
+ [AWS IoT Device SDK for JavaScript v2 Readme](https://github.com/aws/aws-iot-device-sdk-js-v2/blob/master/README.md)

------
#### [ Java ]

**AWS IoT Device SDK for Java**

The AWS IoT Device SDK for Java makes it possible for Java developers to access the APIs of the AWS IoT Core through MQTT or MQTT over the WebSocket protocol\. The SDK supports the Device Shadow service\. You can access shadows by using HTTP methods, including GET, UPDATE, and DELETE\. The SDK also supports a simplified shadow access model, which allows developers to exchange data with shadows by using getter and setter methods, without having to serialize or deserialize any JSON documents\. For more information, see the following:
+ [AWS IoT Device SDK for Java v2 on GitHub](https://github.com/aws/aws-iot-device-sdk-java-v2)
+ [AWS IoT Device SDK for Java v2 Readme](https://github.com/aws/aws-iot-device-sdk-java-dv2/blob/master/README.md)

------
#### [ Embedded C ]

**AWS IoT Device SDK for Embedded C**

**Important**  
This SDK is intended for use by experienced embedded\-software developers\.

The AWS IoT Device SDK for Embedded C is a collection of C source files that can be used in embedded applications to securely connect to the AWS IoT platform\. It includes transport clients, TLS implementations, and examples for their use\. It also supports AWS IoT\-specific features such as an API to access the Device Shadow service\. 

For Fleet Provisioning, use the `v4_beta` version of the AWS IoT Device SDK for Embedded C at [https://github\.com/aws/aws\-iot\-device\-sdk\-embedded\-C/tree/v4\_beta](https://github.com/aws/aws-iot-device-sdk-embedded-C/tree/v4_beta)\.

The AWS IoT Device SDK for Embedded C is generally targeted at resource constrained devices that require an optimized C language runtime\. You can use the SDK on any operating system and host it on any processor type \(for example, MCUs and MPUs\)\. If your device has sufficient memory and processing resources available, we recommend that you use one of the other AWS IoT Device and Mobile SDKs, such as the AWS IoT Device SDK for C\+\+, Java, JavaScript, or Python\.

In general, the AWS IoT Device SDK for Embedded C is intended for systems that use MCUs or low\-end MPUs that run embedded operating systems\. This SDK is distributed as source code and is intended to be built into customer firmware along with application code, other libraries, and FreeRTOS\. For more information, see the following:
+ [AWS IoT Device SDK for Embedded C on GitHub](https://github.com/aws/aws-iot-device-sdk-embedded-C)
+ [AWS IoT Device SDK for Embedded C Readme](https://github.com/aws/aws-iot-device-sdk-embedded-C/blob/master/README.md)
+ [AWS IoT Device SDK for Embedded C Porting Guide](https://github.com/aws/aws-iot-device-sdk-embedded-C/blob/master/PortingGuide.md)

------