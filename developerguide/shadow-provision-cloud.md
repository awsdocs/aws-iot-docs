# Tutorial: Provisioning your device in AWS IoT<a name="shadow-provision-cloud"></a>

This section creates the AWS IoT Core resources that your tutorial will use\.

**Topics**
+ [Step 1: Create an AWS IoT policy for the Device Shadow](#create-policy-shadow)
+ [Step 2: Create a thing resource and attach the policy to the thing](#create-thing-shadow)
+ [Step 3: Review the results and next steps](#resources-shadow-review)

## Step 1: Create an AWS IoT policy for the Device Shadow<a name="create-policy-shadow"></a>

X\.509 certificates authenticate your device with AWS IoT Core\. AWS IoT policies are attached to the certificate that permits the device to perform AWS IoT operations, such as subscribing or publishing to MQTT reserved topics used by the Device Shadow service\. Your device presents its certificate when it connects and sends messages to AWS IoT Core\. 

In this procedure, you'll create a policy that allows your device to perform the AWS IoT operations necessary to run the example program\. We recommend that you create a policy that grants only the permissions required to perform the task\. You create the AWS IoT policy first, and then attach it to the device certificate that you'll create later\.

**To create an AWS IoT policy**

1. On the left menu, choose **Secure**, and then choose **Policies**\. If your account has existing policies, choose **Create**, otherwise, on the **You don’t have a policy yet** page, choose **Create a policy**\.

1. On the **Create a policy** page:

   1. Enter a name for the policy in the **Name** field \(for example, **My\_Device\_Shadow\_policy**\)\. Do not use personally identifiable information in your policy names\.

   1. In the policy document, you describe connect, subscribe, receive, and publish actions that give the device permission to publish and subscribe to the MQTT reserved topics\.

      Copy the following sample policy and paste it in your policy document\. Replace `thingname` with the name of the thing that you'll create \(for example, `My_light_bulb`\), `region` with the AWS IoT Region where you're using the services, and `account` with your AWS account number\. For more information about AWS IoT policies, see [AWS IoT Core policies](iot-policies.md)\.

      ```
      {
        "Version": "2012-10-17",
        "Statement": [
          {
            "Effect": "Allow",
            "Action": [
              "iot:Publish"
            ],
            "Resource": [
              "arn:aws:iot:region:account:topic/$aws/things/thingname/shadow/get",
              "arn:aws:iot:region:account:topic/$aws/things/thingname/shadow/update"
            ]
          },
          {
            "Effect": "Allow",
            "Action": [
              "iot:Receive"
            ],
            "Resource": [
              "arn:aws:iot:region:account:topic/$aws/things/thingname/shadow/get/accepted",
              "arn:aws:iot:region:account:topic/$aws/things/thingname/shadow/get/rejected",
              "arn:aws:iot:region:account:topic/$aws/things/thingname/shadow/update/accepted",
              "arn:aws:iot:region:account:topic/$aws/things/thingname/shadow/update/rejected",
              "arn:aws:iot:region:account:topic/$aws/things/thingname/shadow/update/delta"
            ]
          },
          {
            "Effect": "Allow",
            "Action": [
              "iot:Subscribe"
            ],
            "Resource": [
              "arn:aws:iot:region:account:topicfilter/$aws/things/thingname/shadow/get/accepted",
              "arn:aws:iot:region:account:topicfilter/$aws/things/thingname/shadow/get/rejected",
              "arn:aws:iot:region:account:topicfilter/$aws/things/thingname/shadow/update/accepted",
              "arn:aws:iot:region:account:topicfilter/$aws/things/thingname/shadow/update/rejected",
              "arn:aws:iot:region:account:topicfilter/$aws/things/thingname/shadow/update/delta"
            ]
          },
          {
            "Effect": "Allow",
            "Action": "iot:Connect",
            "Resource": "arn:aws:iot:region:account:client/test-*"
          }
        ]
      }
      ```

## Step 2: Create a thing resource and attach the policy to the thing<a name="create-thing-shadow"></a>

Devices connected to AWS IoT can be represented by *thing resources* in the AWS IoT registry\. A *thing resource* represents a specific device or logical entity, such as the light bulb in this tutorial\.

To learn how to create a thing in AWS IoT, follow the steps described in [Create a thing object](create-iot-resources.md#create-aws-thing)\. Here are some key things to note as you follow the steps in that tutorial:

1. Choose **Create a single thing**, and in the **Name** field, enter a name for the thing that is the same as the `thingname` \(for example, `My_light_bulb`\) you specified when you created the policy earlier\.

   You can't change a thing name after it has been created\. If you gave it a different name other than `thingname`, create a new thing with name as `thingname` and delete the old thing\.
**Note**  
Do not use personally identifiable information in your thing name\. The thing name can appear in unencrypted communications and reports\.

1. We recommend that you download each of the certificate files on the **Certificate created\!** page into a location where you can easily find them\. You'll need to install these files for running the sample application\.

   We recommend that you download the files into a `certs` subdirectory in your `home` directory on the Raspberry Pi and name each of them with a simpler name as suggested in the following table\.  
**Certificate file names**    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/iot/latest/developerguide/shadow-provision-cloud.html)

1. After you activate the certificate to enable connections to AWS IoT, choose **Attach a policy** and make sure you attach the policy that you created earlier \(for example, **My\_Device\_Shadow\_policy**\) to the thing\.

   After you've created a thing, you can see your thing resource displayed in the list of things in the AWS IoT console\.

## Step 3: Review the results and next steps<a name="resources-shadow-review"></a>

**In this tutorial, you learned how to:**
+ Set up and configure the Raspberry Pi device\.
+ Create an AWS IoT policy document that authorizes your device to interact with AWS IoT services\.
+ Create a thing resource and associated X\.509 device certificate, and attach the policy document to it\.

**Next steps**  
You can now install the AWS IoT device SDK for Python, run the `shadow.py` sample application, and use Device Shadows to control the state\. For more information about how to run this tutorial, see [Tutorial: Installing the Device SDK and running the sample application for Device Shadows](lightbulb-shadow-application.md)\.