# Connecting to AWS IoT Core service endpoints<a name="iot-connect-service"></a>

You can access the features of the **AWS IoT Core \- control plane** by using the AWS CLI, the AWS SDK for your preferred language, or by calling the REST API directly\. We recommend using the AWS CLI or an AWS SDK to interact with AWS IoT Core because they incorporate the best practices for calling AWS services\. Calling the REST APIs directly is an option, but you must provide [the necessary security credentials](https://docs.aws.amazon.com/general/latest/gr/signing_aws_api_requests.html) that enable access to the API\.

**Note**  
IoT devices should use [AWS IoT Device SDKs](iot-connect-devices.md#iot-connect-device-sdks)\. The Device SDKs are optimized for use on devices, support MQTT communication with AWS IoT, and support the AWS IoT APIs most used by devices\. For more information about the Device SDKs and the features they provide, see [AWS IoT Device SDKs](iot-connect-devices.md#iot-connect-device-sdks)\.  
Mobile devices should use [AWS Mobile SDKs](#iot-connect-mobile-sdks)\. The Mobile SDKs provide support for AWS IoT APIs, MQTT device communications, and the APIs of other AWS services on mobile devices\. For more information about the Mobile SDKs and the features they provide, see [AWS Mobile SDKs](#iot-connect-mobile-sdks)\.

You can use AWS Amplify tools and resources in web and mobile applications to connect more easily to AWS IoT Core\. For more information about connecting to AWS IoT Core by using Amplify, see [Pub Sub Getting Started](https://docs.amplify.aws/lib/pubsub/getting-started/) in the Amplify documentation\.

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

With AWS SDKs, your apps and compatible devices can call AWS IoT APIs and the APIs of other AWS services\. This section provides links to the AWS SDKs and to the API reference documentation for the APIs of the AWS IoT Core services\. 

**The AWS SDKs support these AWS IoT Core APIs**
+ [AWS IoT](https://docs.aws.amazon.com/iot/latest/apireference/welcome.html)
+ [AWS IoT Data Plane](https://docs.aws.amazon.com/iot/latest/apireference/welcome.html)
+ [AWS IoT Jobs Data Plane](https://docs.aws.amazon.com/iot/latest/apireference/welcome.html)
+ [AWS IoT Secure Tunneling](https://docs.aws.amazon.com/iot/latest/apireference/welcome.html)
+ [AWS IoT Wireless](https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/welcome.html)

------
#### [ C\+\+ ]

**To install the [AWS SDK for C\+\+](https://aws.amazon.com/sdk-for-cpp/) and use it to connect to AWS IoT:**

1. Follow the instructions in [Getting Started Using the AWS SDK for C\+\+](https://docs.aws.amazon.com/sdk-for-cpp/v1/developer-guide/getting-started.html)

   These instructions describe how to:
   + Install and build the SDK from source files
   + Provide credentials to use the SDK with your AWS account
   + Initialize and shutdown the SDK in your app or service
   + Create a CMake project to build your app or service

1. Create and run a sample app\. For sample apps that use the AWS SDK for C\+\+, see [AWS SDK for C\+\+ Code Examples](https://docs.aws.amazon.com/sdk-for-cpp/v1/developer-guide/programming-services.html)\.

**Documentation for the AWS IoT Core services that the AWS SDK for C\+\+ supports**
+ [Aws::IoT::IoTClient reference documentation](http://sdk.amazonaws.com/cpp/api/LATEST/class_aws_1_1_io_t_1_1_io_t_client.html)
+ [Aws::IoTDataPlane::IoTDataPlaneClient reference documentation](http://sdk.amazonaws.com/cpp/api/LATEST/class_aws_1_1_io_t_data_plane_1_1_io_t_data_plane_client.html)
+ [Aws::IoTJobsDataPlane::IoTJobsDataPlaneClient reference documentation](http://sdk.amazonaws.com/cpp/api/LATEST/class_aws_1_1_io_t_jobs_data_plane_1_1_io_t_jobs_data_plane_client.html)
+ [Aws::IoTSecureTunneling::IoTSecureTunnelingClient reference documentation](http://sdk.amazonaws.com/cpp/api/LATEST/class_aws_1_1_io_t_secure_tunneling_1_1_io_t_secure_tunneling_client.html)

------
#### [ Go ]

**To install the [AWS SDK for Go](https://aws.amazon.com/sdk-for-go/) and use it to connect to AWS IoT:**

1. Follow the instructions in [Getting Started with the AWS SDK for Go](https://docs.aws.amazon.com/sdk-for-go/v1/developer-guide/setting-up.html)

   These instructions describe how to:
   + Install the AWS SDK for Go
   + Get access keys for the SDK to access your AWS account
   + Import packages into the source code of our apps or services

1. Create and run a sample app\. For sample apps that use the AWS SDK for Go, see [AWS SDK for Go Code Examples](https://docs.aws.amazon.com/sdk-for-go/v1/developer-guide/common-examples.html)\.

**Documentation for the AWS IoT Core services that the AWS SDK for Go supports**
+ [IoT reference documentation](https://docs.aws.amazon.com/sdk-for-go/api/service/iot/)
+ [IoTDataPlane reference documentation](https://docs.aws.amazon.com/sdk-for-go/api/service/iotdataplane/)
+ [IoTJobsDataPlane reference documentation](https://docs.aws.amazon.com/sdk-for-go/api/service/iotjobsdataplane/)
+ [IoTSecureTunneling reference documentation](https://docs.aws.amazon.com/sdk-for-go/api/service/iotsecuretunneling/)

------
#### [ Java ]

**To install the [AWS SDK for Java](https://aws.amazon.com/sdk-for-java/) and use it to connect to AWS IoT:**

1. Follow the instructions in [Getting Started with AWS SDK for Java 2\.x](https://docs.aws.amazon.com/sdk-for-java/v2/developer-guide/getting-started.html)

   These instructions describe how to:
   + Sign up for AWS and Create an IAM User
   + Download the SDK 
   + Set up AWS Credentials and Region 
   + Use the SDK with Apache Maven 
   + Use the SDK with Gradle 

1. Create and run a sample app using one of the [AWS SDK for Java 2\.x Code Examples](https://docs.aws.amazon.com/sdk-for-java/v2/developer-guide/advanced-topics.html)\.

1. Review the [SDK API reference documentation](https://sdk.amazonaws.com/java/api/latest/)

**Documentation for the AWS IoT Core services that the AWS SDK for Java supports**
+ [IotClient reference documentation](https://sdk.amazonaws.com/java/api/latest/software/amazon/awssdk/services/iot/IotClient.html)
+ [IotDataPlaneClient reference documentation](https://sdk.amazonaws.com/java/api/latest/software/amazon/awssdk/services/iotdataplane/IotDataPlaneClient.html)
+ [IotJobsDataPlaneClient reference documentation](https://sdk.amazonaws.com/java/api/latest/software/amazon/awssdk/services/iotjobsdataplane/IotJobsDataPlaneClient.html)
+ [IoTSecureTunnelingClient reference documentation](https://sdk.amazonaws.com/java/api/latest/software/amazon/awssdk/services/iotsecuretunneling/IoTSecureTunnelingClient.html)

------
#### [ JavaScript ]

**To install the [AWS SDK for JavaScript]() and use it to connect to AWS IoT:**

1. Follow the instructions in [Setting Up the AWS SDK for JavaScript](https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/setting-up.html)\. These instructions apply to using the AWS SDK for JavaScript in the browser and with Node\.JS\. Make sure you follow the directions that apply to your installation\. 

   These instructions describe how to:
   + Check for the prerequisites
   + Install the SDK for JavaScript
   + Load the SDK for JavaScript

1. Create and run a sample app to get started with the SDK as the getting started option for your environment describes\.
   + Get started with the [AWS SDK for JavaScript in the Browser](https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/getting-started-browser.html), or
   + Get started with the [AWS SDK for JavaScript in Node\.js](https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/getting-started-nodejs.html)

**Documentation for the AWS IoT Core services that the AWS SDK for JavaScript supports**
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/Iot.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/Iot.html)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/IotData.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/IotData.html)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/IoTJobsDataPlane.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/IoTJobsDataPlane.html)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/IoTSecureTunneling.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/IoTSecureTunneling.html)

------
#### [ \.NET ]

**To install the [AWS SDK for \.NET](https://aws.amazon.com/sdk-for-net/) and use it to connect to AWS IoT:**

1. Follow the instructions in [Setting up your AWS SDK for \.NET environment](https://docs.aws.amazon.com/sdk-for-net/latest/developer-guide/net-dg-setup.html)

1. Follow the instructions in [Setting up your AWS SDK for \.NET project](https://docs.aws.amazon.com/sdk-for-net/latest/developer-guide/net-dg-config.html)

   These instructions describe how to:
   + Start a new project
   + Obtain and configure AWS credentials
   + Install AWS SDK packages

1. Create and run one of the sample programs in [Working with AWS services in the AWS SDK for \.NET](https://docs.aws.amazon.com/sdk-for-net/latest/developer-guide/tutorials-examples.html)

1. Review the [SDK API reference documentation](https://docs.aws.amazon.com/sdkfornet/v3/apidocs/index.html)

**Documentation for the AWS IoT Core services that the AWS SDK for \.NET supports**
+ [Amazon\.IoT\.Model reference documentation](https://docs.aws.amazon.com/sdkfornet/v3/apidocs/items/IoT/NIoTModel.html)
+ [Amazon\.IotData\.Model reference documentation](https://docs.aws.amazon.com/sdkfornet/v3/apidocs/items/IotData/NIotDataModel.html)
+ [Amazon\.IoTJobsDataPlane\.Model reference documentation](https://docs.aws.amazon.com/sdkfornet/v3/apidocs/items/IoTJobsDataPlane/NIoTJobsDataPlaneModel.html)
+ [Amazon\.IoTSecureTunneling\.Model reference documentation](https://docs.aws.amazon.com/sdkfornet/v3/apidocs/items/IoTSecureTunneling/NIoTSecureTunnelingModel.html)

------
#### [ PHP ]

**To install the [AWS SDK for PHP](https://aws.amazon.com/sdk-for-php/) and use it to connect to AWS IoT:**

1. Follow the instructions in [Getting Started with the AWS SDK for PHP Version 3](https://docs.aws.amazon.com/sdk-for-php/v3/developer-guide/getting-started_index.html)

   These instructions describe how to:
   + Check for the prerequisites
   + Install the SDK
   + Apply the SDK to a PHP script

1. Create and run a sample app using one of the [AWS SDK for PHP Version 3 Code Examples](https://docs.aws.amazon.com/sdk-for-php/v3/developer-guide/examples_index.html)

**Documentation for the AWS IoT Core services that the AWS SDK for PHP supports**
+ [IoTClient reference documentation](https://docs.aws.amazon.com/aws-sdk-php/v3/api/class-Aws.Iot.IotClient.html)
+ [IoTDataPlaneClient reference documentation](https://docs.aws.amazon.com/aws-sdk-php/v3/api/class-Aws.IotDataPlane.IotDataPlaneClient.html)
+ [IoTJobsDataPlaneClient reference documentation](https://docs.aws.amazon.com/aws-sdk-php/v3/api/class-Aws.IoTJobsDataPlane.IoTJobsDataPlaneClient.html)
+ [IoTSecureTunnelingClient reference documentation](https://docs.aws.amazon.com/aws-sdk-php/v3/api/class-Aws.IoTSecureTunneling.IoTSecureTunnelingClient.html)

------
#### [ Python ]

**To install the [AWS SDK for Python \(Boto3\)](https://aws.amazon.com/sdk-for-python/) and use it to connect to AWS IoT:**

1. Follow the instructions in the [AWS SDK for Python \(Boto3\) Quickstart](https://boto3.amazonaws.com/v1/documentation/api/latest/guide/quickstart.html)

   These instructions describe how to:
   + Install the SDK
   + Configure the SDK
   + Use the SDK in your code

1. Create and run a sample program that uses the AWS SDK for Python \(Boto3\)

   This program displays the account's currently configured logging options\. After you install the SDK and configure it for your account, you should be able to run this program\.

   ```
   import boto3
   import json
   
   # initialize client
   iot = boto3.client('iot')
   
   # get current logging levels, format them as JSON, and write them to stdout
   response = iot.get_v2_logging_options()
   print(json.dumps(response, indent=4))
   ```

    For more information about the function used in this example, see [Configure AWS IoT logging](configure-logging.md)\.

**Documentation for the AWS IoT Core services that the AWS SDK for Python \(Boto3\) supports**
+ [IoT reference documentation](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/iot.html)
+ [IoTDataPlane reference documentation](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/iot-data.html)
+ [IoTJobsDataPlane reference documentation](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/iot-jobs-data.html)
+ [IoTSecureTunneling reference documentation](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/iotsecuretunneling.html)

------
#### [ Ruby ]

**To install the [AWS SDK for Ruby](https://aws.amazon.com/sdk-for-ruby/) and use it to connect to AWS IoT:**
+ Follow the instructions in [Getting Started with the AWS SDK for Ruby](https://docs.aws.amazon.com/sdk-for-ruby/v3/developer-guide/getting-started.html)

  These instructions describe how to:
  + Install the SDK
  + Configure the SDK
+ Create and run the [Hello World Tutorial](https://docs.aws.amazon.com/sdk-for-ruby/v3/developer-guide/hello.html)

**Documentation for the AWS IoT Core services that the AWS SDK for Ruby supports**
+ [Aws::IoT::Client reference documentation](https://docs.aws.amazon.com/sdk-for-ruby/v3/api/Aws/IoT/Client.html)
+ [Aws::IoTDataPlane::Client reference documentation](https://docs.aws.amazon.com/sdk-for-ruby/v3/api/Aws/IoTDataPlane/Client.html)
+ [Aws::IoTJobsDataPlane::Client reference documentation](https://docs.aws.amazon.com/sdk-for-ruby/v3/api/Aws/IoTJobsDataPlane/Client.html)
+ [Aws::IoTSecureTunneling::Client reference documentation](https://docs.aws.amazon.com/sdk-for-ruby/v3/api/Aws/IoTSecureTunneling/Client.html)

------

## AWS Mobile SDKs<a name="iot-connect-mobile-sdks"></a>

The AWS Mobile SDKs provide mobile app developers platform\-specific support for the APIs of the AWS IoT Core services, IoT device communication using MQTT, and the APIs of other AWS services\. 

------
#### [ Android ]

**AWS Mobile SDK for Android**

The AWS Mobile SDK for Android contains a library, samples, and documentation for developers to build connected mobile applications using AWS\. This SDK also includes support for MQTT device communications and calling the APIs of the AWS IoT Core services\. For more information, see the following:
+ [AWS Mobile SDK for Android on GitHub](https://github.com/aws/aws-sdk-android)
+ [AWS Mobile SDK for Android Readme](https://github.com/aws-amplify/aws-sdk-android/blob/main/README.md#aws-sdk-for-android)
+ [AWS Mobile SDK for Android Samples](https://github.com/awslabs/aws-sdk-android-samples#aws-sdk-for-android-samples)
+ [AWS SDK for Android API reference](https://aws-amplify.github.io/aws-sdk-android/docs/reference/)
+ [AWSIoTClient Class reference documentation](https://aws-amplify.github.io/aws-sdk-android/docs/reference/com/amazonaws/services/iot/AWSIotClient.html)

------
#### [ iOS ]

**AWS Mobile SDK for iOS**

The AWS Mobile SDK for iOS is an open\-source software development kit, distributed under an Apache Open Source license\. The SDK for iOS provides a library, code samples, and documentation to help developers build connected mobile applications using AWS\. This SDK also includes support for MQTT device communications and calling the APIs of the AWS IoT Core services\. For more information, see the following:
+ [AWS Mobile SDK for iOS on GitHub](https://github.com/aws/aws-sdk-ios)
+ [AWS SDK for iOS Readme](https://github.com/aws-amplify/aws-sdk-ios/blob/main/README.md#aws-sdk-for-ios)
+ [AWS SDK for iOS Samples](https://github.com/awslabs/aws-sdk-ios-samples#the-aws-sdk-for-ios-samples)
+ [AWS IoT Class reference docs in the AWS SDK for iOS](https://aws-amplify.github.io/aws-sdk-ios/docs/reference/AWSIoT/index.html)

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
  + [API reference for IoT jobs data](https://docs.aws.amazon.com/iot/latest/apireference/API_Operations_AWS_IoT_Jobs_Data_Plane.html)\.
  + [API reference for IoT secure tunneling](https://docs.aws.amazon.com/iot/latest/apireference/API_Operations_AWS_IoT_Secure_Tunneling.html)\.