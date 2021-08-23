# Create a virtual device with Amazon EC2<a name="creating-a-virtual-thing"></a>

In this tutorial, you'll create an Amazon EC2 instance to serve as your virtual device in the cloud\.

To complete this tutorial, you need an AWS account\. If you don't have one, complete the steps described in [Set up your AWS account](setting-up.md) before you continue\. 

**Topics**
+ [Set up an Amazon EC2 instance](#set-up-ec2)
+ [Install Git, Node\.js and configure the AWS CLI](#install-git-node)
+ [Create AWS IoT resources for your virtual device](#ec2-create-certificate)
+ [Install the AWS IoT Device SDK for JavaScript](#ec2-sdk)
+ [Run the sample application](#ec2-run-app)
+ [View messages from the sample app in the AWS IoT console](#ec2-view-msg)

## Set up an Amazon EC2 instance<a name="set-up-ec2"></a>

The following steps show you how to create an Amazon EC2 instance that will act as your virtual device in place of a physical device\.

**To launch an instance**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. From the console dashboard, choose **Launch Instance**\.

1. The **Step 1: Choose an Amazon Machine Image \(AMI\)** page displays a list of basic configurations, called *Amazon Machine Images \(AMIs\)*, which serve as templates for your instance\. Select an HVM version of Amazon Linux 2, such as *Amazon Linux 2 AMI \(HVM\), SSD Volume Type*\. Notice that this AMI is marked "Free tier eligible\."

1. On the **Choose an Instance Type** page, you can select the hardware configuration of your instance\. Select the `t2.micro` type, which is selected by default\. Notice that this instance type is eligible for the free tier\.

1. Choose **Review and Launch** to let the wizard complete the other configuration settings for you\.

1. On the **Review Instance Launch** page, choose **Launch**\.

1. When prompted for a key pair, select **Create a new key pair**, enter a name for the key pair, and then choose **Download Key Pair**\. *This is your only chance to save the private key file, so be sure to download it\.* Save the private key file in a safe place\. You'll need to provide the name of your key pair when you launch an instance and the corresponding private key each time you connect to the instance\.
**Warning**  
Don't select the **Proceed without a key pair** option\. If you launch your instance without a key pair, then you can't connect to it\.

   When you are ready, choose **Launch Instances**\. 

1. A confirmation page lets you know that your instance is launching\. Choose **View Instances** to close the confirmation page and return to the console\.

1. On the **Instances** screen, you can view the status of the launch\. It takes a short time for an instance to launch\. When you launch an instance, its initial state is `pending`\. After the instance starts, its state changes to `running` and it receives a public DNS name\. \(If the **Public DNS \(IPv4\)** column is hidden, choose **Show/Hide Columns** \(the gear\-shaped icon\) in the top right corner of the page and then select **Public DNS \(IPv4\)**\.\)

1. It can take a few minutes for the instance to be ready so that you can connect to it\. Check that your instance has passed its status checks; you can view this information in the **Status Checks** column\.

   After your new instance has passed its status checks, continue to the next procedure and connect to it\.

**To connect to your instance**

You can connect to an instance using the browser\-based client by selecting the instance from the Amazon EC2 console and choosing to connect using Amazon EC2 Instance Connect\. Instance Connect handles the permissions and provides a successful connection\.

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. In the left menu, choose **Instances**\.

1. Select the instance and choose **Connect**\.

1. Choose **Amazon EC2 Instance Connect \(browser\-based SSH connection\)**, **Connect**\.

You should now have an **Amazon EC2 Instance Connect** window that is logged into your new Amazon EC2 instance\.

## Install Git, Node\.js and configure the AWS CLI<a name="install-git-node"></a>

In this section, you'll install Git and Node\.js, on your Linux instance\.

**To install Git**

1. In your **Amazon EC2 Instance Connect** window, update your instance by using the following command\.

   ```
   sudo yum update -y
   ```

1. In your **Amazon EC2 Instance Connect** window, install Git by using the following command\.

   ```
   sudo yum install git -y
   ```

**To install Node\.js**

1. In your **Amazon EC2 Instance Connect** window, install node version manager \(nvm\) by using the following command\.

   ```
   curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.34.0/install.sh | bash
   ```

   We will use nvm to install Node\.js because nvm can install multiple versions of Node\.js and allow you to switch between them\. 

1. In your **Amazon EC2 Instance Connect** window, activate nvm by using this command\.

   ```
   . ~/.nvm/nvm.sh
   ```

1. In your **Amazon EC2 Instance Connect** window, use nvm to install the latest version of Node\.js by using this command\.

   ```
   nvm install node
   ```

   Installing Node\.js also installs the Node Package Manager \(npm\) so you can install additional modules as needed\.

1. In your **Amazon EC2 Instance Connect** window, test that Node\.js is installed and running correctly by using this command\.

   ```
   node -v
   ```

    This tutorial requires Node v10\.0 or later\.

**To configure AWS CLI**

Your Amazon EC2 instance comes preloaded with the AWS CLI\. However, you must complete your AWS CLI profile\. For more information on how to configure your CLI, see [Configuring the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-configure.html)\.

1. The following example shows sample values\. Replace them with your own values\. You can find these values in your AWS console in your account info under **My Security Credentials**\.

   In your **Amazon EC2 Instance Connect** window, enter this command:

   ```
   aws configure
   ```

   Then enter the values from your account at the prompts displayed\.

   ```
   AWS Access Key ID [None]: AKIAIOSFODNN7EXAMPLE
   AWS Secret Access Key [None]: wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
   Default region name [None]: us-west-2
   Default output format [None]: json
   ```

1. You can test your AWS CLI configuration with this command:

   ```
   aws iot describe-endpoint --endpoint-type iot:Data-ATS
   ```

   If your AWS CLI is configured correctly, the command should return an endpoint address from your AWS account\.

## Create AWS IoT resources for your virtual device<a name="ec2-create-certificate"></a>

This section describes how to use the AWS CLI to create the thing object and its certificate files directly on the virtual device\. This is done directly on the device to avoid the potential complication that might arise from copying them to the device from another computer\.

**To create an AWS IoT thing object in your Linux instance**

Devices connected to AWS IoT are represented by *thing objects* in the AWS IoT registry\. A *thing object* represents a specific device or logical entity\. In this case, your *thing object* will represent your virtual device, this Amazon EC2 instance\.

1. In your **Amazon EC2 Instance Connect** window, run the following command to create your thing object\.

   ```
   aws iot create-thing --thing-name "MyIotThing"
   ```

1. The JSON response should look like this:

   ```
   {
       "thingArn": "arn:aws:iot:your-region:your-aws-account:thing/MyIotThing", 
       "thingName": "MyIotThing",
       "thingId": "6cf922a8-d8ea-4136-f3401EXAMPLE"
   }
   ```

**To create and attach AWS IoT keys and certificates in your Linux instance**

The [create\-keys\-and\-certificate](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/iot/create-keys-and-certificate.html) command creates client certificates signed by the Amazon Root certificate authority\. This certificate is used to authenticate the identity of your virtual device\.

1. In your **Amazon EC2 Instance Connect** window, create a directory to store your certificate and key files\.

   ```
   mkdir ~/certs
   ```

1. In your **Amazon EC2 Instance Connect** window, download a copy of the Amazon certificate authority \(CA\) certificate by using this command\.

   ```
   curl -o ~/certs/Amazon-root-CA-1.pem \
       https://www.amazontrust.com/repository/AmazonRootCA1.pem
   ```

1. In your **Amazon EC2 Instance Connect** window, run the following command to create your private key, public key, and X\.509 certificate files\. This command also registers and activates the certificate with AWS IoT\.

   ```
   aws iot create-keys-and-certificate \
       --set-as-active \
       --certificate-pem-outfile "~/certs/device.pem.crt" \
       --public-key-outfile "~/certs/public.pem.key" \
       --private-key-outfile "~/certs/private.pem.key"
   ```

   The response looks like the following\. Save the `certificateArn` so that you can use it in subsequent commands\. You'll need it to attach your certificate to your thing and to attach the policy to the certificate in a later steps\.

   ```
   {
       "certificateArn": "arn:aws:iot:us-west-2:123456789012:cert/9894ba17925e663f1d29c23af4582b8e3b7619c31f3fbd93adcb51ae54b83dc2",
       "certificateId": "9894ba17925e663f1d29c23af4582b8e3b7619c31f3fbd93adcb51ae54b83dc2",
       "certificatePem": "
   -----BEGIN CERTIFICATE-----
   MIICiTCCEXAMPLE6m7oRw0uXOjANBgkqhkiG9w0BAQUFADCBiDELMAkGA1UEBhMC
   VVMxCzAJBgNVBAgEXAMPLEAwDgYDVQQHEwdTZWF0dGxlMQ8wDQYDVQQKEwZBbWF6
   b24xFDASBgNVBAsTC0lBTSEXAMPLE2xlMRIwEAYDVQQDEwlUZXN0Q2lsYWMxHzAd
   BgkqhkiG9w0BCQEWEG5vb25lQGFtYEXAMPLEb20wHhcNMTEwNDI1MjA0NTIxWhcN
   MTIwNDI0MjA0NTIxWjCBiDELMAkGA1UEBhMCEXAMPLEJBgNVBAgTAldBMRAwDgYD
   VQQHEwdTZWF0dGxlMQ8wDQYDVQQKEwZBbWF6b24xFDAEXAMPLEsTC0lBTSBDb25z
   b2xlMRIwEAYDVQQDEwlUZXN0Q2lsYWMxHzAdBgkqhkiG9w0BCQEXAMPLE25lQGFt
   YXpvbi5jb20wgZ8wDQYJKoZIhvcNAQEBBQADgY0AMIGJAoGBAMaK0dn+aEXAMPLE
   EXAMPLEfEvySWtC2XADZ4nB+BLYgVIk60CpiwsZ3G93vUEIO3IyNoH/f0wYK8m9T
   rDHudUZEXAMPLELG5M43q7Wgc/MbQITxOUSQv7c7ugFFDzQGBzZswY6786m86gpE
   Ibb3OhjZnzcvQAEXAMPLEWIMm2nrAgMBAAEwDQYJKoZIhvcNAQEFBQADgYEAtCu4
   nUhVVxYUntneD9+h8Mg9qEXAMPLEyExzyLwaxlAoo7TJHidbtS4J5iNmZgXL0Fkb
   FFBjvSfpJIlJ00zbhNYS5f6GuoEDEXAMPLEBHjJnyp378OD8uTs7fLvjx79LjSTb
   NYiytVbZPQUQ5Yaxu2jXnimvw3rrszlaEXAMPLE=
   -----END CERTIFICATE-----\n",
       "keyPair": {
           "PublicKey": "-----BEGIN PUBLIC KEY-----\nMIIBIjANBgkqhkEXAMPLEQEFAAOCAQ8AMIIBCgKCAQEAEXAMPLE1nnyJwKSMHw4h\nMMEXAMPLEuuN/dMAS3fyce8DW/4+EXAMPLEyjmoF/YVF/gHr99VEEXAMPLE5VF13\n59VK7cEXAMPLE67GK+y+jikqXOgHh/xJTwo+sGpWEXAMPLEDz18xOd2ka4tCzuWEXAMPLEahJbYkCPUBSU8opVkR7qkEXAMPLE1DR6sx2HocliOOLtu6Fkw91swQWEXAMPLE\GB3ZPrNh0PzQYvjUStZeccyNCx2EXAMPLEvp9mQOUXP6plfgxwKRX2fEXAMPLEDa\nhJLXkX3rHU2xbxJSq7D+XEXAMPLEcw+LyFhI5mgFRl88eGdsAEXAMPLElnI9EesG\nFQIDAQAB\n-----END PUBLIC KEY-----\n",
           "PrivateKey": "-----BEGIN RSA PRIVATE KEY-----\nkey omitted for security reasons\n-----END RSA PRIVATE KEY-----\n"
       }
   }
   ```

1. In your **Amazon EC2 Instance Connect** window, attach your thing object to the certificate you just created by using the following command and the *certificateArn* in the response from the previous command\.

   ```
   aws iot attach-thing-principal \
       --thing-name "MyIotThing" \
       --principal "certificateArn"
   ```

   If successful, this command does not display any output\.

**To create and attach a policy**

1. In your **Amazon EC2 Instance Connect** window, create the policy file by copying and pasting this policy document to a file named **\~/policy\.json**\.

   If you don't have a favorite Linux editor, you can open nano, by using this command\.

   ```
   nano ~/policy.json
   ```

   And pasting the policy document for `policy.json` into it\. Enter ctrl\-x to exit the nano editor and save the file\.

   Contents of the policy document for `policy.json`\.

   ```
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Effect": "Allow",
               "Action": [
                   "iot:Publish",
                   "iot:Subscribe",
                   "iot:Receive",
                   "iot:Connect"
               ],
               "Resource": [
                   "*"
               ]
           }
       ]
   }
   ```

1. In your **Amazon EC2 Instance Connect** window, create your policy by using the following command\.

   ```
   aws iot create-policy \
       --policy-name "MyIotThingPolicy" \
       --policy-document "file://~/policy.json"
   ```

   Output:

   ```
   {
       "policyName": "MyIotThingPolicy",
       "policyArn": "arn:aws:iot:your-region:your-aws-account:policy/MyIotThingPolicy",
       "policyDocument": "{
           \"Version\": \"2012-10-17\",
           \"Statement\": [
               {
                   \"Effect\": \"Allow\",
                   \"Action\": [
                       \"iot:Publish\",
                       \"iot:Receive\",
                       \"iot:Subscribe\",
                       \"iot:Connect\"
                   ],
                   \"Resource\": [
                       \"*\"
                   ]
               }
           ]
       }",
       "policyVersionId": "1"
   }
   ```

1. In your **Amazon EC2 Instance Connect** window, attach the policy to your virtual device's certificate by using the following command\.

   ```
   aws iot attach-policy \
       --policy-name "MyIotThingPolicy" \
       --target "certificateArn"
   ```

   If successful, this command does not display any output\.

At this point, you have created for your virtual device: 
+ A thing object to represent your virtual device in AWS IoT\.
+ A certificate to authenticate your virtual device\.
+ A policy document to authorize your virtual device to Connect to AWS IoT, and to Publish, Receive, and Subscribe to messages\.

## Install the AWS IoT Device SDK for JavaScript<a name="ec2-sdk"></a>

In this section, you'll install the AWS IoT Device SDK for JavaScript, which contains the code that applications can use to communicate with AWS IoT and the sample programs\.

**To install the AWS IoT Device SDK for JavaScript on your Linux instance**

1. In your **Amazon EC2 Instance Connect** window, clone the AWS IoT Device SDK for JavaScript repository into the `aws-iot-device-sdk-js-v2` directory of your home directory by using this command\.

   ```
   cd ~
   git clone https://github.com/aws/aws-iot-device-sdk-js-v2.git
   ```

1. Navigate to the `aws-iot-device-sdk-js-v2` directory that you created in the preceding step\.

   ```
   cd aws-iot-device-sdk-js-v2
   ```

1. Use npm to install the SDK\.

   ```
   npm install
   ```

## Run the sample application<a name="ec2-run-app"></a>

 The commands in the next sections assume that your key and certificate files are stored on your virtual device as shown in this table\.


**Certificate file names**  

|  File  |  File path  | 
| --- | --- | 
|  Private key  |  `~/certs/private.pem.key`  | 
|  Device certificate  |  `~/certs/device.pem.crt`  | 
|  Root CA certificate  |  `~/certs/Amazon-root-CA-1.pem`  | 

In this section, you'll install and run the `pub-sub.js` sample app found in the `aws-iot-device-sdk-js-v2/samples/node` directory of the AWS IoT Device SDK for JavaScript\. This app shows how a device, your Amazon EC2 instance, uses the MQTT library to publish and subscribe to MQTT messages\. The `pub-sub.js` sample app subscribes to a topic, `topic_1`, publishes 10 messages to that topic, and displays the messages as they're received from the message broker\.

**To install and run the sample app**

1. In your **Amazon EC2 Instance Connect** window, navigate to the `aws-iot-device-sdk-js-v2/samples/node/pub_sub` directory that the SDK created and install the sample app by using these commands\.

   ```
   cd ~/aws-iot-device-sdk-js-v2/samples/node/pub_sub
   npm install
   ```

1. In your **Amazon EC2 Instance Connect** window, get *your\-iot\-endpoint* from AWS IoT by using this command\.

   ```
   aws iot describe-endpoint --endpoint-type iot:Data-ATS
   ```

1. In your **Amazon EC2 Instance Connect** window, insert *your\-iot\-endpoint* as indicated and run this command\.

   ```
   node dist/index.js --topic topic_1 --root-ca ~/certs/Amazon-root-CA-1.pem --cert ~/certs/device.pem.crt --key ~/certs/private.pem.key --endpoint your-iot-endpoint
   ```

The sample app:

1. Connects to the AWS IoT service for your account\.

1. Subscribes to the message topic, **topic\_1**, and displays the messages it receives on that topic\.

1. Publishes 10 messages to the topic, **topic\_1**\.

1. Displays output similar to the following:

   ```
   Publish received on topic topic_1
   {"message":"Hello world!","sequence":1}
   Publish received on topic topic_1
   {"message":"Hello world!","sequence":2}
   Publish received on topic topic_1
   {"message":"Hello world!","sequence":3}
   Publish received on topic topic_1
   {"message":"Hello world!","sequence":4}
   Publish received on topic topic_1
   {"message":"Hello world!","sequence":5}
   Publish received on topic topic_1
   {"message":"Hello world!","sequence":6}
   Publish received on topic topic_1
   {"message":"Hello world!","sequence":7}
   Publish received on topic topic_1
   {"message":"Hello world!","sequence":8}
   Publish received on topic topic_1
   {"message":"Hello world!","sequence":9}
   Publish received on topic topic_1
   {"message":"Hello world!","sequence":10}
   ```

If you're having trouble running the sample app, review [Troubleshooting problems with the sample app](gs-device-troubleshoot.md)\.

You can also add the `--verbosity Debug` parameter to the command line so the sample app displays detailed messages about what it’s doing\. That information might provide you the help you need to correct the problem\. 

## View messages from the sample app in the AWS IoT console<a name="ec2-view-msg"></a>

You can see the sample app's messages as they pass through the message broker by using the **MQTT client** in the **AWS IoT console**\. 

**To view the MQTT messages published by the sample app**

1. Review [View MQTT messages with the AWS IoT MQTT client](view-mqtt-messages.md)\. This helps you learn how to use the **MQTT client** in the **AWS IoT console** to view MQTT messages as they pass through the message broker\.

1. Open the **MQTT client** in the **AWS IoT console**\.

1. Subscribe to the topic, **topic\_1**\.

1. In your **Amazon EC2 Instance Connect** window, run the sample app again and watch the messages in the **MQTT client** in the **AWS IoT console**\.

   ```
   cd ~/aws-iot-device-sdk-js-v2/samples/node/pub_sub
   node dist/index.js --topic topic_1 --root-ca ~/certs/Amazon-root-CA-1.pem --cert ~/certs/device.pem.crt --key ~/certs/private.pem.key --endpoint your-iot-endpoint
   ```