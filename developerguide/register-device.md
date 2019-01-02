# Register a Device in the Registry<a name="register-device"></a>

Devices connected to AWS IoT are represented by *things* in the registry\. The registry allows you to keep a record of all of the devices that are connected to your AWS IoT account\.

The fastest way to start using your AWS IoT Button is to download the mobile app for iOS or Android\. The mobile app creates the required AWS IoT resources for you, and adds an event source to your button that uses a Lambda blueprint to invoke a new AWS Lambda function of your choice\. Blueprints are preconfigured Lambda functions that allow you to quickly connect the click of a button to the functions that fit you best, such as sending automated emails or text messages or deploying other AWS services\. You can download the mobile apps from [The Apple App Store](https://itunes.apple.com/us/app/aws-iot-button/id1178216626?mt=8) or [Google Play](https://play.google.com/store/apps/details?id=com.amazonaws.iotbutton)\.

If you are unable to use the mobile apps, follow these instructions\.

**To register your device in the registry:**

1. On the **Welcome to the AWS IoT Console** page, in the left navigation pane, choose **Manage** to expand the choices, and then choose **Things**\.   
![\[Image NOT FOUND\]](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/images/first-visit.png)

1. On the page that says **You don't have any things yet**, choose **Register a thing**\.  
![\[Image NOT FOUND\]](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/images/no-things-yet.png)

1. On the **Creating AWS IoT things** page, choose **Create a single thing**\.  
![\[Image NOT FOUND\]](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/images/creating-things.png)

1. On the **Create a thing** page, in the **Name** field, type a name for your device, such as **MyIoTButton**\. Choose **Next** to add your device to the registry\.  
![\[Image NOT FOUND\]](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/images/register-thing.png)![\[Image NOT FOUND\]](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/images/register-thing-next.png)