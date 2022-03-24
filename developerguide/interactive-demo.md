# Try the AWS IoT Core interactive tutorial<a name="interactive-demo"></a>

The interactive tutorial shows the components of a simple IoT solution built on AWS IoT\. The tutorial's animations show how IoT devices interact with AWS IoT Core services\. This topic provides a preview of the AWS IoT Core interactive tutorial\. 

To run the demo, you must first [Set up your AWS account](setting-up.md)\. The tutorial, however, doesn't require any AWS IoT resources, additional software, or any coding\.

Expect to spend about 5\-10 minutes on this demo\. Giving yourself 10 minutes will allow more time to consider each of the steps\.

**To run the AWS IoT Core interactive tutorial**

1. Open the [Learning hub](https://console.aws.amazon.com/iot/home#/learnHub) in the AWS IoT console\.

   On the **Welcome to the AWS IoT console** page, in the **See how AWS IoT works** tile, choose **Start the tutorial**\.  
![\[AWS IoT console Learning hub\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/aws-iot-learn-home-demo.png)

1. In the **AWS IoT Interactive Tutorial** page, review the parts of the tutorial, and choose **Start tutorial** when you're ready to continue\.

**The following sections describe how the **AWS IoT Interactive Tutorial** presents these AWS IoT Core features:**
+ [Connecting IoT devices](#interactive-demo-part1)
+ [Saving offline device state](#interactive-demo-part2)
+ [Routing device data to services](#interactive-demo-part3)

## Connecting IoT devices<a name="interactive-demo-part1"></a>

Learn how IoT devices communicate with AWS IoT Core\.

![\[AWS IoT interactive tutorial - Step 1\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/aws-iot-learn-demo-step-1.png)

The animation in this step shows how two devices, the control device on the left and a smart lamp in the house on the right, connect and communicate with AWS IoT Core in the cloud\. The animation shows the devices communicating with AWS IoT Core and reacting to the messages they receive\.

The image in the console includes animations that don't appear in this image\.

For more information about connecting devices to AWS IoT Core, see [Connecting to AWS IoT Core](connect-to-iot.md)\.

## Saving offline device state<a name="interactive-demo-part2"></a>

Learn how AWS IoT Core saves device state for while a device or app is offline\.

![\[AWS IoT interactive tutorial - Step2\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/aws-iot-learn-demo-step-2.png)

The animation in this step shows how the Device Shadow service in AWS IoT Core saves device state information for the control device and the smart lamp\. While the smart lamp is offline, the Device Shadow saves commands from the control device\.

When the smart lamp reconnects to AWS IoT Core, it retrieves those commands\. When the control device is offline, the Device Shadow saves state information from the smart lamp\. When the control device reconnects, it retrieves the current state of the smart lamp to update its display\.

For more information about Device Shadows, see [AWS IoT Device Shadow service](iot-device-shadows.md)\.

## Routing device data to services<a name="interactive-demo-part3"></a>

Learn how AWS IoT Core sends device state to other AWS services\.

![\[AWS IoT interactive tutorial - Step3\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/aws-iot-learn-demo-step-3.png)

The animation in this step shows how AWS IoT Core sends data from the devices to other AWS services by using AWS IoT rules\. AWS IoT rules subscribe to specific messages from the devices, interpret the data in those messages, and route the interpreted data to other services\. In this example, an AWS IoT rule interprets data from a motion sensor and sends commands to a Device Shadow, which then sends them to the smart bulb\. As in the previous example, the Device Shadow stores the device\-state info for the control device\.

For more information about AWS IoT rules, see [Rules for AWS IoT](iot-rules.md)\.