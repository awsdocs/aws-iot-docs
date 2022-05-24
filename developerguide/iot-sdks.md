# AWS IoT Device SDKs, Mobile SDKs, and AWS IoT Device Client<a name="iot-sdks"></a>

This page summarizes the AWS IoT Device SDKs, open\-source libraries, developer guides, sample apps, and porting guides to help you build innovative IoT solutions with AWS IoT and your choice of hardware platforms\.

These SDKs are for use on your IoT device\. If you're developing an IoT app for use on a mobile device, see the [AWS Mobile SDKs](#iot-mobile-sdks)\. If you're developing an IoT app or server\-side program, see the [AWS SDKs](iot-connect-service.md#iot-service-sdks)\.

## AWS IoT Device SDKs<a name="iot-device-sdks"></a>

The AWS IoT Device SDKs include open\-source libraries, developer guides with samples, and porting guides so that you can build innovative IoT products or solutions on your choice of hardware platforms\.

These SDKs help you connect your IoT devices to AWS IoT using the MQTT and WSS protocols\.

------
#### [ C\+\+ ]

**AWS IoT C\+\+ Device SDK**

The AWS IoT C\+\+ Device SDK allows developers to build connected applications using AWS and the AWS IoT APIs\. Specifically, this SDK was designed for devices that are not resource constrained and require advanced features such as message queuing, multi\-threading support, and the latest language features\. For more information, see the following:
+ [AWS IoT Device SDK C\+\+ v2 on GitHub](https://github.com/aws/aws-iot-device-sdk-cpp-v2)
+ [AWS IoT Device SDK C\+\+ v2 Readme](https://github.com/aws/aws-iot-device-sdk-cpp-v2#aws-iot-device-sdk-for-c-v2)
+ [AWS IoT Device SDK C\+\+ v2 Samples](https://github.com/aws/aws-iot-device-sdk-cpp-v2/tree/main/samples#sample-apps-for-the-aws-iot-device-sdk-for-c-v2)
+ [AWS IoT Device SDK C\+\+ v2 API documentation](https://aws.github.io/aws-iot-device-sdk-cpp-v2/)

------
#### [ Python ]

**AWS IoT Device SDK for Python**

The AWS IoT Device SDK for Python makes it possible for developers to write Python scripts to use their devices to access the AWS IoT platform through MQTT or MQTT over the WebSocket protocol\. By connecting their devices to AWS IoT, users can securely work with the message broker, rules, and shadows provided by AWS IoT and with other AWS services like AWS Lambda, Kinesis, and Amazon S3, and more\.
+ [AWS IoT Device SDK for Python v2 on GitHub](https://github.com/aws/aws-iot-device-sdk-python-v2)
+ [AWS IoT Device SDK for Python v2 Readme](https://github.com/aws/aws-iot-device-sdk-python-v2#aws-iot-device-sdk-v2-for-python)
+ [AWS IoT Device SDK for Python v2 Samples](https://github.com/aws/aws-iot-device-sdk-python-v2/tree/main/samples#sample-apps-for-the-aws-iot-device-sdk-v2-for-python)
+ [AWS IoT Device SDK for Python v2 API documentation](https://aws.github.io/aws-iot-device-sdk-python-v2/)

------
#### [ JavaScript ]

**AWS IoT Device SDK for JavaScript**

The aws\-iot\-device\-sdk\.js package makes it possible for developers to write JavaScript applications that access AWS IoT using MQTT or MQTT over the WebSocket protocol\. It can be used in Node\.js environments and browser applications\. For more information, see the following:
+ [AWS IoT Device SDK for JavaScript v2 on GitHub](https://github.com/aws/aws-iot-device-sdk-js-v2)
+ [AWS IoT Device SDK for JavaScript v2 Readme](https://github.com/aws/aws-iot-device-sdk-js-v2#aws-iot-device-sdk-for-javascript-v2)
+ [AWS IoT Device SDK for JavaScript v2 Samples](https://github.com/aws/aws-iot-device-sdk-js-v2/tree/main/samples#sample-apps-for-the-aws-iot-device-sdk-for-javascript-v2)
+ [AWS IoT Device SDK for JavaScript v2 API documentation](https://aws.github.io/aws-iot-device-sdk-js-v2/index.html)

------
#### [ Java ]

**AWS IoT Device SDK for Java**

The AWS IoT Device SDK for Java makes it possible for Java developers to access the AWS IoT platform through MQTT or MQTT over the WebSocket protocol\. The SDK is built with shadow support\. You can access shadows by using HTTP methods, including GET, UPDATE, and DELETE\. The SDK also supports a simplified shadow access model, which allows developers to exchange data with shadows by just using getter and setter methods, without having to serialize or deserialize any JSON documents\. For more information, see the following:
+ [AWS IoT Device SDK for Java v2 on GitHub](https://github.com/aws/aws-iot-device-sdk-java-v2)
+ [AWS IoT Device SDK for Java v2 Readme](https://github.com/aws/aws-iot-device-sdk-java-v2#aws-iot-device-sdk-for-java-v2)
+ [AWS IoT Device SDK for Java v2 Samples](https://github.com/aws/aws-iot-device-sdk-java-v2/tree/main/samples#sample-apps-for-the-aws-iot-device-sdk-for-java-v2)
+ [AWS IoT Device SDK for Java v2 API documentation](https://aws.github.io/aws-iot-device-sdk-java-v2/)

------

### AWS IoT Device SDK for Embedded C<a name="iot-constrained-device-sdk"></a>

**Note**  
This SDK is intended for use by experienced embedded\-software developers\.

The AWS IoT Device SDK for Embedded C \(C\-SDK\) is a collection of C source files under the MIT open source license that can be used in embedded applications to securely connect IoT devices to AWS IoT Core\. It includes an MQTT client, JSON Parser, and AWS IoT Device Shadow, AWS IoT Jobs, AWS IoT Fleet Provisioning, and AWS IoT Device Defender libraries\. This SDK is distributed in source form and can be built into customer firmware along with application code, other libraries, and an operating system \(OS\) of your choice\.

The AWS IoT Device SDK for Embedded C is generally targeted at resource constrained devices that require an optimized C language runtime\. You can use the SDK on any operating system and host it on any processor type \(for example, MCUs and MPUs\)\. 

For more information, see the following:
+ [AWS IoT Device SDK for Embedded C on GitHub](https://github.com/aws/aws-iot-device-sdk-embedded-C)
+ [AWS IoT Device SDK for Embedded C Readme](https://github.com/aws/aws-iot-device-sdk-embedded-C#aws-iot-device-sdk-for-embedded-c)
+ [AWS IoT Device SDK for Embedded C Samples](https://docs.aws.amazon.com/embedded-csdk/latest/lib-ref/docs/doxygen/output/html/demos_main.html)

### Earlier AWS IoT Device SDKs versions<a name="earlier-sdks"></a>

These are earlier versions of AWS IoT Device SDKs that have been replaced by the newer versions listed above\. These SDKs are receiving only maintenance and security updates\. They will not be updated to include new features and should not be used on new projects\.
+ [AWS IoT C\+\+ Device SDK on GitHub](https://github.com/aws/aws-iot-device-sdk-cpp/tree/release)
+ [AWS IoT C\+\+ Device SDK Readme](https://github.com/aws/aws-iot-device-sdk-python/blob/master/README.rst#new-version-available)
+ [AWS IoT Device SDK for Python v1 on GitHub](https://github.com/aws/aws-iot-device-sdk-python)
+ [AWS IoT Device SDK for Python v1 Readme](https://github.com/aws/aws-iot-device-sdk-python#new-version-available)
+ [AWS IoT Device SDK for Java on GitHub](https://github.com/aws/aws-iot-device-sdk-java)
+ [AWS IoT Device SDK for Java Readme](https://github.com/aws/aws-iot-device-sdk-java#new-version-available)
+ [AWS IoT Device SDK for JavaScript on GitHub](https://github.com/aws/aws-iot-device-sdk-js)
+ [AWS IoT Device SDK for JavaScript Readme](https://github.com/aws/aws-iot-device-sdk-js#new-version-available)
+ [Arduino Yún SDK on GitHub](https://github.com/aws/aws-iot-device-sdk-arduino-yun)
+ [Arduino Yún SDK Readme](https://github.com/aws/aws-iot-device-sdk-arduino-yun#aws-iot-arduino-y%C3%BAn-sdk)

## AWS Mobile SDKs<a name="iot-mobile-sdks"></a>

The AWS Mobile SDKs provide mobile app developers platform\-specific support for the APIs of the AWS IoT Core services, IoT device communication using MQTT, and the APIs of other AWS services\. 

------
#### [ Android ]

**AWS Mobile SDK for Android**

The AWS Mobile SDK for Android contains a library, samples, and documentation for developers to build connected mobile applications using AWS\. This SDK also includes support for MQTT device communications and calling the APIs of the AWS IoT Core services\. For more information, see the following:
+ [AWS Mobile SDK for Android on GitHub](https://github.com/aws/aws-sdk-android)
+ [AWS Mobile SDK for Android Readme](https://github.com/aws-amplify/aws-sdk-android/blob/main/README.md#aws-sdk-for-android)
+ [AWS Mobile SDK for Android Samples](https://github.com/awslabs/aws-sdk-android-samples#aws-sdk-for-android-samples)
+ [AWS Mobile SDK for Android API reference](https://aws-amplify.github.io/aws-sdk-android/docs/reference/)
+ [AWSIoTClient Class reference documentation](https://aws-amplify.github.io/aws-sdk-android/docs/reference/com/amazonaws/services/iot/AWSIotClient.html)

------
#### [ iOS ]

**AWS Mobile SDK for iOS**

The AWS Mobile SDK for iOS is an open\-source software development kit, distributed under an Apache Open Source license\. The AWS Mobile SDK for iOS provides a library, code samples, and documentation to help developers build connected mobile applications using AWS\. This SDK also includes support for MQTT device communications and calling the APIs of the AWS IoT Core services\. For more information, see the following:
+ [AWS Mobile SDK for iOS on GitHub](https://github.com/aws/aws-sdk-ios)
+ [AWS Mobile SDK for iOS Readme](https://github.com/aws-amplify/aws-sdk-ios/blob/main/README.md#aws-sdk-for-ios)
+ [AWS Mobile SDK for iOS Samples](https://github.com/awslabs/aws-sdk-ios-samples#the-aws-sdk-for-ios-samples)
+ [AWSIoT Class reference docs in the AWS Mobile SDK for iOS](https://aws-amplify.github.io/aws-sdk-ios/docs/reference/AWSIoT/index.html)

------

## AWS IoT Device Client<a name="iot-sdk-device-client"></a>

The AWS IoT Device Client provides code to help your device connect to AWS IoT, perform fleet provisioning tasks, support device security policies, connect using secure tunneling, and process jobs on your device\. You can install this software on your device to handle these routine device tasks so you can focus on your specific solution\.

**Note**  
The AWS IoT Device Client works with microprocessor\-based IoT devices with x86\_64 or ARM processors and common Linux operating systems\.

------
#### [ C\+\+ ]

**AWS IoT Device Client**

For more information about the AWS IoT Device Client in C\+\+, see the following:
+ [AWS IoT Device Client in C\+\+ source code on GitHub](https://github.com/awslabs/aws-iot-device-client)
+ [AWS IoT Device Client in C\+\+ Readme](https://github.com/awslabs/aws-iot-device-client#aws-iot-device-client)

------
