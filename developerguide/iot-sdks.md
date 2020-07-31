# AWS IoT device and mobile SDKs<a name="iot-sdks"></a>

The AWS IoT Device and Mobile SDKs include open\-source libraries, developer guides with samples, and porting guides so that you can build innovative IoT products or solutions on your choice of hardware platforms\. 

**AWS IoT device SDKs**

These SDKs help you connect your IoT devices to AWS IoT\.
+ [AWS IoT C\+\+ device SDK](#iot-cpp-sdk)
+ [AWS IoT device SDK for Python](#iot-python-sdk)
+ [AWS IoT device SDK for JavaScript](#iot-javascript-sdk)
+ [AWS IoT device SDK for Java](#iot-java-sdk)

**AWS IoT constrained device SDK**

This SDK is slimmed down for resource\-constrained IoT devices to help you connect them to AWS IoT\.
**Note**  
This SDK is intended for use by experienced embedded\-software developers\.
+ [AWS IoT Device SDK for Embedded C](#iot-c-sdk)

**Mobile SDKs**

These SDKs help you develop AWS IoT apps on mobile devices\.
+ [AWS Mobile SDK for Android](#iot-android-sdk)
+ [AWS Mobile SDK for iOS](#iot-ios-sdk)

**Earlier SDK versions**

These are earlier SDKs that have been replaced by newer versions\. These SDK versions are not be updated to include new features and should not be used on new projects\.
+ [Earlier SDK versions](#earlier-sdks)

For the AWS SDKs used to access AWS APIs, see [AWS SDKs and Tools](http://aws.amazon.com/tools/#sdk)\.

## AWS IoT C\+\+ device SDK<a name="iot-cpp-sdk"></a>

The AWS IoT C\+\+ Device SDK allows developers to build connected applications using AWS and the AWS IoT APIs\. Specifically, this SDK was designed for devices that are not resource constrained and require advanced features such as message queuing, multi\-threading support, and the latest language features\. For more information, see the following:
+ [AWS IoT C\+\+ Device SDK v2 on GitHub](https://github.com/aws/aws-iot-device-sdk-cpp-v2)
+ [AWS IoT C\+\+ Device SDK v2 Readme](https://github.com/aws/aws-iot-device-sdk-cpp-v2/blob/master/README.md)

## AWS IoT device SDK for Python<a name="iot-python-sdk"></a>

The AWS IoT Device SDK for Python makes it possible for developers to write Python scripts to use their devices to access the AWS IoT platform through MQTT or MQTT over the WebSocket protocol\. By connecting their devices to AWS IoT, users can securely work with the message broker, rules, and shadows provided by AWS IoT and with other AWS services like AWS Lambda, Kinesis, and Amazon S3, and more\.
+ [AWS IoT Device SDK for Python v2 on GitHub](https://github.com/aws/aws-iot-device-sdk-python-v2)
+ [AWS IoT Device SDK for Python v2 Readme](https://github.com/aws/aws-iot-device-sdk-python-v2/blob/master/README.md)

## AWS IoT device SDK for JavaScript<a name="iot-javascript-sdk"></a>

The aws\-iot\-device\-sdk\.js package makes it possible for developers to write JavaScript applications that access AWS IoT using MQTT or MQTT over the WebSocket protocol\. It can be used in Node\.js environments and browser applications\. For more information, see the following:
+ [AWS IoT Device SDK for JavaScript v2 on GitHub](https://github.com/aws/aws-iot-device-sdk-js-v2)
+ [AWS IoT Device SDK for JavaScript v2 Readme](https://github.com/aws/aws-iot-device-sdk-js-v2/blob/master/README.md)

## AWS IoT device SDK for Java<a name="iot-java-sdk"></a>

The AWS IoT Device SDK for Java makes it possible for Java developers to access the AWS IoT platform through MQTT or MQTT over the WebSocket protocol\. The SDK is built with shadow support\. You can access shadows by using HTTP methods, including GET, UPDATE, and DELETE\. The SDK also supports a simplified shadow access model, which allows developers to exchange data with shadows by just using getter and setter methods, without having to serialize or deserialize any JSON documents\. For more information, see the following:
+ [AWS IoT Device SDK for Java v2 on GitHub](https://github.com/aws/aws-iot-device-sdk-java-v2)
+ [AWS IoT Device SDK for Java v2 Readme](https://github.com/aws/aws-iot-device-sdk-java-v2/blob/master/README.md)

## AWS IoT Device SDK for Embedded C<a name="iot-c-sdk"></a>

The AWS IoT Device SDK for Embedded C is a collection of C source files that can be used in embedded applications to securely connect to the AWS IoT platform\. It includes transport clients, TLS implementations, and examples for their use\. It also supports AWS IoT\-specific features such as an API to access the Device Shadow service\. 

For Fleet Provisioning, use the `v4_beta` version of the AWS IoT Device SDK for Embedded C at [https://github\.com/aws/aws\-iot\-device\-sdk\-embedded\-C/tree/v4\_beta](https://github.com/aws/aws-iot-device-sdk-embedded-C/tree/v4_beta)\.

The AWS IoT Device SDK for Embedded C is generally targeted at resource constrained devices that require an optimized C language runtime\. You can use the SDK on any operating system and host it on any processor type \(for example, MCUs and MPUs\)\. If you have more memory and processing resources available, we recommend that you use one of the higher order AWS IoT Device and Mobile SDKs \(for example, C\+\+, Java, JavaScript, and Python\)\.

**Note**  
This SDK is intended for use by experienced embedded\-software developers\.

In general, the AWS IoT Device SDK for Embedded C is intended for systems that use MCUs or low\-end MPUs that run embedded operating systems\. This SDK is distributed as source code and is intended to be built into customer firmware along with application code, other libraries, and RTOS\. For more information, see the following:
+ [AWS IoT Device SDK for Embedded C on GitHub](https://github.com/aws/aws-iot-device-sdk-embedded-C)
+ [AWS IoT Device SDK for Embedded C Readme](https://github.com/aws/aws-iot-device-sdk-embedded-C/blob/master/README.md)
+ [AWS IoT Device SDK for Embedded C Porting Guide](https://github.com/aws/aws-iot-device-sdk-embedded-C/blob/master/PortingGuide.md)

## AWS Mobile SDK for Android<a name="iot-android-sdk"></a>

The AWS SDK for Android contains a library, samples, and documentation for developers to build connected mobile applications using AWS\. This SDK also includes support for calling AWS IoT APIs\. For more information, see the following:
+ [AWS Mobile SDK for Android on GitHub](https://github.com/aws/aws-sdk-android)
+ [AWS Mobile SDK for Android Readme](https://github.com/aws/aws-sdk-android/blob/main/README.md)
+ [AWS Mobile SDK for Android Samples](https://github.com/awslabs/aws-sdk-android-samples)

## AWS Mobile SDK for iOS<a name="iot-ios-sdk"></a>

The AWS SDK for iOS is an open\-source software development kit, distributed under an Apache Open Source license\. The SDK for iOS provides a library, code samples, and documentation to help developers build connected mobile applications using AWS\. This SDK also includes support for calling the AWS IoT API\.
+ [AWS SDK for iOS on GitHub](https://github.com/aws/aws-sdk-ios)
+ [AWS SDK for iOS Readme](https://github.com/aws/aws-sdk-ios/blob/main/README.md)
+ [AWS SDK for iOS Samples](https://github.com/aws/aws-sdk-ios/blob/main/README.md#iot-sample-swift)

## Earlier SDK versions<a name="earlier-sdks"></a>

These SDKs are receiving only maintenance and security updates\. They will not be updated to include new features\.
+ [AWS IoT C\+\+ Device SDK on GitHub](https://github.com/aws/aws-iot-device-sdk-cpp/tree/release)
+ [AWS IoT C\+\+ Device SDK Readme](https://github.com/aws/aws-iot-device-sdk-cpp/blob/release/README.md)
+ [AWS IoT Device SDK for Python v1 on GitHub](https://github.com/aws/aws-iot-device-sdk-python)
+ [AWS IoT Device SDK for Python v1 Readme](https://github.com/aws/aws-iot-device-sdk-python/blob/master/README.rst)
+ [AWS IoT Device SDK for Java on GitHub](https://github.com/aws/aws-iot-device-sdk-java)
+ [AWS IoT Device SDK for Java Readme](https://github.com/aws/aws-iot-device-sdk-java/blob/master/README.md)
+ [AWS IoT Device SDK for JavaScript on GitHub](https://github.com/aws/aws-iot-device-sdk-js)
+ [AWS IoT Device SDK for JavaScript Readme](https://github.com/aws/aws-iot-device-sdk-js/blob/master/README.md)
+ [Arduino Yún SDK on GitHub](https://github.com/aws/aws-iot-device-sdk-arduino-yun)
+ [Arduino Yún SDK Readme](https://github.com/aws/aws-iot-device-sdk-arduino-yun/blob/master/README.md)