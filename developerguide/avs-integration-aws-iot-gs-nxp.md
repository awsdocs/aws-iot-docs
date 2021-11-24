# Getting started with Alexa Voice Service \(AVS\) Integration for AWS IoT on an NXP device<a name="avs-integration-aws-iot-gs-nxp"></a>

 With the NXP i\.MX RT106A development kit, you can preview Alexa Voice Service \(AVS\) Integration for AWS IoT using a preconfigured NXP account\. After you preview the functionality with the NXP account, you customize the firmware \(application source code\) to use your own account\. This topic walks you through the steps to preview with the preconfigured account and to customize your device with your own account\. 

**Topics**
+ [Preview Alexa Voice Service \(AVS\) Integration for AWS IoT with a preconfigured NXP account](#iot-mqtt-alexa-gs-nxp-preview)
+ [Interact with Alexa](#iot-mqtt-alexa-gs-nxp-alexa-interact)
+ [Use your AWS and Alexa Voice Service developer accounts to set up AVS for AWS IoT](#iot-mqtt-alexa-gs-nxp-configure)

## Preview Alexa Voice Service \(AVS\) Integration for AWS IoT with a preconfigured NXP account<a name="iot-mqtt-alexa-gs-nxp-preview"></a>

### Prerequisites<a name="iot-mqtt-alexa-gs-nxp-preview-prereqs"></a>

To follow these steps, you need the following resources\.
+ [NXP i\.MX RT106A development kit](https://www.nxp.com/design/designs/mcu-based-solution-for-br-alexa-voice-service:MCU-VOICE-CONTROL-AVS)

  This kit is preloaded with software that enables both [zero touch setup](#iot-mqtt-alexa-gs-nxp-zero-touch-setup) and [user\-guided setup](#iot-mqtt-alexa-gs-nxp-user-guided-setup)\.
+ A Mac, Windows 7 or 10, or Linux computer
+ An Android or iOS mobile device
+ The [Amazon Alexa app for iOS](https://apps.apple.com/us/app/amazon-alexa/id944011620) or the [Amazon Alexa app for Android](https://play.google.com/store/apps/details?id=com.amazon.dee.app&hl=en_US&gl=US)
+ An [Amazon Alexa account](https://alexa.amazon.com)

### Turn on the development kit<a name="iot-mqtt-alexa-gs-nxp-poweron-devkit"></a>

Verify that your development kit box contains a USB Type\-C to dual Type\-A cable\. Connect both of the USB\-A connections to your computer\. Connect the USB\-C connector to your kit\. Your configuration will look like the following image\.

![\[Plug the two USB-A connections into your computer. Plug the single USB-A connection to your device kit.\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/iot-nxp-power.png)

**Note**  
If the box contains a quick start card, disregard it and refer to these instructions instead\.

When the board has power, the status indicator LED lights up and displays various colors\. These are status indicators for the various stages of the boot process\. The colors and the blink rate indicate the status of the device\. Your device is ready for setup when the status indicator light turns solid blue, as shown in the following image\.

![\[The status indicator light displays solid blue.\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/iot-nxp-on.png)

The development kit supports the following setups, depending on your environment\.
+ [User\-guided setup](#iot-mqtt-alexa-gs-nxp-user-guided-setup): Use this setup when the device arrives in factory state and doesn't meet the conditions for zero touch setup \(ZTS\)\.

  You also use user\-guided setup when someone has already performed ZTS on the device\. ZTS can occur only once in the lifetime of a product\.
+  [Zero touch setup \(ZTS\)](#iot-mqtt-alexa-gs-nxp-zero-touch-setup): Use this setup when your environment meets the following conditions\.
  + You purchased the kit from Amazon\.com\.
  + You didn't purchase the kit or receive the kit as a gift\.
  + You've already installed a provisioner device in the Wi\-Fi network that you're using with the kit\.

    A provisioner device is an Amazon device \(such as an Echo \(3rd Gen\)\) that is registered to an Amazon customer account\.

    For a list of Amazon devices that qualify as provisioning devices, see [Testing Your Device](https://developer.amazon.com/docs/frustration-free-setup/understanding-ffs.html#testing-your-device) in [Understanding Frustration\-Free Setup](https://developer.amazon.com/docs/frustration-free-setup/understanding-ffs.html)\.
  + Your kit is within the Bluetooth Low Energy \(BLE\) range of the provisioner device\.
  + Your Wi\-Fi credentials are available in the Amazon Wi\-Fi locker\.
  + You have an [Alexa skill](https://developer.amazon.com/en-US/alexa/alexa-skills-kit) linked to your Amazon account\.
  + You have implemented [Login with Amazon](https://developer.amazon.com/apps-and-games/login-with-amazon)\.

  For more information about this kind of setup, see [Zero Touch Setup](https://developer.amazon.com/docs/frustration-free-setup/understanding-ffs.html#zero-touch-setup)\.

### User\-guided setup<a name="iot-mqtt-alexa-gs-nxp-user-guided-setup"></a>

When a kit that doesn't meet the requirements for ZTS turns on, it waits for user\-guided setup to occur through the Amazon Alexa app on your phone\. Make sure that the Amazon Alexa app is installed on your phone and that the Bluetooth and location permissions are enabled for the app\.

The following procedure describes how to perform user\-guided setup\.

1. Open the Alexa App and log in to your Amazon Alexa account\. The app detects that a nearby device is waiting for user\-guided setup and displays the page in the following image\. Choose **Continue**\.  
![\[The Amazon Alexa app displays a window that contains a Continue button.\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/iot-nxp-alexa-app.png)

   If you choose **Later** or if the app doesn't display this page, use the following steps to start user\-guided setup\.

   1. Choose the **Devices** tab, and then select the plus sign \(**\+**\) in the window that appears\.

   1. Choose **Add Device**\.

   1. Choose **Development Device**\.

   1. On the **What brand is your development device?** page, choose **NXP**, and then choose **Next**\.

      The following images show how the prompts described in these steps appear in the app\.  
![\[The Amazon Alexa app displays prompts that enable you to add an NXP device to your environment.\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/iot-nxp-alexa-menu.png)

   When the app connects to the device, the status indicator light blinks orange, as in the following images\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/iot/latest/developerguide/avs-integration-aws-iot-gs-nxp.html)
**Note**  
If user guided setup is interrupted \(for example, if you close the app\), the device returns to discovery mode, and the status indicator light displays solid blue\.

1. The app asks the kit to scan the environment for Wi\-Fi networks and return a list of networks that it detects\. Choose the network to which the device should connect\. The following image shows how this list appears in the app\.  
![\[The app displays a list of available Wi-Fi networks.\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/iot-nxp-wifi.png)
**Note**  
If you've already saved the selected network selected in your Amazon account, you don't need to enter the Wi\-Fi password\.

   When you select the Wi\-Fi network, the screen displays the following message as Wi\-Fi provisioning and communication with the setup servers takes place: **Connecting your NXP development device to *Wi\-Fi Network Name***\. The following image shows how this screen appears in the app\.  
![\[The app displays a message indicating that it is connecting to the Wi-Fi network that you selected.\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/iot-nxp-wifi-connect.png)

   The status indicator light continues to blink orange until the kit's registration is complete\. When registration is complete, the device says, "Your Alexa device is ready\." The kit then reboots\.

The following describes the steps that the kit takes after it reboots and reconnects to the Wi\-Fi network that you selected\.

1. As it reboots, the kit again displays various colors and alternates between blinks and solid colors as it progresses through the boot process\.

1. The device then tries to reconnect to the Wi\-Fi network that you selected\. As it does this, the status indicator light blinks yellow at 500 millisecond \(ms\) intervals\. After it connects to the Wi\-Fi network, it blinks yellow faster, at 250 ms intervals\. The following images show how this blinking appears in the kit\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/iot/latest/developerguide/avs-integration-aws-iot-gs-nxp.html)

1. The kit connects to AWS IoT\. While it connects, the status indicator light blinks green at 500 ms intervals\. When the kit is done connecting, the status indicator light blinks green at 250 ms intervals\. The following images show how this blinking appears in the kit\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/iot/latest/developerguide/avs-integration-aws-iot-gs-nxp.html)

1. The kit plays a chime sound that indicates that you can use it to interact with Alexa\.

When the kit connects to AWS IoT, the screen in the following image appears in the app\.

![\[When the kit connects to AWS IoT, the app displays a screen that contains the following message: NXP light connected.\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/iot-nxp-wifi-done.png)

The **NXP light connected** message appears in the app because the kit implements the smart home capabilities for an NXP light device\.

### Zero touch setup \(ZTS\)<a name="iot-mqtt-alexa-gs-nxp-zero-touch-setup"></a>

If your environment fulfills all of the prerequisites for ZTS, the provisioning device discovers your kit and starts ZTS setup when you turn the kit on\. The Amazon Alexa app is also a ZTS provisioner, so opening the Amazon Alexa app can also start ZTS setup\.

As the provisioning process continues, the status indicator light state follows the same patterns as the ones described in the user\-guided setup section\. During provisioning, log messages are sent to the SLN\-ALEXA\-IOT console over its virtual COM port\. When provisioning is complete, the kit plays the chime sound that indicates that you can use it to interact with Alexa\.

**Note**  
ZTS setup can occur only once in the lifetime of a device, even if you return it to factory settings\.

## Interact with Alexa<a name="iot-mqtt-alexa-gs-nxp-alexa-interact"></a>

You can start using the kit to interact with Alexa by asking it a question\. Even a simple question, such as "Alexa, how is the weather?" goes through several states as Alexa processes and responds to it\.

You see the first indication that the kit is listening when you speak the Alexa wake word\. When the kit detects this word, the kit starts listening and sending information from the microphone to AVS through AWS IoT\. The status indicator light displays solid cyan, as in the following image\.

![\[When the kit detects the wake word, the status indicator light displays solid cyan.\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/iot-nxp-cyan.png)

When the device finishes sending information from the microphone to AVS through AWS IoT, the device stops listening and switches to a thinking state\. This state indicates that AVS is processing the question and is determining the best response\. While the kit is in this state, the status indicator LED blinks cyan and blue at 200 ms intervals\. The following images show how this blinking appears in the kit\.


|  |  | 
| --- |--- |
|  ![\[The kit's status indicator light blinks cyan and blue as AVS processes the question.\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/iot-nxp-blue-cyan-1.png)  |  ![\[The kit's status indicator light blinks cyan and blue as AVS processes the question.\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/iot-nxp-blue-cyan-2.png)  | 

When the device finishes thinking, it starts to respond\. Before the kit begins to speak, the status indicator light switches to a speaking state\. The kit blinks cyan and blue at 500 ms intervals\.

The response from Alexa plays out of the kit's speaker while the status indicator light blinks cyan and blue, Alexa describes weather conditions based on the location of your Alexa consumer account\. When the response is complete, the status indicator light stops blinking and turns off\. This indicates that the kit it is in an idle state and waiting for the Alexa wake word\.

## Use your AWS and Alexa Voice Service developer accounts to set up AVS for AWS IoT<a name="iot-mqtt-alexa-gs-nxp-configure"></a>

The preconfigured NXP account is only for evaluating the kit\. When you use your own account, you get the following benefits\.
+ Full control of over the air \(OTA\) jobs and deployments, such as remote firmware updates\.
+ Control over AWS services\.
+ Customization of smart home skills\.

To migrate from the preconfigured NXP account to your own account, download the **MCU Alexa Voice Solution Migration Guide** from the **Getting Started** section of the [EdgeReady MCU Based Solution for Alexa for IOT](https://www.nxp.com/design/designs/edgeready-mcu-based-solution-for-alexa-for-iot:MCU-VOICE-CONTROL-AVS?&&tid=vanmcu-avs) page\. Follow the steps in this guide\.

**Note**  
To download this file, you need an NXP account\.