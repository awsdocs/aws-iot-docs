# Step 2: Create the Thing<a name="iot-plant-step2"></a>

In this step, you create a *thing* in AWS IoT to represent the Raspberry Pi \(or your development computer as a device simulator\)\.

Devices connected to AWS IoT are represented by things in the AWS IoT registry\. The registry enables you to keep a record of all of the devices that are connected to your AWS account in AWS IoT \.

1. With the [ AWS IoT console](https://console.aws.amazon.com/iot/home) open, in the service navigation pane, choose **Manage**\.

1. If an **Introducing AWS IoT Device Management** dialog box is displayed, choose **Show me later**, or press **Esc**\.

1. In the service navigation pane, with **Manage** expanded, choose **Things**\.  
![\[Choose Things in the Manage section of the AWS IoT console .\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/console-manage-things.png)

1. If a **You don’t have any things yet** dialog box is displayed, choose **Register a thing**\. Otherwise, choose **Create**\.

1. On the **Creating AWS IoT things** page, for **Register a single AWS IoT thing**, choose **Create a single thing**\.  
![\[Choose Create a single thing on the Creating AWS IoT things page.\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/console-create-a-single-thing.png)

1. On the **Add your device to the device registry** page, provide a **Name** that represents your Raspberry Pi \(or your development computer as a device simulator\), for example, *MyRPi*\.
**Note**  
If you choose to use a different name, be sure to substitute it throughout this sample\.

1. Leave the rest of this page unchanged, and then choose **Next**\.

1. On the **Add a certificate for your thing** page, choose **Create certificate**\.  
![\[Choose Create certificate on the Add a certificate for your thing page.\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/console-create-certificate.png)

1. For **A certificate for this thing**, choose **Download**\. Then follow your web browser’s onscreen directions to save the file ending in `certificate.pem.crt.txt` to your local development computer\.
**Note**  
Although the dialog box shows a file ending in `cert.pem`, the file that you download ends in `certificate.pem.crt.txt`\.

1. Repeat the previous step in this section for **A public key**, **A private key**, and **A root CA for AWS IoT**\. Save the files ending in `public.pem.key`, `private.pem.key`, and `.pem`, respectively, to your development computer\.

   When you choose the **Download** link next to **A root CA for AWS IoT**, the [Server Authentication](managing-device-certs.md#server-authentication) section of the *AWS IoT Developer Guide* is displayed\. From there, to get the root CA for AWS IoT , click the Amazon Root CA 1 link in that section, which downloads the RSA 2048 bit key for the Amazon Trust Services endpoint\.
**Important**  
You can download the files for **A certificate for this thing** and **A root CA for AWS** at any time\. However, this is your only opportunity to download the files for **A public key** and **A private key for this thing**\.

1. Choose **Activate**\.  
![\[Activate highlighted on the Certificate created! page.\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/console-download-certificates.png)

1. Choose **Attach a policy**\.

1. For **Add a policy for your thing**, select **PlantWateringPolicy** \(**0 policies selected** changes to **1 policy selected\)**\. Then choose **Register Thing**\.

1. If an **Introducing AWS IoT Device Management** dialog box is displayed again, choose **Show me later**, or press **Esc**\.