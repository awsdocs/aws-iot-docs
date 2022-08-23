# How AWS IoT works<a name="aws-iot-how-it-works"></a>

 AWS IoT provides cloud services and device support that you can use to implement IoT solutions\. AWS provides many cloud services to support IoT\-based applications\. So to help you understand where to start, this section provides a diagram and definition of essential concepts to introduce you to the IoT universe\. 

## The IoT universe<a name="iot-universe"></a>

In general, the Internet of Things \(IoT\) consists of the key components shown in this diagram\.

![\[The IoT universe\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/iot-universe.png)

### Apps<a name="iot-universe-apps"></a>

 Apps give end users access to IoT devices and the features provided by the cloud services to which those devices are connected\. 

### Cloud services<a name="iot-universe-cloud"></a>

Cloud services are distributed, large\-scale data storage and processing services that are connected to the internet\. Examples include: 
+ IoT connection and management services 

  *AWS IoT is an example of an IoT connection and management service*\.
+ Compute services, such as Amazon Elastic Compute Cloud and AWS Lambda
+ Database services, such as Amazon DynamoDB

### Communications<a name="iot-universe-comms"></a>

 Devices communicate with cloud services by using various technologies and protocols\. Examples include: 
+ Wi\-Fi/Broadband internet
+ Broadband cellular data
+ Narrow\-band cellular data
+ Long\-range Wide Area Network \(LoRaWAN\)
+ Proprietary RF communications

### Devices<a name="iot-universe-devices"></a>

A device is a type of hardware that manages interfaces and communications\. Devices are usually located in close proximity to the real\-world interfaces they monitor and control\. Devices can include computing and storage resources, such as microcontrollers, CPU, memory\. Examples include: 
+ Raspberry Pi
+ Arduino
+ Voice\-interface assistants
+ LoRaWAN and devices
+ Amazon Sidewalk devices
+ Custom IoT devices

### Interfaces<a name="iot-universe-interfaces"></a>

 An interface is a component that connects a device to the physical world\. 
+ User interfaces

  Components that allow devices and users to communicate with each other\.
  + Input interfaces

    Enable a user to communicate with a device

    Examples: keypad, button
  + Output interfaces

    Enable a device to communicate with a user

    Examples: Alpha\-numeric display, graphical display, indicator light, alarm bell
+ Sensors

  Input components that measure or sense something in the outside world in a way that a device understands\. Examples include:
  + Temperature sensor \(converts temperature to an analog or digital signal\)
  + Humidity sensor \(converts relative humidity to an analog or digital signal\)
  + Analog to digital convertor \(converts an analog voltage to a numeric value\)
  + Ultrasonic distance measuring unit \(converts a distance to a numeric value\)
  + Optical sensor \(converts a light level to a numeric value\)
  + Camera \(converts image data to digital data\)
+ Actuators

  Output components that the device can use to control something in the outside world\. Examples include:
  + Stepper motors \(convert electric signals to movement\)
  + Relays \(control high electric voltages and currents\)

## AWS IoT services overview<a name="aws-iot-components"></a>

In the IoT universe, AWS IoT provides the services that support the devices that interact with the world and the data that passes between them and AWS IoT\. AWS IoT is made up of the services that are shown in this illustration to support your IoT solution\.

![\[AWS IoT architecture\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/architecture-diagram.png)

### AWS IoT device software<a name="aws-iot-components-device"></a>

AWS IoT provides this software to support your IoT devices\.

**AWS IoT Device SDKs**  
The [AWS IoT Device and Mobile SDKs](iot-sdks.md) help you efficiently connect your devices to AWS IoT\. The AWS IoT Device and Mobile SDKs include open\-source libraries, developer guides with samples, and porting guides so that you can build innovative IoT products or solutions on your choice of hardware platforms\.

**AWS IoT Device Tester**  
[AWS IoT Device Tester](https://docs.aws.amazon.com/freertos/latest/userguide/device-tester-for-freertos-ug.html) for FreeRTOS and AWS IoT Greengrass is a test automation tool for microcontrollers\. AWS IoT Device Tester tests your device to determine if it will run FreeRTOS or AWS IoT Greengrass and interoperate with AWS IoT services\.

**AWS IoT ExpressLink**  
AWS IoT ExpressLink powers a range of hardware modules developed and offered by [AWS Partners](http://aws.amazon.com/iot-expresslink/partners/?nc=sn&loc=6)\. The connectivity modules include AWS\-validated software, making it faster and easier for you to securely connect devices to the cloud and seamlessly integrate with a range of AWS services\. For more information, visit the [AWS IoT ExpressLink](http://aws.amazon.com/iot-expresslink/) overview page or see the [AWS IoT ExpressLink Programmer's Guide](https://docs.aws.amazon.com/iot-expresslink/latest/programmersguide/elpg.html)\.

**AWS IoT Greengrass**  
 [AWS IoT Greengrass](https://docs.aws.amazon.com/greengrass/) extends AWS IoT to edge devices so they can act locally on the data they generate and use the cloud for management, analytics, and durable storage\. With AWS IoT Greengrass, connected devices can run [AWS Lambda](https://docs.aws.amazon.com/lambda/) functions, Docker containers, or both, execute predictions based on machine learning models, keep device data in sync, and communicate with other devices securely – even when they are not connected to the Internet\.

**FreeRTOS**  
[FreeRTOS](https://docs.aws.amazon.com/freertos/) is an open source, real\-time operating system for microcontrollers that lets you include small, low\-power edge devices in your IoT solution\. FreeRTOS includes a kernel and a growing set of software libraries that support many applications\. FreeRTOS systems can securely connect your small, low\-power devices to [AWS IoT](https://docs.aws.amazon.com/iot/) and support more powerful edge devices running [AWS IoT Greengrass](https://docs.aws.amazon.com/greengrass/)\.

### AWS IoT control services<a name="aws-iot-components-control"></a>

Connect to the following AWS IoT services to manage the devices in your IoT solution\.

**AWS IoT Core**  
[AWS IoT Core](https://docs.aws.amazon.com/iot/) is a managed cloud service that enables connected devices to securely interact with cloud applications and other devices\. AWS IoT Core can support many devices and messages, and it can process and route those messages to AWS IoT endpoints and other devices\. With AWS IoT Core, your applications can interact with all of your devices even when they aren’t connected\.

**AWS IoT Core Device Advisor**  
[AWS IoT Core Device Advisor](https://docs.aws.amazon.com/iot/latest/developerguide/device-advisor.html) is a cloud\-based, fully managed test capability for validating IoT devices during device software development\. Device Advisor provides pre\-built tests that you can use to validate IoT devices for reliable and secure connectivity with AWS IoT Core, before deploying devices to production\.

**AWS IoT Device Defender**  
[AWS IoT Device Defender](https://docs.aws.amazon.com/iot-device-defender/) helps you secure your fleet of IoT devices\. AWS IoT Device Defender continuously audits your IoT configurations to make sure that they aren’t deviating from security best practices\. AWS IoT Device Defender sends an alert when it detects any gaps in your IoT configuration that might create a security risk, such as identity certificates being shared across multiple devices or a device with a revoked identity certificate trying to connect to [AWS IoT Core](https://aws.amazon.com/iot-core/)\.

**AWS IoT Device Management**  
[AWS IoT Device Management](https://docs.aws.amazon.com/iot-device-management/) services help you track, monitor, and manage the plethora of connected devices that make up your device fleets\. AWS IoT Device Management services help you ensure that your IoT devices work properly and securely after they have been deployed\. They also provide secure tunneling to access your devices, monitor their health, detect and remotely troubleshoot problems, as well as services to manage device software and firmware updates\.

### AWS IoT data services<a name="aws-iot-components-data"></a>

Analyze the data from the devices in your IoT solution and take appropriate action by using the following AWS IoT services\.

**Amazon Kinesis Video Streams**  
[Amazon Kinesis Video Streams](https://docs.aws.amazon.com/kinesisvideostreams/latest/dg/what-is-kinesis-video.html) allows you to stream live video from devices to the AWS Cloud, where it is durably stored, encrypted, and indexed, allowing you to access your data through easy\-to\-use APIs\. You can use Amazon Kinesis Video Streams to capture massive amounts of live video data from millions of sources, including smartphones, security cameras, webcams, cameras embedded in cars, drones, and other sources\. Amazon Kinesis Video Streams enables you to play back video for live and on\-demand viewing, and quickly build applications that take advantage of computer vision and video analytics through integration with Amazon Rekognition Video, and libraries for ML frameworks\. You can also send non\-video time\-serialized data such as audio data, thermal imagery, depth data, RADAR data, and more\. 

**Amazon Kinesis Video Streams with WebRTC**  
[Amazon Kinesis Video Streams with WebRTC](https://docs.aws.amazon.com/kinesisvideostreams-webrtc-dg/latest/devguide/what-is-kvswebrtc.html) provides a standards\-compliant WebRTC implementation as a fully managed capability\. You can use Amazon Kinesis Video Streams with WebRTC to securely live stream media or perform two\-way audio or video interaction between any camera IoT device and WebRTC\-compliant mobile or web players\. As a fully managed capability, you don't have to build, operate, or scale any WebRTC\-related cloud infrastructure, such as signaling or media relay servers to securely stream media across applications and devices\. Using Amazon Kinesis Video Streams with WebRTC, you can easily build applications for live peer\-to\-peer media streaming, or real\-time audio or video interactivity between camera IoT devices, web browsers, and mobile devices for a variety of use cases\. 

**AWS IoT Analytics**  
[AWS IoT Analytics](https://docs.aws.amazon.com/iotanalytics/) lets you efficiently run and operationalize sophisticated analytics on massive volumes of unstructured IoT data\. AWS IoT Analytics automates each difficult step that is required to analyze data from IoT devices\. AWS IoT Analytics filters, transforms, and enriches IoT data before storing it in a time\-series data store for analysis\. You can analyze your data by running one\-time or scheduled queries using the built\-in SQL query engine or machine learning\.

**AWS IoT Events**  
[AWS IoT Events](https://docs.aws.amazon.com/iotevents/) detects and responds to events from IoT sensors and applications\. Events are patterns of data that identify more complicated circumstances than expected, such as motion detectors using movement signals to activate lights and security cameras\. AWS IoT Events continuously monitors data from multiple IoT sensors and applications, and integrates with other services, such as AWS IoT Core, IoT SiteWise, DynamoDB, and others to enable early detection and unique insights\.

**AWS IoT FleetWise**  
[AWS IoT FleetWise](https://docs.aws.amazon.com/iot-fleetwise/latest/developerguide/what-is-iotfleetwise.html) is a managed service that you can use to collect and transfer vehicle data to the cloud in near\-real time\. With AWS IoT FleetWise, you can easily collect and organize data from vehicles that use different protocols and data formats\. AWS IoT FleetWise helps to transform low\-level messages into human\-readable values and standardize the data format in the cloud for data analyses\. You can also define data collection schemes to control what data to collect in vehicles and when to transfer it to the cloud\.

**AWS IoT SiteWise**  
[AWS IoT SiteWise](https://docs.aws.amazon.com/iot-sitewise/) collects, stores, organizes, and monitors data passed from industrial equipment by MQTT messages or APIs at scale by providing software that runs on a gateway in your facilities\. The gateway securely connects to your on\-premises data servers and automates the process of collecting and organizing the data and sending it to the AWS Cloud\. 

**AWS IoT TwinMaker**  
[AWS IoT TwinMaker](https://docs.aws.amazon.com/iot-twinmaker/) builds operational digital twins of physical and digital systems\. AWS IoT TwinMaker creates digital visualizations using measurements and analysis from a variety of real\-world sensors, cameras, and enterprise applications to help you keep track of your physical factory, building, or industrial plant\. You can use real\-world data to monitor operations, diagnose and correct errors, and optimize operations\.

## AWS IoT Core services<a name="aws-iot-core-services"></a>

AWS IoT Core provides the services that connect your IoT devices to the AWS Cloud so that other cloud services and applications can interact with your internet\-connected devices\.

![\[A high-level view of AWS IoT Core that shows the device gateway, message broker, rules engine, device shadow, and the other services it provides\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/aws_iot_data_services.png)

The next section describes each of the AWS IoT Core services shown in the illustration\.

### AWS IoT Core messaging services<a name="aws-iot-core-connect"></a>

The AWS IoT Core connectivity services provide secure communication with the IoT devices and manage the messages that pass between them and AWS IoT\.

**Device gateway **  
Enables devices to securely and efficiently communicate with AWS IoT\. Device communication is secured by secure protocols that use X\.509 certificates\. 

**Message broker **  
Provides a secure mechanism for devices and AWS IoT applications to publish and receive messages from each other\. You can use either the MQTT protocol directly or MQTT over WebSocket to publish and subscribe\. For more information about the protocols that AWS IoT supports, see [Device communication protocols](protocols.md)\. Devices and clients can also use the HTTP REST interface to publish data to the message broker\.  
The message broker distributes device data to devices that have subscribed to it and to other AWS IoT Core services, such as the Device Shadow service and the rules engine\.

**AWS IoT Core for LoRaWAN**  
AWS IoT Core for LoRaWAN makes it possible to set up a private LoRaWAN network by connecting your LoRaWAN devices and gateways to AWS without the need to develop and operate a LoRaWAN Network Server \(LNS\)\. Messages received from LoRaWAN devices are sent to the rules engine where they can be formatted and sent to other AWS IoT services\.

**Rules engine **  
The Rules engine connects data from the message broker to other AWS IoT services for storage and additional processing\. For example, you can insert, update, or query a DynamoDB table or invoke a Lambda function based on an expression that you defined in the Rules engine\. You can use an SQL\-based language to select data from message payloads, and then process and send the data to other services, such as Amazon Simple Storage Service \(Amazon S3\), Amazon DynamoDB, and AWS Lambda\. You can also create rules that republish messages to the message broker and on to other subscribers\. For more information, see [Rules for AWS IoT](iot-rules.md)\.

### AWS IoT Core control services<a name="aws-iot-core-control"></a>

The AWS IoT Core control services provide device security, management, and registration features\.

**Custom Authentication service**  
You can define custom authorizers that allow you to manage your own authentication and authorization strategy using a custom authentication service and a Lambda function\. Custom authorizers allow AWS IoT to authenticate your devices and authorize operations using bearer token authentication and authorization strategies\.  
Custom authorizers can implement various authentication strategies; for example, JSON Web Token verification or OAuth provider callout\. They must return policy documents that are used by the device gateway to authorize MQTT operations\. For more information, see [Custom authentication](custom-authentication.md)\.

**Device Provisioning service**  
Allows you to provision devices using a template that describes the resources required for your device: a *thing object*, a certificate, and one or more policies\. A thing object is an entry in the registry that contains attributes that describe a device\. Devices use certificates to authenticate with AWS IoT\. Policies determine which operations a device can perform in AWS IoT\.  
The templates contain variables that are replaced by values in a dictionary \(map\)\. You can use the same template to provision multiple devices just by passing in different values for the template variables in the dictionary\. For more information, see [Device provisioning](iot-provision.md)\.

**Group registry**  
Groups allow you to manage several devices at once by categorizing them into groups\. Groups can also contain groups—you can build a hierarchy of groups\. Any action that you perform on a parent group will apply to its child groups\. The same action also applies to all the devices in the parent group and all devices in the child groups\. Permissions granted to a group will apply to all devices in the group and in all of its child groups\. For more information, see [Managing devices with AWS IoT](iot-thing-management.md)\.

**Jobs service**  
Allows you to define a set of remote operations that are sent to and run on one or more devices connected to AWS IoT\. For example, you can define a job that instructs a set of devices to download and install application or firmware updates, reboot, rotate certificates, or perform remote troubleshooting operations\.  
To create a job, you specify a description of the remote operations to be performed and a list of targets that should perform them\. The targets can be individual devices, groups or both\. For more information, see [Jobs](iot-jobs.md)\.

**Registry**  
Organizes the resources associated with each device in the AWS Cloud\. You register your devices and associate up to three custom attributes with each one\. You can also associate certificates and MQTT client IDs with each device to improve your ability to manage and troubleshoot them\. For more information, see [Managing devices with AWS IoT](iot-thing-management.md)\.

**Security and Identity service **  
Provides shared responsibility for security in the AWS Cloud\. Your devices must keep their credentials safe to securely send data to the message broker\. The message broker and rules engine use AWS security features to send data securely to devices or other AWS services\. For more information, see [Authentication](authentication.md)\.

### AWS IoT Core data services<a name="aws-iot-core-data"></a>

The AWS IoT Core data services help your IoT solutions provide a reliable application experience even with devices that are not always connected\.

**Device shadow **  
A JSON document used to store and retrieve current state information for a device\.

**Device Shadow service **  
The Device Shadow service maintains a device's state so that applications can communicate with a device whether the device is online or not\. When a device is offline, the Device Shadow service manages its data for connected applications\. When the device reconnects, it synchronizes its state with that of its shadow in the Device Shadow service\. Your devices can also publish their current state to a shadow for use by applications or other devices that might not be connected all the time\. For more information, see [AWS IoT Device Shadow service](iot-device-shadows.md)\.

### AWS IoT Core support service<a name="aws-iot-core-integ"></a>

**Alexa Voice Service \(AVS\) Integration for AWS IoT**  
Brings Alexa Voice to any connected device\. AVS for AWS IoT reduces the cost and complexity of integrating Alexa\. This feature uses AWS IoT to offload intensive computational and memory audio tasks from the device to the cloud\. Because of the resulting reduction in the engineering bill of materials \(EBOM\) cost, device makers can cost\-effectively bring Alexa to resource\-constrained IoT devices, and enable consumers to talk directly to Alexa in parts of their home, office, or hotel rooms for an ambient experience\.  
AVS for AWS IoT enables Alexa built\-in functionality on MCUs, such as the ARM Cortex M class with less than 1 MB embedded RAM\. To do so, AVS offloads memory and compute tasks to a virtual Alexa Built\-in device in the cloud\. This reduces EBOM cost by up to 50 percent\. For more information, see [Alexa Voice Service \(AVS\) Integration for AWS IoT](avs-integration-aws-iot.md)\.

**Amazon Sidewalk Integration for AWS IoT Core**  
[Amazon Sidewalk](https://www.amazon.com/Amazon-Sidewalk/b?ie=UTF8&node=21328123011) is a shared network that improves connectivity options to help devices work together better\. Amazon Sidewalk supports a wide range of customer devices such as those that locate pets or valuables, those that provide smart home security and lighting control, and those that provide remote diagnostics for appliances and tools\. Amazon Sidewalk Integration for AWS IoT Core makes it possible for device manufacturers to add their Sidewalk device fleet to the AWS IoT Cloud\.  
For more information, see [Amazon Sidewalk Integration for AWS IoT Core](iot-sidewalk.md)