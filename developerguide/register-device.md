# Register a Device in the Registry<a name="register-device"></a>

Devices connected to AWS IoT are represented by *IoT things* in the AWS IoT registry\. The registry allows you to keep a record of all of the devices that are registered to your AWS IoT account\.

**To register your device in the registry:**

1. On the **Welcome to the AWS IoT Console** page, in the navigation pane, choose **Manage**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/first-visit.png)

1. On the **You don't have any things yet** page, choose **Register a thing**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/no-things-yet.png)

1. On the **Creating AWS IoT things** page, choose **Create a single thing**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/creating-things.png)

1. On the **Create a thing** page, in the **Name** field, type a name for your thing, such as **MyIotThing**\. Choose **Next**\.
**Note**  
We do not recommend using personally identifiable information in your thing name\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/add-device-thing-registry.png)

1. On the **Add a certificate for your thing** page, choose **Create certificate**\. This generates an X\.509 certificate and key pair\.  
![\[Add a certificate for your thing\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/sdk-create-cert.png)

1. On the **Certificate created\!** page, download your public and private keys, certificate, and root certificate authority \(CA\):

   1. Choose **Download** for your certificate\.

   1. Choose **Download** for your private key\.

   1. Choose **Download** for the Amazon root CA\. This will display a new web page\. Choose **RSA 2048 bit key: Amazon Root CA 1**\. This opens another web page with the text of the root CA certificate\. Copy this text and paste it into a file named `Amazon_Root_CA_1.pem`\.

   Most web browsers save downloaded files into a Downloads directory\. You will copy these files to a different directory when you run the sample applications\. Choose **Activate** to activate the X\.509 certificate, and then choose **Attach a policy**\.  
![\[Certificate created!\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/sdk-attach-policy.png)

1. On the **Add a policy for your thing** page, choose **Register Thing**\.

   After you register your thing, you will need to create and attach a new policy to the certificate\.  
![\[Register thing\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/add-policy-for-thing.png)

1. On the AWS IoT console, in the navigation pane, choose **Secure** and **Policies**\.

   Choose **Create**\.  
![\[Policies\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/create-first-policy.png)

1. On the **Create a policy** page:

   1. Enter a **Name** for the policy, such as **MyIotPolicy**\.

   1. For **Action**, enter **iot:\***\. For **Resource ARN**, enter **\***\.

   1. Under **Effect**, choose **Allow**, and then choose **Create**\.

      This policy allows your device to perform all AWS IoT actions on all AWS IoT resources\.
**Important**  
These settings are overly permissive\. In a production environment narrow the scope of the permissions to that which are required by your device\. For more information, see [Authorization](authorization.md)\.  
![\[Create policy\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/gs-create-policy.png)

1. Choose **Manage** and then choose your AWS IoT thing\.  
![\[Select your thing\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/gs-thing-detail.png)

1. Choose **Security**  
![\[Select security\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/gs-thing-security.png)

1. Choose your certificate\.

1. In the certificate detail page, choose **Actions** and then **Attach policy**\.  
![\[Attach policy\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/gs-attach-policy.png)

1. Choose the policy you created \(MyIotPolicy\) and choose **Attach**\.  
![\[Attach policy\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/gs-attach-policy-2.png)