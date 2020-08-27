# Connecting to AWS IoT Core service endpoints<a name="iot-connect-service"></a>

You can access the features that the AWS IoT Core services provide by using the AWS CLI, the AWS SDK for your preferred language, or by calling the REST API directly\. We recommend using the AWS CLI or an AWS SDK to interact with AWS IoT Core because they incorporate the best practices for calling AWS services\. Calling the REST APIs directly is an option, but you must provide [the necessary security credentials](https://docs.aws.amazon.com/general/latest/gr/signing_aws_api_requests.html) that enable access to the API\.

**Note**  
IoT devices should use [AWS IoT Device and Mobile SDKs](iot-sdks.md)\. The Device SDKs are optimized for use on devices, support MQTT communication with AWS IoT, and support the AWS IoT APIs most used by devices\. For more information about the Device SDKs and the features they provide, see [AWS IoT Device and Mobile SDKs](iot-sdks.md)\.  
Mobile devices should use [AWS Mobile SDKs](#iot-connect-mobile-sdks)\. The Mobile SDKs provide support for AWS IoT APIs, MQTT device communications, and the APIs of other AWS services on mobile devices\. For more information about the Mobile SDKs and the features they provide, see [AWS Mobile SDKs](#iot-connect-mobile-sdks)\.

The following sections describe the tools and SDKs that you can use to develop and interact with AWS IoT and other AWS services\. For the complete list of AWS tools and development kits that are available to build and manage apps on AWS, see [Tools to Build on AWS](https://aws.amazon.com/tools/)\.

## AWS CLI for AWS IoT Core<a name="iot-connect-cli"></a>

The AWS CLI provides command\-line access to AWS APIs\.
+ 

**Installation**  
For information about how to install the AWS CLI, see [Installing the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-install.html)\.
+ 

**Authentication**  
The AWS CLI uses credentials from your AWS account\.
+ 

**Reference**  
For information about the AWS CLI commands for these AWS IoT Core services, see:
  + [AWS CLI Command Reference for IoT](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/iot/index.html)
  + [AWS CLI Command Reference for IoT data](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/iot-data/index.html)
  + [AWS CLI Command Reference for IoT jobs data](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/iot-jobs-data/index.html)
  + [AWS CLI Command Reference for IoT secure tunneling](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/iotsecuretunneling/index.html)

For tools to manage AWS services and resources in the PowerShell scripting environment, see [AWS Tools for PowerShell](https://aws.amazon.com/powershell/)\.

## AWS SDKs<a name="iot-service-sdks"></a>

The AWS SDKs enable your apps and compatible devices to call AWS IoT APIs and the APIs of other AWS services\. This section provides links to the AWS SDKs and to the API reference documentation for the APIs of the AWS IoT Core services\.

------
#### [ C\+\+ ]

**[AWS SDK for C\+\+](https://aws.amazon.com/sdk-for-cpp/)**
+ [AWS SDK for C\+\+ developer guide](https://github.com/aws/aws-sdk-cpp#aws-sdk-for-c)
+ [Aws::IoT::IoTClient reference documentation](http://sdk.amazonaws.com/cpp/api/LATEST/class_aws_1_1_io_t_1_1_io_t_client.html)
+ [Aws::IoTDataPlane::IoTDataPlaneClient reference documentation](http://sdk.amazonaws.com/cpp/api/LATEST/class_aws_1_1_io_t_data_plane_1_1_io_t_data_plane_client.html)
+ [Aws::IoTJobsDataPlane::IoTJobsDataPlaneClient reference documentation](http://sdk.amazonaws.com/cpp/api/LATEST/class_aws_1_1_io_t_jobs_data_plane_1_1_io_t_jobs_data_plane_client.html)
+ [Aws::IoTSecureTunneling::IoTSecureTunnelingClient reference documentation](http://sdk.amazonaws.com/cpp/api/LATEST/class_aws_1_1_io_t_secure_tunneling_1_1_io_t_secure_tunneling_client.html)

------
#### [ Go ]

**[AWS SDK for Go](https://aws.amazon.com/sdk-for-go/)**
+ [AWS SDK for Go getting started](https://docs.aws.amazon.com/sdk-for-go/v1/developer-guide/)
+ [IoT reference documentation](https://docs.aws.amazon.com/sdk-for-go/api/service/iot/)
+ [IoTDataPlane reference documentation](https://docs.aws.amazon.com/sdk-for-go/api/service/iotdataplane/)
+ [IoTJobsDataPlane reference documentation](https://docs.aws.amazon.com/sdk-for-go/api/service/iotjobsdataplane/)
+ [IoTSecureTunneling reference documentation](https://docs.aws.amazon.com/sdk-for-go/api/service/iotsecuretunneling/)

------
#### [ Java ]

**[AWS SDK for Java](https://aws.amazon.com/sdk-for-java/)**
+ [AWS SDK for Java developer guide](https://docs.aws.amazon.com/sdk-for-java/v2/developer-guide/welcome.html)
+ [SDK API reference documentation](https://sdk.amazonaws.com/java/api/latest/)
+ [IotClient reference documentation](https://sdk.amazonaws.com/java/api/latest/software/amazon/awssdk/services/iot/IotClient.html)
+ [IotDataPlaneClient reference documentation](https://sdk.amazonaws.com/java/api/latest/software/amazon/awssdk/services/iotdataplane/IotDataPlaneClient.html)
+ [IotJobsDataPlaneClient reference documentation](https://sdk.amazonaws.com/java/api/latest/software/amazon/awssdk/services/iotjobsdataplane/IotJobsDataPlaneClient.html)
+ [IoTSecureTunnelingClient reference documentation](https://sdk.amazonaws.com/java/api/latest/software/amazon/awssdk/services/iotsecuretunneling/IoTSecureTunnelingClient.html)

------
#### [ JavaScript ]

**[AWS SDK for JavaScript in the Browser](https://aws.amazon.com/sdk-for-browser/)**

**[AWS SDK for JavaScript in Node\.js](https://aws.amazon.com/sdk-for-node-js/)**
+ [AWS SDK for JavaScript developer guide](https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/welcome.html)
+ [AWS\.Iot reference documentation](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/Iot.html)
+ [AWS\.IotData reference documentation](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/IotData.html)
+ [AWS\.IotJobsDataPlane reference documentation](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/IoTJobsDataPlane.html)
+ [AWS\.IotSecureTunneling reference documentation](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/IoTSecureTunneling.html)

------
#### [ \.NET ]

**[AWS SDK for \.NET](https://aws.amazon.com/sdk-for-net/)**
+ [AWS SDK for \.NET d](https://docs.aws.amazon.com/sdk-for-net/v3/developer-guide/welcome.html)
+ [SDK API reference documentation](https://docs.aws.amazon.com/sdkfornet/v3/apidocs/index.html)
+ [Amazon\.IoT\.Model reference documentation](https://docs.aws.amazon.com/sdkfornet/v3/apidocs/items/IoT/NIoTModel.html)
+ [Amazon\.IotData\.Model reference documentation](https://docs.aws.amazon.com/sdkfornet/v3/apidocs/items/IotData/NIotDataModel.html)
+ [Amazon\.IoTJobsDataPlane\.Model reference documentation](https://docs.aws.amazon.com/sdkfornet/v3/apidocs/items/IoTJobsDataPlane/NIoTJobsDataPlaneModel.html)
+ [Amazon\.IoTSecureTunneling\.Model reference documentation](https://docs.aws.amazon.com/sdkfornet/v3/apidocs/items/IoTSecureTunneling/NIoTSecureTunnelingModel.html)

------
#### [ PHP ]

**[AWS SDK for PHP](https://aws.amazon.com/sdk-for-php/)**
+ [AWS SDK for PHP getting started](https://docs.aws.amazon.com/sdk-for-php/v3/developer-guide/getting-started_index.html)
+ [IoTClient reference documentation](https://docs.aws.amazon.com/aws-sdk-php/v3/api/class-Aws.Iot.IotClient.html)
+ [IoTDataPlaneClient reference documentation](https://docs.aws.amazon.com/aws-sdk-php/v3/api/class-Aws.IotDataPlane.IotDataPlaneClient.html)
+ [IoTJobsDataPlaneClient reference documentation](https://docs.aws.amazon.com/aws-sdk-php/v3/api/class-Aws.IoTJobsDataPlane.IoTJobsDataPlaneClient.html)
+ [IoTSecureTunnelingClient reference documentation](https://docs.aws.amazon.com/aws-sdk-php/v3/api/class-Aws.IoTSecureTunneling.IoTSecureTunnelingClient.html)

------
#### [ Python ]

**[AWS SDK for Python \(Boto3\)](https://aws.amazon.com/sdk-for-python/)**
+ [AWS SDK for Python Quickstart](https://boto3.amazonaws.com/v1/documentation/api/latest/guide/quickstart.html)
+ [IoT reference documentation](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/iot.html)
+ [IoTDataPlane reference documentation](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/iot-data.html)
+ [IoTJobsDataPlane reference documentation](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/iot-jobs-data.html)
+ [IoTSecureTunneling reference documentation](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/iotsecuretunneling.html)

------
#### [ Ruby ]

**[AWS SDK for Ruby](https://aws.amazon.com/sdk-for-ruby/)**
+ [AWS SDK for Ruby developer guide](http://docs.aws.amazon.com/sdk-for-ruby/latest/DeveloperGuide/)
+ [Aws::IoT::Client reference documentation](https://docs.aws.amazon.com/sdk-for-ruby/v3/api/Aws/IoT/Client.html)
+ [Aws::IoTDataPlane::Client reference documentation](https://docs.aws.amazon.com/sdk-for-ruby/v3/api/Aws/IoTDataPlane/Client.html)
+ [Aws::IoTJobsDataPlane::Client reference documentation](https://docs.aws.amazon.com/sdk-for-ruby/v3/api/Aws/IoTJobsDataPlane/Client.html)
+ [Aws::IoTSecureTunneling::Client reference documentation](https://docs.aws.amazon.com/sdk-for-ruby/v3/api/Aws/IoTSecureTunneling/Client.html)

------

## AWS Mobile SDKs<a name="iot-connect-mobile-sdks"></a>

The AWS Mobile SDKs provide mobile app developers platform\-specific support for the APIs of the AWS IoT Core services, IoT device communication using MQTT, and the APIs of other AWS services\. 

------
#### [ Android ]

**AWS SDK for Android**

The AWS SDK for Android contains a library, samples, and documentation for developers to build connected mobile applications using AWS\. This SDK also includes support for MQTT device communications and calling the APIs of the AWS IoT Core services\. For more information, see the following:
+ [AWS Mobile SDK for Android on GitHub](https://github.com/aws/aws-sdk-android)
+ [AWS Mobile SDK for Android Readme](https://github.com/aws/aws-sdk-android/blob/main/README.md)
+ [AWS Mobile SDK for Android Samples](https://github.com/awslabs/aws-sdk-android-samples)
+ [AWS SDK for Android API reference](https://aws-amplify.github.io/aws-sdk-android/docs/reference/)
+ [AWSIoTClient Class reference documentation](https://aws-amplify.github.io/aws-sdk-android/docs/reference/com/amazonaws/services/iot/AWSIotClient.html)

------
#### [ iOS ]

**AWS SDK for iOS**

The AWS SDK for iOS is an open\-source software development kit, distributed under an Apache Open Source license\. The SDK for iOS provides a library, code samples, and documentation to help developers build connected mobile applications using AWS\. This SDK also includes support for MQTT device communications and calling the APIs of the AWS IoT Core services\. For more information, see the following:
+ [AWS SDK for iOS on GitHub](https://github.com/aws/aws-sdk-ios)
+ [AWS SDK for iOS Readme](https://github.com/aws/aws-sdk-ios/blob/main/README.md)
+ [AWS SDK for iOS Samples](https://github.com/aws/aws-sdk-ios/blob/main/README.md#iot-sample-swift)
+ [AWSIoT Class reference docs in the AWS SDK for iOS](https://aws-amplify.github.io/aws-sdk-ios/docs/reference/AWSIoT/index.html)

------

## REST APIs of the AWS IoT Core services<a name="iot-connect-rest"></a>

The REST APIs of the AWS IoT Core services can be called directly by using HTTP requests\.
+ 

**Endpoint URL**  
The service endpoints that expose the REST APIs of the AWS IoT Core services vary by Region and are listed in [AWS IoT Core Endpoints and Quotas](https://docs.aws.amazon.com/general/latest/gr/iot-core.html)\. You must use the endpoint for the Region that has the AWS IoT resources that you want to access, because AWS IoT resources are Region specific\.
+ 

**Authentication**  
The REST APIs of the AWS IoT Core services use AWS IAM credentials for authentication\. For more information, see [Signing AWS API requests](https://docs.aws.amazon.com/general/latest/gr/signing_aws_api_requests.html) in the AWS General Reference\.
+ 

**API reference**  
For information about the specific functions provided by the REST APIs of the AWS IoT Core services, see:
  + [API reference for IoT](https://docs.aws.amazon.com/iot/latest/apireference/API_Operations_AWS_IoT.html)\.
  + [API reference for IoT data](https://docs.aws.amazon.com/iot/latest/apireference/API_Operations_AWS_IoT_Data_Plane.html)\.
  + [API reference for IoT jobs data](https://docs.aws.amazon.com/iot/latest/apireference/API_Operations_AWS_IoT_Secure_Tunneling.html)\.
  + [API reference for IoT secure tunneling](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/iotsecuretunneling/index.html)\.