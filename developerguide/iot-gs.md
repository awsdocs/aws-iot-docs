# Getting started with AWS IoT Core<a name="iot-gs"></a>

AWS IoT Core services connect IoT devices to AWS IoT services and other AWS services\. AWS IoT Core includes the device gateway and the message broker, which connect and process messages between your IoT devices and the cloud\.

Here's how you can get started with AWS IoT Core and AWS IoT\.

![\[AWS IoT Core getting started tour map\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/iot-gs-tour-map.png)

This section presents a tour of the AWS IoT Core to introduce its key services and provides several examples of how to connect a device to AWS IoT Core and pass messages between them\. Passing messages between devices and the cloud is fundamental to every IoT solution and is how your devices can interact with other AWS services\.
+ 

**[Set up your AWS account](setting-up.md)**  
Before you can use AWS IoT services, you must set up an AWS account\. If you already have an AWS account and an IAM user for yourself, you can use them and skip this step\.
+ 

**[Try the interactive demo](interactive-demo.md)**  
This demo is best if you want to see what a basic AWS IoT solution can do without connecting a device or downloading any software\. The interactive demo presents a simulated solution built on AWS IoT Core services that illustrates how they interact\.
+ 

**[Try the quick connect tutorial](iot-quick-start.md)**  
This tutorial is best if you want to quickly get started with AWS IoT and see how it works in a limited scenario\. In this tutorial, you'll need a device and you'll install some AWS IoT software on it\. If you don't have an IoT device, you can use your Windows, Linux, or macOS personal computer as a device for this tutorial\. If you want to try AWS IoT, but you don't have a device, try the next option\.
+ 

**[Explore AWS IoT Core services with a hands\-on tutorial](iot-gs-first-thing.md)**  
This tutorial is best for developers who want to get started with AWS IoT so they can continue to explore other AWS IoT Core features such as the rules engine and shadows\. This tutorial follows a process similar to the quick connect tutorial, but provides more details on each step to enable a smoother transition to the more advanced tutorials\.

**Note**  
If you want to try more than one of these getting started tutorials or repeat the same tutorial, you should delete the thing object that you created from an earlier tutorial before you start another one\. If you don't delete the thing object from an earlier tutorial, you will need to use a different thing name for subsequent tutorials\. This is because the thing name must be unique in your account and AWS Region\.

For more information about AWS IoT Core, see [What Is AWS IoT Core](what-is-aws-iot.md)?

**Topics**
+ [Set up your AWS account](setting-up.md)
+ [Try the AWS IoT Core interactive demo](interactive-demo.md)
+ [Try the AWS IoT quick connect](iot-quick-start.md)
+ [Explore AWS IoT Core services in hands\-on tutorial](iot-gs-first-thing.md)
+ [View MQTT messages with the AWS IoT MQTT client](view-mqtt-messages.md)