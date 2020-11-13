# Try the AWS IoT quick connect<a name="iot-quick-start"></a>

In this tutorial, you'll create your first thing object, connect a device to it, and watch it send MQTT messages\. 

You can expect to spend 15\-20 minutes on this tutorial\.

This tutorial is best for people who want to quickly get started with AWS IoT to see how it works in a limited scenario\. If you are looking for an example that will get you started so that you can explore more features and services, try [Explore AWS IoT Core services in hands\-on tutorial](iot-gs-first-thing.md)\.

In this tutorial, you'll download and run software on a device that connects to a *thing object* in AWS IoT Core as part of a very small IoT solution\. The device can be an IoT device, such as a Raspberry Pi; however, and it can also be a personal computer that is running Linux, OS/X, or Windows\.

If your device supports a browser that can run the [AWS IoT console](https://console.aws.amazon.com/iot/home), complete this tutorial on the device\.

If your device does not support a compatible browser, complete this tutorial on a PC or Mac and, when the procedure asks you to download the file, you'll need to download it to your PC or Mac, and then transfer the downloaded file to your device using FTP or similar process\. If you're not sure about how to do this, try to find a device that supports a compatible browser to do this tutorial\.

## Step 1\. Start the tutorial<a name="iot-quick-start-connect"></a>

If possible, complete this procedure on your device; otherwise, be ready to transfer a file to your device later in this procedure\.

1. Open your [AWS IoT console](https://console.aws.amazon.com/iot/home) and from the left menu, choose **Learn**\.  
![\[AWS IoT console home page\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/aws-iot-home.png)

1. On the **Connect to AWS IoT** tile, choose **View connection options**\.  
![\[AWS IoT console Learn home page\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/aws-iot-learn-home.png)

1. In the **Onboard a device** tile, choose **Get started**\.  
![\[AWS IoT console connect option page\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/aws-iot-learn-connect.png)

1. Review the steps that describe what you'll do in this tutorial\. When you're ready to continue, choose **Get started**\.  
![\[AWS IoT console connect quick start overview page\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/aws-iot-learn-connect-overview.png)

## Step 2\. Create a thing object<a name="iot-quick-start-configure"></a>

1. On the **How are you connecting to AWS IoT?** page, select the platform and the language of the AWS IoT Device SDK that you want to use\. This example uses the Linux/OSX platform and the Node\.js SDK\.
**Note**  
Be sure to check the list of prerequisite software required by your selected SDK at the bottom of the console page\.  
You must have the required software installed on your target computer before you continue to the next step\.

   After you select the platform and Device SDK language, choose **Next**\.  
![\[AWS IoT console quick start options page\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/aws-iot-learn-connect-options.png)

1. In the **Name** field, enter the name for your thing object\. The thing name used in this example is **MyIotThing**\.
**Important**  
Double\-check your thing name before you continue\.  
A thing name cannot be changed after the thing object is created\. If you want to change a thing name, you must create a new thing object with the correct thing name and then delete the one with the incorrect name\.  
![\[AWS IoT console thing name page\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/aws-iot-learn-connect-thing-1.png)

1. After you give your thing object a name, choose **Next step**\.  
![\[AWS IoT console thing name page with name\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/aws-iot-learn-connect-thing-1-named.png)

## Step 3\. Download files to your device<a name="iot-quick-start-name"></a>

This page appears after AWS IoT has created the connection kit, which includes the following files and resources that your device requires:
+ The thing's certificate files used to authenticate the device
+ A policy resource to authorize your thing object to interact with AWS IoT
+ The script to download the AWS Device SDK and run the sample program on your device

1. When you're ready to continue, choose the **Download connection kit for** button to download the connection kit for the platform that you selected earlier\.  
![\[AWS IoT console certificate file download page\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/aws-iot-learn-connect-thing-2-before.png)

1. If you're running this procedure on your device, save the connection kit file to a directory from which you can run command line commands\.

   If you're not running this procedure on your device, save the connection kit file to a local directory and then transfer the file to your device\.

1. After you have the connection kit file on the device, choose **Next step** to continue the tutorial\.  
![\[AWS IoT console certificate file download page after connection kit download\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/aws-iot-learn-connect-thing-2-after.png)

## Step 4\. Run the sample<a name="iot-quick-start-install-run"></a>

This procedure is done in a terminal or command window on your device while following the directions displayed in the console\. The commands shown in the console are those for the operating system you selected in [Step 2\. Create a thing object](#iot-quick-start-configure)\. Those shown here are for the Linux/OSX operating systems\. 

1. In a terminal or command window on your device, in the directory with the connection kit file, perform the steps shown in the AWS IoT console\.

   If you using a Windows PowerShell command window and the unzip command doesn't work, replace unzip with expand\-archive and try the command line again\.   
![\[AWS IoT console quick start install and run page\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/aws-iot-learn-connect-thing-3.png)

1. In the terminal or command window on your device, after you enter the command from **Step 3** in the console, you should see an output similar to this\. This output is from the messages the program is sending to and then receiving back from AWS IoT Core\. While the sample program is communicating with AWS IoT Core, you won't see any activity in the console\. To see activity in the console while you run the sample program, see Step 4 of this procedure\.   
![\[AWS IoT console quick start example program output\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/aws-iot-learn-connect-console-output.png)

1. You can repeat the commands from **Step 3/3** in the console \(Step 1 of this procedure\), to run the sample program again\.

1. \(Optional\) If you want to see the messages from your IoT client in the [AWS IoT console](https://console.aws.amazon.com/iot/home), open the [MQTT client](https://console.aws.amazon.com/iot/home#/test) on the **Test** page of the AWS IoT console\. In the **MQTT client**, subscribe to **topic\_1**, and then, on your device, run the program \./start\.sh as described in the previous step\. See [View MQTT messages with the AWS IoT MQTT client](view-mqtt-messages.md) for more information\.

1. After you have finished running the sample app on your device, in the AWS IoT console, choose **Done** to finish the tutorial and see this summary\.  
![\[AWS IoT console quick start complete\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/aws-iot-learn-connect-complete.png)

## Step 5\. Explore further<a name="iot-quick-start-test"></a>

Here are some ideas to explore AWS IoT further after you complete the quick start\.
+ 

**[View MQTT messages in the MQTT client](https://console.aws.amazon.com/iot/home#/test)**  
From the [AWS IoT console](https://console.aws.amazon.com/iot/home) you can open the [MQTT client](https://console.aws.amazon.com/iot/home#/test) on the **Test** page of the AWS IoT console\. In the **MQTT client**, subscribe to **sdk/test/Python**, and then, on your device, run the program \./start\.sh as described in the previous step\. See [View MQTT messages with the AWS IoT MQTT client](view-mqtt-messages.md) for more information\.
+ 

**[Try the AWS IoT Core interactive demo](interactive-demo.md)**  
From the **Learn** page of the AWS IoT console, in the **See how AWS IoT works** tile, choose **Start the tutorial** to start the interactive tutorial\.
+ 

**[Get ready to explore more tutorials](iot-gs-first-thing.md)**  
This quick start gives you just a sample of AWS IoT\. If you want to explore AWS IoT further and learn about the features that make it a powerful IoT solution platform, start preparing your development platform by [Explore AWS IoT Core services in hands\-on tutorial](iot-gs-first-thing.md)\.
