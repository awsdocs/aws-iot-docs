# Connecting Your Raspberry Pi to AWS IoT<a name="iot-sdk-setup"></a>

Follow these steps to connect your Raspberry Pi to the AWS IoT platform\.

## Prerequisites<a name="iot-sdk-prereqs"></a>

This tutorial requires the following hardware and software:

+ A [Raspberry Pi 2 Model B](https://www.raspberrypi.org/help/quick-start-guide/), or newer

+ [Raspbian Wheezy](http://archive.raspbian.org/raspbian/dists/) OS, or newer

+ Chrome \([Chromium](https://www.chromium.org/getting-involved/download-chromium)\) or Firefox \([Iceweasel](https://packages.debian.org/search?keywords=iceweasel)\) web browser

+ An internet connection

## Sign in to the AWS IoT Console<a name="iot-sdk-login-console"></a>

1. Turn on your Raspberry Pi, and confirm that it is connected to the internet\.

1. Sign in to the AWS Management Console, and then open the AWS IoT console at [https://aws.amazon.com/iot](https://aws.amazon.com/iot)\. On the **Welcome** page, choose **Get started**\.

   If you have already used the AWS IoT console, you automatically skip this step\.  
![\[AWS IoT get started\]](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/images/main-iot-page.png)

1. If this is the first time you are using the AWS IoT console, you see the **Welcome to the AWS IoT Console** page\. If it's not your first time using the the AWS IoT console, you see the **Monitor** page\. In the left navigation pane, choose **Manage**, and then choose **Things**\.  
![\[AWS IoT console welcome page\]](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/images/first-visit.png)

1. If you have not created any things yet, you see the **You don't have any things yet** page\. Choose **Register a thing**\.  
![\[Manage things\]](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/images/no-things-yet.png)

   If you have already created a thing, you see the **Things** page\. Choose **Create** at the top right of that page\.

## Create and Attach a Thing \(Device\)<a name="iot-sdk-create-thing"></a>

A *thing* represents a device whose status or data is stored in the AWS Cloud\. The device's status or data is stored in a JSON document known as the device's *shadow*\. The shadow is used to both store and retrieve state information\. The [Device Shadow service](iot-device-shadows.html) maintains a shadow for each device that is connected to AWS IoT\.

1. On the **Creating AWS IoT things** page, choose **Create a single thing**\.  
![\[Create a single thing\]](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/images/create-single-thing.png)

1. On the **Add your device to the thing registry** page, enter **MyRaspberryPi** for the device **Name**\. Leave the default values for all the other fields, and then choose **Next**\.  
![\[Add your device to the thing registry\]](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/images/register-thing-raspberry.png)

1. On the **Add a certificate for your thing** page, choose **Create certificate**\. This generates an X\.509 certificate and key pair\.  
![\[Add a certificate for your thing\]](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/images/create-cert-raspberry.png)

1. On your Raspberry Pi, create a working directory named `deviceSDK`\. This is where your certificate and key files will be stored\.

   On the **Certificate created\!** page, download your public and private keys, certificate, and root certificate authority \(CA\)\. Save them in the `deviceSDK` directory\. Choose **Activate** to activate the X\.509 certificate, and then choose **Attach a policy**\.  
![\[Certificate created!\]](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/images/certificate-created-raspberry.png)

1. On the **Add a policy for your thing** page, choose **Register Thing**\.

   After you register your thing, you will need to create and attach a new policy to the certificate\.  
![\[Register thing\]](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/images/add-policy-for-thing.png)

1. Navigate to the AWS IoT console\. In the left navigation pane, choose **Secure**, **Policies**\.

   If you haven't created an AWS IoT policy, you see the **You don't have any policies yet** page\. Choose **Create a policy**\.  
![\[Policies\]](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/images/create-first-policy.png)

   If you have already created a policy, you see the **Policies** page\. Choose **Create** at the top right of that page\.

1. On the **Create a policy** page, enter a **Name** for the policy\. For **Action**, enter **iot:\***\. For **Resource ARN**, enter **\***\. Under **Effect**, choose **Allow**, and then choose **Create**\. This policy allows your Raspberry Pi to publish messages to AWS IoT\.  
![\[Create policy\]](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/images/create-policy-raspberry.png)

1. In the AWS IoT console, choose **Manage**, **Things**\. On the **Things** page, choose **MyRaspberryPi**\.  
![\[Manage MyRaspberryPi\]](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/images/choose-thing-raspberry.png)

1. On the thing's **Details** page, in the left navigation pane, choose **Interact**\.  
![\[MyRaspberryPi details\]](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/images/thing-details-interact-raspberry.png)

1. Make a note of the REST API endpoint\. You will need it to connect to your device shadow\. In the navigation pane, choose **Security**\.  
![\[MyRaspberryPi interact\]](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/images/thing-rest-api-raspberry.png)

1. Choose the certificate that you created earlier\.  
![\[MyRaspberryPi security\]](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/images/choose-certificates-raspberry.png)

1. On the certificate's **Details** page, in **Actions**, choose **Attach policy**\.  
![\[Certificate details\]](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/images/attach-policy-to-cert-raspberry.png)

1. On the **Attach policies to certificate\(s\)** page, choose the policy you created, and then choose **Attach**\.  
![\[Attach policy to certificate\]](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/images/attach-policy-to-cert-raspberry-2.png)