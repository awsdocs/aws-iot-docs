# View MQTT messages with the AWS IoT MQTT client<a name="view-mqtt-messages"></a>

This section describes how to use the AWS IoT MQTT client in the [AWS IoT console](https://console.aws.amazon.com/iot/home) to watch the MQTT messages sent and received by AWS IoT\. The example used in this section relates to the examples used in [Getting started with AWS IoT Core](iot-gs.md); however, you can replace the *topicName* used in the examples with any [topic name or topic filter](topics.md) used by your IoT solution\. 

Devices publish MQTT messages that are identified by [topics](topics.md) to communicate their state to AWS IoT, and AWS IoT publishes MQTT messages to inform the devices and apps of changes and events\. You can use the MQTT client to subscribe to these topics and watch the messages as they occur\. You can also use the MQTT client to publish MQTT messages to subscribed devices and services in your AWS account\. 

## Viewing MQTT messages in the MQTT client<a name="view-mqtt-subscribe"></a>

**To view MQTT messages in the MQTT client**

1. In the [AWS IoT console](https://console.aws.amazon.com/iot/home), in the left menu, choose **Test**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/choose-test.png)

1. Subscribe to the *topicName* on which your device publishes\. In the getting started example, one *topicName* in use is **topic\_1**\. Continuing with the getting started example, on the **Subscribe** page, in the **Subscription topic** field, enter **topic\_1**, and then choose **Subscribe to topic**\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/subscribe-button-topic.png)

   The **Publish** page opens and the **topic\_1** topic appears in the **Subscriptions** column\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/subscribed-button-topic.png)

1. To continue with the getting started example, repeat the preceding two steps using the other *topicName*, **topic\_2** as the **Subscription topic** in place of **topic\_1**

1.  If the device that you configured in [ Configure your device  This section describes how to configure your device to connect to AWS IoT\. If you'd like to get started with AWS IoT but don't have a device yet, you can create a virtual device by using Amazon EC2 or you can use your Windows PC or Mac as an IoT device\. Select the best device option for you to try AWS IoT\. Of course, you can try all of them, but try only one at a time\. If you're not sure which device option is best for you, read about how to choose [which device option is the best](iot-gs-first-thing.md#choosing-a-gs-system), and then return to this page\.  Device options  [Create a virtual device with Amazon EC2](creating-a-virtual-thing.md)   [Use your Windows or Linux PC or Mac as an AWS IoT device](using-laptop-as-device.md)   [Connect a Raspberry Pi or another device](connecting-to-existing-device.md)    Create a virtual device with Amazon EC2  In this tutorial, you'll create an Amazon EC2 instance to serve as your virtual device in the cloud\. To complete this tutorial, you need an AWS account\. If you don't have one, complete the steps described in [Set up your AWS account](setting-up.md) before you continue\.  In this tutorial, you'll:  Set up an Amazon EC2 instance  The following steps show you how to create an Amazon EC2 instance that will act as your virtual device in place of a physical device\. To launch an instance Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.  From the console dashboard, choose **Launch Instance**\.   The **Step 1: Choose an Amazon Machine Image \(AMI\)** page displays a list of basic configurations, called *Amazon Machine Images \(AMIs\)*, which serve as templates for your instance\. Select an HVM version of Amazon Linux 2, such as *Amazon Linux 2 AMI \(HVM\), SSD Volume Type*\. Notice that this AMI is marked "Free tier eligible\."   On the **Choose an Instance Type** page, you can select the hardware configuration of your instance\. Select the `t2.micro` type, which is selected by default\. Notice that this instance type is eligible for the free tier\.   Choose **Review and Launch** to let the wizard complete the other configuration settings for you\.   On the **Review Instance Launch** page, choose **Launch**\.   When prompted for a key pair, select **Create a new key pair**, enter a name for the key pair, and then choose **Download Key Pair**\. *This is your only chance to save the private key file, so be sure to download it\.* Save the private key file in a safe place\. You'll need to provide the name of your key pair when you launch an instance and the corresponding private key each time you connect to the instance\.  Don't select the **Proceed without a key pair** option\. If you launch your instance without a key pair, then you can't connect to it\.  When you are ready, choose **Launch Instances**\.    A confirmation page lets you know that your instance is launching\. Choose **View Instances** to close the confirmation page and return to the console\.   On the **Instances** screen, you can view the status of the launch\. It takes a short time for an instance to launch\. When you launch an instance, its initial state is `pending`\. After the instance starts, its state changes to `running` and it receives a public DNS name\. \(If the **Public DNS \(IPv4\)** column is hidden, choose **Show/Hide Columns** \(the gear\-shaped icon\) in the top right corner of the page and then select **Public DNS \(IPv4\)**\.\)   It can take a few minutes for the instance to be ready so that you can connect to it\. Check that your instance has passed its status checks; you can view this information in the **Status Checks** column\. After your new instance has passed its status checks, continue to the next procedure and connect to it\.   To connect to your instance You can connect to an instance using the browser\-based client by selecting the instance from the Amazon EC2 console and choosing to connect using Amazon EC2 Instance Connect\. Instance Connect handles the permissions and provides a successful connection\. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.  In the left menu, choose **Instances**\.   Select the instance and choose **Connect**\.   Choose **Amazon EC2 Instance Connect \(browser\-based SSH connection\)**, **Connect**\.   You should now have an **Amazon EC2 Instance Connect** window that is logged into your new Amazon EC2 instance\.   Install Git, Node\.js and configure the AWS CLI  In this section, you'll install Git and Node\.js, on your Linux instance\. To install Git  In your **Amazon EC2 Instance Connect** window, update your instance by using the following command\. 

   ```
   sudo yum update -y
   ```   In your **Amazon EC2 Instance Connect** window, install Git by using the following command\. 

   ```
   sudo yum install git -y
   ```   To install Node\.js  In your **Amazon EC2 Instance Connect** window, install node version manager \(nvm\) by using the following command\. 

   ```
   curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.34.0/install.sh | bash
   ``` We will use nvm to install Node\.js because nvm can install multiple versions of Node\.js and allow you to switch between them\.    In your **Amazon EC2 Instance Connect** window, activate nvm by using this command\. 

   ```
   . ~/.nvm/nvm.sh
   ```   In your **Amazon EC2 Instance Connect** window, use nvm to install the latest version of Node\.js by using this command\. 

   ```
   nvm install node
   ``` Installing Node\.js also installs the Node Package Manager \(npm\) so you can install additional modules as needed\.   In your **Amazon EC2 Instance Connect** window, test that Node\.js is installed and running correctly by using this command\. 

   ```
   node -v
   ```  This tutorial requires Node v10\.0 or later\.   To configure AWS CLI Your Amazon EC2 instance comes preloaded with the AWS CLI\. However, you must complete your AWS CLI profile\. For more information on how to configure your CLI, see [Configuring the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-configure.html)\.  The following example shows sample values\. Replace them with your own values\. You can find these values in your AWS console in your account info under **My Security Credentials**\. In your **Amazon EC2 Instance Connect** window, enter this command: 

   ```
   aws configure
   ``` Then enter the values from your account at the prompts displayed\. 

   ```
   AWS Access Key ID [None]: AKIAIOSFODNN7EXAMPLE
   AWS Secret Access Key [None]: wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
   Default region name [None]: us-west-2
   Default output format [None]: json
   ```   You can test your AWS CLI configuration with this command: 

   ```
   aws iot describe-endpoint --endpoint-type iot:Data-ATS
   ``` If your AWS CLI is configured correctly, the command should return an endpoint address from your AWS IoT account\.     Create AWS IoT resources for your virtual device  This section describes how to use the AWS CLI to create the thing object and its certificate files directly on the virtual device\. This is done directly on the device to avoid the potential complication that might arise from copying them to the device from another computer\. To create an AWS IoT thing object in your Linux instance Devices connected to AWS IoT are represented by *thing objects* in the AWS IoT registry\. A *thing object* represents a specific device or logical entity\. In this case, your *thing object* will represent your virtual device, this Amazon EC2 instance\.  In your **Amazon EC2 Instance Connect** window, run the following command to create your thing object\. 

   ```
   aws iot create-thing --thing-name "MyIotThing"
   ```   The JSON response should look like this: 

   ```
   {
       "thingArn": "arn:aws:iot:your-region:your-aws-account:thing/MyIotThing", 
       "thingName": "MyIotThing",
       "thingId": "6cf922a8-d8ea-4136-f3401EXAMPLE"
   }
   ```   To create and attach AWS IoT keys and certificates in your Linux instance The [create\-keys\-and\-certificate](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/iot/create-keys-and-certificate.html) command creates client certificates signed by the Amazon Root certificate authority\. This certificate is used to authenticate the identity of your virtual device\.  In your **Amazon EC2 Instance Connect** window, create a directory to store your certificate and key files\. 

   ```
   mkdir ~/certs
   ```   In your **Amazon EC2 Instance Connect** window, download a copy of the Amazon certificate authority \(CA\) certificate by using this command\. 

   ```
   curl -o ~/certs/Amazon-root-CA-1.pem \
       https://www.amazontrust.com/repository/AmazonRootCA1.pem
   ```   In your **Amazon EC2 Instance Connect** window, run the following command to create your private key, public key, and X\.509 certificate files\. This command also registers and activates the certificate with AWS IoT\. 

   ```
   aws iot create-keys-and-certificate \
       --set-as-active \
       --certificate-pem-outfile ~/certs/device.pem.crt \
       --public-key-outfile ~/certs/public.pem.key \
       --private-key-outfile ~/certs/private.pem.key
   ``` The response looks like the following\. Save the `certificateArn` so that you can use it in subsequent commands\. You'll need it to attach your certificate to your thing and to attach the policy to the certificate in a later steps\. 

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
   ```   In your **Amazon EC2 Instance Connect** window, attach your thing object to the certificate you just created by using the following command and the *certificateArn* in the response from the previous command\. 

   ```
   aws iot attach-thing-principal \
       --thing-name MyIotThing \
       --principal certificateArn
   ``` If successful, this command does not display any output\.   To create and attach a policy   In your **Amazon EC2 Instance Connect** window, create the policy file by copying and pasting this policy document to a file named **\~/policy\.json**\. If you don't have a favorite Linux editor, you can open nano, by using this command\. 

   ```
   nano ~/policy.json
   ``` And pasting the policy document for `policy.json` into it\. Enter ctrl\-x to exit the nano editor and save the file\. Contents of the policy document for `policy.json`\. 

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
   ```   In your **Amazon EC2 Instance Connect** window, create your policy by using the following command\. 

   ```
   aws iot create-policy \
       --policy-name MyIotThingPolicy \
       --policy-document file://~/policy.json
   ``` Output: 

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
   ```   In your **Amazon EC2 Instance Connect** window, attach the policy to your virtual device's certificate by using the following command\. 

   ```
   aws iot attach-policy \
       --policy-name MyIotThingPolicy \
       --target "certificateArn"
   ``` If successful, this command does not display any output\.   At this point, you have created for your virtual device:    A thing object to represent your virtual device in AWS IoT\.   A certificate to authenticate your virtual device\.   A policy document to authorize your virtual device to Connect to AWS IoT, and to Publish, Receive, and Subscribe to messages\.     Install the AWS IoT Device SDK for JavaScript  In this section, you'll install the AWS IoT Device SDK for JavaScript, which contains the code that applications can use to communicate with AWS IoT and the sample programs\. To install the AWS IoT Device SDK for JavaScript on your Linux instance  In your **Amazon EC2 Instance Connect** window, clone the AWS IoT Device SDK for JavaScript repository into the `aws-iot-device-sdk-js-v2` directory of your home directory by using this command\. 

   ```
   cd ~
   git clone https://github.com/aws/aws-iot-device-sdk-js-v2.git
   ```   Navigate to the `aws-iot-device-sdk-js-v2` directory that you created in the preceding step\. 

   ```
   cd aws-iot-device-sdk-js-v2
   ```   Use npm to install the SDK\. 

   ```
   npm install
   ```     Run the sample application   The commands in the next sections assume that your key and certificate files are stored on your virtual device as shown in this table\. 


**Certificate file names**  

|  File  |  File path  | 
| --- | --- | 
|  Private key  |  `~/certs/private.pem.key`  | 
|  Device certificate  |  `~/certs/device.pem.crt`  | 
|  Root CA certificate  |  `~/certs/Amazon-root-CA-1.pem`  |  In this section, you'll install and run the `pub-sub.js` sample app found in the `aws-iot-device-sdk-js-v2/samples/node` directory of the AWS IoT Device SDK for JavaScript\. This app shows how a device, your Amazon EC2 instance, uses the MQTT library to publish and subscribe to MQTT messages\. The `pub-sub.js` sample app subscribes to a topic, `topic_1`, publishes 10 messages to that topic, and displays the messages as they're received from the AWS IoT message broker\. To install and run the sample app  In your **Amazon EC2 Instance Connect** window, navigate to the `aws-iot-device-sdk-js-v2/samples/node/pub_sub` directory that the SDK created and install the sample app by using these commands\. 

   ```
   cd ~/aws-iot-device-sdk-js-v2/samples/node/pub_sub
   npm install
   ```   In your **Amazon EC2 Instance Connect** window, get *your\-iot\-endpoint* from AWS IoT by using this command\. 

   ```
   aws iot describe-endpoint --endpoint-type iot:Data-ATS
   ```   In your **Amazon EC2 Instance Connect** window, insert *your\-iot\-endpoint* as indicated and run this command\. 

   ```
   node dist/index.js --topic topic_1 --root-ca ~/certs/Amazon-root-CA-1.pem --cert ~/certs/device.pem.crt --key ~/certs/private.pem.key --endpoint your-iot-endpoint
   ```   The sample app:   Connects to the AWS IoT service for your account\.   Subscribes to the message topic, **topic\_1**, and displays the messages it receives on that topic\.   Publishes 10 messages to the topic, **topic\_1**\.   Displays output similar to the following: 

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
   ```   If you're having trouble running the sample app, add the `--verbosity debug` parameter to the command line so the sample app displays detailed messages about what it’s doing\. That information might provide you the help you need to correct the problem\.   View messages from the sample app in the AWS IoT console  You can see the sample app's messages as they pass through the AWS IoT message broker by using the **MQTT client** in the **AWS IoT console**\.  To view the MQTT messages published by the sample app Review [View MQTT messages with the AWS IoT MQTT client](view-mqtt-messages.md)\. This helps you learn how to use the **MQTT client** in the **AWS IoT console** to view MQTT messages as they pass through the AWS IoT message broker\. Open the **MQTT client** in the **AWS IoT console**\. Subscribe to the topic, **topic\_1**\.  In your **Amazon EC2 Instance Connect** window, run the sample app again and watch the messages in the **MQTT client** in the **AWS IoT console**\. 

   ```
   cd ~/aws-iot-device-sdk-js-v2/samples/node/pub_sub
   node dist/index.js --topic topic_1 --root-ca ~/certs/Amazon-root-CA-1.pem --cert ~/certs/device.pem.crt --key ~/certs/private.pem.key --endpoint your-iot-endpoint
   ```      Use your Windows or Linux PC or Mac as an AWS IoT device  In this tutorial, you'll configure a personal computer for use with AWS IoT\. These instructions support Windows and Linux PCs and Macs\. To accomplish this, you will need to install some software on your computer\. If you don't want to install software on your computer, you might try [Create a virtual device with Amazon EC2](creating-a-virtual-thing.md), which installs all software on a virtual machine\.  In this tutorial, you'll:  Set up your personal computer  To complete this tutorial, you need a Windows or Linux PC or a Mac with a connection to the Internet\. Before you continue to the next step, make sure you can open a command line window on your computer\. Use cmd\.exe on a Windows PC\. On a Linux PC or a Mac, use Terminal\.    Install Git, Node\.js, npm, and the AWS IoT Device SDK for JavaScript  In this section, you'll install Node\.js, the npm package manager, and the AWS IoT Device SDK for JavaScript on your computer\.  Install the latest version of Node\.js  To download and install Node\.js and the npm package manager on your computer  Check to see if you have already installed Node\. Enter this command in the command line\. 

   ```
   node -v
   ``` If the command displays the Node version, Node is already installed\. This tutorial requires Node v10\.0 or later\.    Check to see if you have already installed npm\. Enter this command in the command line\. 

   ```
   npm -v
   ``` If the command displays the npm version, npm is already installed\.   If Node and npm are both installed, you can skip the rest of the steps in this section\. If not, continue\.   Open [https://nodejs\.org/](https://nodejs.org/) and download the installer for your computer\.   If the download didn't automatically start to install, run the downloaded program to install Node and npm\.    Verify the installation of Node\. 

   ```
   node -v
   ``` Confirm that the command displays the Node version\. If the Node version isn't displayed, try downloading and installing Node again\.   Verify the installation of npm\. 

   ```
   npm -v
   ``` Confirm that the command displays the npm version\. If the npm version is not displayed, try downloading and installing Node again\.     Install the AWS IoT Device SDK for JavaScript  To install the AWS IoT Device SDK for JavaScript on your computer  Check to see if you have Git installed on your computer\. Enter this command in the command line\. 

   ```
   git --version
   ``` If the command displays the Git version, Git is installed and you can continue to the next step\. If the command displays an error, open [https://git-scm.com/download](https://git-scm.com/download) and install Git for your computer\.   Clone the AWS IoT Device SDK for JavaScript repository into the aws\-iot\-device\-sdk\-js\-v2 directory of your home directory\. This procedure refers to the base directory for the files to install as *home*\. The actual location of the *home* directory depends on your operating system\. In Windows, you can find the home directory path by running this command in the `cmd` window\. 

   ```
   echo %USERPROFILE%
   ``` In macOS and Linux, the *home* directory is `~`\.   From the *home* directory, run this command to create a `certs` subdirectory that you'll use when you run the sample applications\. 

   ```
   mkdir certs
   ```    From your *home* directory, enter this command to create the `aws-iot-device-sdk-js-v2` directory and copy the SDK code into it\. 

   ```
   git clone https://github.com/aws/aws-iot-device-sdk-js-v2.git
   ```   Change to the `aws-iot-device-sdk-js-v2` directory that was created in the preceding step\. 

   ```
   cd aws-iot-device-sdk-js-v2
   ```   Use npm to install the SDK by running this command from your current directory\. 

   ```
   npm install
   ```     Prepare to run the sample applications  To prepare your system to run the sample application  Into the `certs` directory that you created in the previous section, copy the private key, device certificate, and root CA certificate files you saved when you created and registered the thing object in [Create AWS IoT resources](create-iot-resources.md)\. The file names of each file in the destination directory should match those in the table\.  The commands in the next section assume that your key and certificate files are stored on your device as shown in this table\.   Linux/macOS    
**Certificate file names**    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/iot/latest/developerguide/using-laptop-as-device.html) Run this command to list the files in the `certs` directory and compare them to those listed in the table\. 

  ```
  ls -l ~/certs
  ```    Windows    
**Certificate file names**    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/iot/latest/developerguide/using-laptop-as-device.html) Run this command to list the files in the `certs` directory and compare them to those listed in the table\. 

  ```
  dir %USERPROFILE%\certs
  ```         Install and run the sample application  In this section, you'll install and run the `pub-sub.js` sample app found in the `aws-iot-device-sdk-js-v2/samples/node` directory of the AWS IoT Device SDK for JavaScript\. This app shows how your device uses the MQTT library to publish and subscribe to MQTT messages\. The `pub-sub.js` sample app subscribes to a topic, `topic_1`, publishes 10 messages to that topic, and displays the messages as they're received from the AWS IoT message broker\.  To run the `pub-sub.js` sample app, you need the following information:  


**Application parameter values**  

|  Parameter  |  Where to find the value  | 
| --- | --- | 
| your\-iot\-endpoint |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/iot/latest/developerguide/using-laptop-as-device.html)  |  The *your\-iot\-endpoint* value has a format of: `endpoint_id-ats.iot.region.amazonaws.com`, for example, `a3qj468EXAMPLE-ats.iot.us-west-2.amazonaws.com`\.    Linux/macOS  To install and run the example app on Linux/macOS  In your command line window, navigate to the `~/aws-iot-device-sdk-js-v2/samples/node/pub_sub` directory that the SDK created and install the sample app by using these commands\. 

   ```
   cd ~/aws-iot-device-sdk-js-v2/samples/node/pub_sub
   npm install
   ```   In your command line window, replace *your\-iot\-endpoint* as indicated and run this command\.  

   ```
   node dist/index.js --topic topic_1 --root-ca ~/certs/Amazon-root-CA-1.pem --cert ~/certs/device.pem.crt --key ~/certs/private.pem.key --endpoint your-iot-endpoint
   ```      Windows  To run the example app on a Windows PC  In your command line window, navigate to the `%USERPROFILE%/aws-iot-device-sdk-js-v2/samples/node/pub_sub` directory that the SDK created and install the sample app by using these commands\. 

   ```
   cd %USERPROFILE%\aws-iot-device-sdk-js-v2\samples\node\pub_sub
   npm install
   ```   In your command line window, replace *your\-iot\-endpoint* as indicated and run this command \. 

   ```
   node dist\index.js --topic topic_1 --root-ca %USERPROFILE%\certs\Amazon-root-CA-1.pem --cert %USERPROFILE%\certs\device.pem.crt --key %USERPROFILE%\certs\private.pem.key --endpoint your-iot-endpoint
   ```      The sample app:   Connects to the AWS IoT service for your account\.   Subscribes to the message topic, **topic\_1**, and displays the messages it receives on that topic\.    Publishes 10 messages to the topic, **topic\_1**\.    Displays output similar to the following:   

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
``` If you're having trouble running the sample app, add the `--verbosity debug` parameter to the command line so the sample app displays detailed messages about what it’s doing\. That information might provide you the help you need to correct the problem\.    View messages from the sample app in the AWS IoT console  You can see the sample app's messages as they pass through the AWS IoT message broker by using the **MQTT client** in the **AWS IoT console**\.  To view the MQTT messages published by the sample app  Review [View MQTT messages with the AWS IoT MQTT client](view-mqtt-messages.md)\. This helps you learn how to use the **MQTT client** in the **AWS IoT console** to view MQTT messages as they pass through the AWS IoT message broker  Open the **MQTT client** in the **AWS IoT console**\. Subscribe to the topic, **topic\_1**\.  In your command line window, run the sample app again and watch the messages in the **MQTT client** in the **AWS IoT console**\.   Linux/macOS  

   ```
   cd ~/aws-iot-device-sdk-js-v2/samples/node/pub_sub
   node dist/index.js --topic topic_1 --root-ca ~/certs/Amazon-root-CA-1.pem --cert ~/certs/device.pem.crt --key ~/certs/private.pem.key --endpoint your-iot-endpoint
   ```    Windows  

   ```
   cd %USERPROFILE%\aws-iot-device-sdk-js-v2\samples\node\pub_sub
   node dist\index.js --topic topic_1 --root-ca %USERPROFILE%\certs\Amazon-root-CA-1.pem --cert %USERPROFILE%\certs\device.pem.crt --key %USERPROFILE%\certs\private.pem.key --endpoint your-iot-endpoint
   ```         Connect a Raspberry Pi or another device  In this section, we'll configure a Raspberry Pi for use with AWS IoT\. If you have another device that you'd like to connect, the instructions for the Raspberry Pi include references that can help you adapt these instructions to your device\. In this tutorial, you'll:  Adapting these instructions to other devices and operating systems can be challenging\. You'll need to understand your device well enough to be able to interpret these instructions and apply them to your device\. If you encounter difficulties while configuring your device for AWS IoT, we can't offer any assistance beyond the instructions in this section\. However, you might try one of the other device options as an alternative, such as [Create a virtual device with Amazon EC2](creating-a-virtual-thing.md) or [Use your Windows or Linux PC or Mac as an AWS IoT device](using-laptop-as-device.md)\.    Set up your device  The goal of this step is to collect what you'll need to configure your device such that it can start the operating system \(OS\), connect to the Internet, and allow you to interact with it at a command line interface\.  To complete this tutorial, you need the following:   An AWS account\. If you don't have one, complete the steps described in [Set up your AWS account](setting-up.md) before you continue\.    A [Raspberry Pi 3 Model B](https://www.raspberrypi.org/products/) or more recent model\. This might work on earlier versions of the Raspberry Pi, but they have not been tested\.   [Raspberry Pi OS \(32\-bit\)](https://www.raspberrypi.org/downloads/raspberry-pi-os/) or later\. We recommend using the latest version of the Raspberry Pi OS\. Earlier versions of the OS might work, but they have not been tested\. To run this example, you do not need to install the desktop with the graphical user interface \(GUI\); however, if you're new to the Raspberry Pi and your Raspberry Pi hardware supports it, using the desktop with the GUI might be easier\.   An Ethernet or Wi\-Fi connection\.   Keyboard, mouse, monitor, cables, power supplies, and other hardware required by your device\.    Before you continue to the next step, your device must have its operating system installed, configured, and running\. The device must be connected to the Internet and you will need to be able to access the device by using its command line interface\. Command line access can be through a directly\-connected keyboard, mouse, and monitor, or by using an SSH terminal remote interface\.    If you are running an operating system on your Raspberry Pi that has a graphical user interface \(GUI\), open a terminal window on the device and perform the following instructions in that window\. Otherwise, if you are connecting to your device by using a remote terminal, such as PuTTY, open a remote terminal to your device and use that\.   Install Git, Node\.js, npm, and the AWS IoT Device SDK for JavaScript  In this section, you'll install Git, Node\.js, the npm package manager, and the AWS IoT Device SDK for JavaScript on your device\. These instructions are for a Raspberry Pi running the Raspberry Pi OS\. If you have another device or are using another operating system, you might need to adapt these instructions for your device\.  Install Git  If your device's operating system doesn't come with Git installed, you'll need to install it to install the AWS IoT Device SDK for JavaScript\. To test for Git and install it if necessary Test to see if Git is already installed by running this command\. 

   ```
   git --version
   ```   If the previous command returns the Git version, Git is already installed and you can skip to the next section\.  If an error is displayed when you run the git command, install Git by running this command\. 

   ```
   sudo apt-get install git
   ``` Test again to see if Git is installed by running this command\. 

   ```
   git --version
   ```  If Git is installed, continue to the next section\. If not, troubleshoot and correct the error before continuing\. You need Git to install the AWS IoT Device SDK for JavaScript\.    Install the latest version of Node\.js  The AWS IoT Device SDK for JavaScript requires Node\.js and the npm package manager to be installed on your Raspberry Pi\. To download the latest version of the Node repository  Download the latest version of the Node repository by entering this command\. 

   ```
   cd ~
   curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash -
   ```   Install Node and npm\. 

   ```
   sudo apt-get install -y nodejs
   ```   Verify the installation of Node\. 

   ```
   node -v
   ``` Confirm that the command displays the Node version\. This tutorial requires Node v10\.0 or later\. If the Node version isn't displayed, try downloading the Node repository again\.   Verify the installation of npm\. 

   ```
   npm -v
   ``` Confirm that the command displays the npm version\. If the npm version isn't displayed, try installing Node and npm again\.     Install the AWS IoT Device SDK for JavaScript  Install the AWS IoT Device SDK for JavaScript on your Raspberry Pi\. To install the AWS IoT Device SDK for JavaScript on your device  Clone the AWS IoT Device SDK for JavaScript repository into the `aws-iot-device-sdk-js-v2` directory of your *home* directory\. On the Raspberry Pi, the *home* directory is `~/`, which is used as the *home* directory in the following commands\. If your device uses a different path for the *home* directory, you must replace `~/` with the correct path for your device in the following commands\. These commands create the `~/aws-iot-device-sdk-js-v2` directory and copies the SDK code into it\. 

   ```
   cd ~
   git clone https://github.com/aws/aws-iot-device-sdk-js-v2.git
   ```   Change to the `aws-iot-device-sdk-js-v2` directory that you created in the preceding step and install the SDK\. 

   ```
   cd ~/aws-iot-device-sdk-js-v2
   npm install
   ```     Install and run the sample app  This section describes how to install and run the sample app\. The steps in the procedures are run from a command window running on, or remotely connected to, your device\. To prepare your system to run the sample application  In your *home* directory, create a `certs` subdirectory by running these commands\. 

   ```
   cd ~
   mkdir certs
   ```   Into the `~/certs` directory, copy the private key, device certificate, and root CA certificate that you created earlier in [Create AWS IoT resources](create-iot-resources.md)\. How you copy the certificate files to your device depends on the device and operating system and isn't described here\. However, if your device supports a graphical user interface \(GUI\) and has a web browser, you can perform the procedure described in [Create AWS IoT resources](create-iot-resources.md) from your device's web browser to download the resulting files directly to your device\. The commands in the next section assume that your key and certificate files are stored on the device as shown in this table\.   
**Certificate file names**    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/iot/latest/developerguide/connecting-to-existing-device.html)      Install and run the sample app  In this section, you'll install and run the `pub-sub.js` sample app found in the `aws-iot-device-sdk-js-v2/samples/node` directory of the AWS IoT Device SDK for JavaScript\. This app shows how your device uses the MQTT library to publish and subscribe to MQTT messages\. The `pub-sub.js` sample app subscribes to a topic, `topic_1`, publishes 10 messages to that topic, and displays the messages as they're received from the AWS IoT message broker\. To run the `pub-sub.js` sample app, you need the following information: 


**Application parameter values**  

|  Parameter  |  Where to find the value  | 
| --- | --- | 
| your\-iot\-endpoint |  In the [AWS IoT console](https://console.aws.amazon.com/iot/home), choose **Manage**, and then choose **Things**\. Choose the IoT thing you created for your device, **MyIotThing** was the name used earlier, and then choose **Interact**\. On the thing details page, your endpoint is displayed in the ** HTTPS** section\.  |  The *your\-iot\-endpoint* value has a format of: `endpoint_id-ats.iot.region.amazonaws.com`, for example, `a3qj468EXAMPLE-ats.iot.us-west-2.amazonaws.com`\.  To install and run the example app  In your command line window, navigate to the `~/aws-iot-device-sdk-js-v2/samples/node/pub_sub` directory that the SDK created and install the sample app by using these commands\. 

   ```
   cd ~/aws-iot-device-sdk-js-v2/samples/node/pub_sub
   npm install
   ```   In the command line window, replace *your\-iot\-endpoint* as indicated and run this command\. 

   ```
   node dist/index.js --topic topic_1 --root-ca ~/certs/Amazon-root-CA-1.pem --cert ~/certs/device.pem.crt --key ~/certs/private.pem.key --endpoint your-iot-endpoint
   ```   The sample app:   Connects to the AWS IoT service for your account\.   Subscribes to the message topic, **topic\_1**, and displays the messages it receives on that topic\.    Publishes 10 messages to the topic, **topic\_1**\.    Displays output similar to the following:   

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
``` If you're having trouble running the sample app, add the `--verbosity debug` parameter to the command line so the sample app displays detailed messages about what it’s doing\. That information might provide you the help you need to correct the problem\.    View messages from the sample app in the AWS IoT console  You can see the sample app's messages as they pass through the AWS IoT message broker by using the **MQTT client** in the **AWS IoT console**\.  To view the MQTT messages published by the sample app Review [View MQTT messages with the AWS IoT MQTT client](view-mqtt-messages.md)\. This helps you learn how to use the **MQTT client** in the **AWS IoT console** to view MQTT messages as they pass through the AWS IoT message broker\. Open the **MQTT client** in the **AWS IoT console**\. Subscribe to the topic, **topic\_1**\.  In your command line window, run the sample app again and watch the messages in the **MQTT client** in the **AWS IoT console**\. 

   ```
   cd ~/aws-iot-device-sdk-js-v2/samples/node/pub_sub
   node dist/index.js --topic topic_1 --root-ca ~/certs/Amazon-root-CA-1.pem --cert ~/certs/device.pem.crt --key ~/certs/private.pem.key --endpoint your-iot-endpoint
   ```     ](configure-device.md) is running the example program instances, you should see the messages that each instance is sending to AWS IoT in the message log on the corresponding **Publish** page\. 

1. On the **Publish** page, you can also publish messages to the subscribed topic\. Messages published to the subscribed topic appear below the message payload window as they are received\. 

**Troubleshooting**  
If your messages are not showing up as you expect, try subscribing to a wild card topic as described in [Topic filters](topics.md#topicfilters)\. For example, in the example shown here, you might subscribe to wild card topic filter of `#` to see all messages\. MQTT topics are case sensitive, so if your device is publishing messages to `Topic_1` instead of `topic_1`, to which you subscribed, its messages would not appear in the MQTT client\.

## Publishing MQTT messages from the MQTT client<a name="view-mqtt-publish"></a>

**To publish a message to an MQTT topic**

1. On the MQTT client page, in the **Publish** section, in the **Specify a topic and a message to publish** field, enter the *topicName* of your message\. In this example, use **my/topic**\. 
**Note**  
Do not use personally identifiable information in topic names, whether using them in the MQTT client or in your system implementation\. Topic names can appear in unencrypted communications and reports\.

1. In the message payload window, enter the following JSON:

   ```
   {
       "message": "Hello, world",
       "clientType": "MQTT client"
   }
   ```

1. Choose **Publish to topic** to publish your message to AWS IoT\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/publish-to-topic.png)

1. In the **Subscriptions** column, choose **my/topic** to see the message\. You should see the message appear in the MQTT client below the publish message payload window\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/publish-to-topic-received.png)

You can publish MQTT messages to other topics by changing the *topicName* in the **Specify a topic and a message to publish** field and choosing the **Publish to topic** button\.