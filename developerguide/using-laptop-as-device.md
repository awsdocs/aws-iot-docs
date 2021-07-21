# Use your Windows or Linux PC or Mac as an AWS IoT device<a name="using-laptop-as-device"></a>

In this tutorial, you'll configure a personal computer for use with AWS IoT\. These instructions support Windows and Linux PCs and Macs\. To accomplish this, you need to install some software on your computer\. If you don't want to install software on your computer, you might try [Create a virtual device with Amazon EC2](creating-a-virtual-thing.md), which installs all software on a virtual machine\. 

**Topics**
+ [Set up your personal computer](#gs-pc-prereqs)
+ [Install Git, Python, and the AWS IoT Device SDK for Python](#gs-pc-sdk-node)
+ [Set up the policy and run the sample application](#gs-pc-python-app-run)
+ [View messages from the sample app in the AWS IoT console](#gs-pc-view-msg)

## Set up your personal computer<a name="gs-pc-prereqs"></a>

To complete this tutorial, you need a Windows or Linux PC or a Mac with a connection to the internet\.

Before you continue to the next step, make sure you can open a command line window on your computer\. Use cmd\.exe on a Windows PC\. On a Linux PC or a Mac, use Terminal\. 

## Install Git, Python, and the AWS IoT Device SDK for Python<a name="gs-pc-sdk-node"></a>

In this section, you'll install Python, and the AWS IoT Device SDK for Python on your computer\.

### Install the latest version of Git and Python<a name="gs-pc-node-runtime"></a>

**To download and install Git and Python on your computer**

1. Check to see if you have Git installed on your computer\. Enter this command in the command line\.

   ```
   git --version
   ```

   If the command displays the Git version, Git is installed and you can continue to the next step\.

   If the command displays an error, open [https://git-scm.com/download](https://git-scm.com/download) and install Git for your computer\.

1. Check to see if you have already installed Python\. Enter this command in the command line\.

   ```
   python -V
   ```
**Note**  
If this command gives an error: `Python was not found`, it might be because your operating system calls the Python v3\.x executable as `Python3`\. In that case, replace all instances of `python` with `python3` and continue the remainder of this tutorial\.

   If the command displays the Python version, Python is already installed\. This tutorial requires Python v3\.5 or later\. 

1. If Python is installed, you can skip the rest of the steps in this section\. If not, continue\.

1. Open [https://www\.python\.org/downloads/](https://www.python.org/downloads/) and download the installer for your computer\.

1. If the download didn't automatically start to install, run the downloaded program to install Python\. 

1. Verify the installation of Python\.

   ```
   python -V
   ```

   Confirm that the command displays the Python version\. If the Python version isn't displayed, try downloading and installing Python again\.

### Install the AWS IoT Device SDK for Python<a name="gs-pc-python-intall-sdk"></a>

**To install the AWS IoT Device SDK for Python on your computer**

1. Install v2 of the AWS IoT Device SDK for Python\.

   ```
   python3 -m pip install awsiotsdk
   ```

1. Clone the AWS IoT Device SDK for Python repository into the aws\-iot\-device\-sdk\-python\-v2 directory of your home directory\. This procedure refers to the base directory for the files you're installing as *home*\.

   The actual location of the *home* directory depends on your operating system\.

------
#### [ Linux/macOS ]

   In macOS and Linux, the *home* directory is `~`\.

   ```
   cd ~ 
   git clone https://github.com/aws/aws-iot-device-sdk-python-v2.git
   ```

------
#### [ Windows ]

   In Windows, you can find the *home* directory path by running this command in the `cmd` window\.

   ```
   echo %USERPROFILE%
   cd %USERPROFILE%
   git clone https://github.com/aws/aws-iot-device-sdk-python-v2.git
   ```

------
**Note**  
If you're using Windows PowerShell as opposed to cmd\.exe, then use the following command\.   

   ```
   echo $home
   ```

### Prepare to run the sample applications<a name="gs-pc-python-config-app"></a>

**To prepare your system to run the sample application**
+ Create the `certs` directory\. Into the `certs` directory, copy the private key, device certificate, and root CA certificate files you saved when you created and registered the thing object in [Create AWS IoT resources](create-iot-resources.md)\. The file names of each file in the destination directory should match those in the table\.

   The commands in the next section assume that your key and certificate files are stored on your device as shown in this table\.

------
#### [ Linux/macOS ]

  Run this command to create the `certs` subdirectory that you'll use when you run the sample applications\.

  ```
  mkdir ~/certs
  ```

  Into the new subdirectory, copy the files to the destination file paths shown in the following table\.  
**Certificate file names**    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/iot/latest/developerguide/using-laptop-as-device.html)

  Run this command to list the files in the `certs` directory and compare them to those listed in the table\.

  ```
  ls -l ~/certs
  ```

------
#### [ Windows ]

  Run this command to create the `certs` subdirectory that you'll use when you run the sample applications\.

  ```
  mkdir %USERPROFILE%\certs
  ```

  Into the new subdirectory, copy the files to the destination file paths shown in the following table\.  
**Certificate file names**    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/iot/latest/developerguide/using-laptop-as-device.html)

  Run this command to list the files in the `certs` directory and compare them to those listed in the table\.

  ```
  dir %USERPROFILE%\certs
  ```

------

## Set up the policy and run the sample application<a name="gs-pc-python-app-run"></a>

In this section, you'll set up your policy and run the `pubsub.py` sample script found in the `aws-iot-device-sdk-python-v2/samples` directory of the AWS IoT Device SDK for Python\. This script shows how your device uses the MQTT library to publish and subscribe to MQTT messages\.

The `pubsub.py` sample app subscribes to a topic, `test/topic`, publishes 10 messages to that topic, and displays the messages as they're received from the message broker\. 

To run the `pubsub.py` sample script, you need the following information: 


**Application parameter values**  

|  Parameter  |  Where to find the value  | 
| --- | --- | 
| your\-iot\-endpoint |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/iot/latest/developerguide/using-laptop-as-device.html)  | 

The *your\-iot\-endpoint* value has a format of: `endpoint_id-ats.iot.region.amazonaws.com`, for example, `a3qj468EXAMPLE-ats.iot.us-west-2.amazonaws.com`\. 

Before running the script, make sure your thing's policy provides permissions for the sample script to connect, subscribe, publish, and receive\. The Policy JSON is displayed in the following example\. In the `"Resource"` element, you should replace the region and account word with your AWS account and Region `"arn:aws:iot:region:account.:topic/test/topic"` 

```
{
"Version": "2012-10-17",
"Statement": [
    {
        "Effect": "Allow",
        "Action": [
            "iot:Publish",
            "iot:Receive"
        ],
        "Resource": [
            "arn:aws:iot:region:account:topic/test/topic"
        ]
    },
    {
        "Effect": "Allow",
        "Action": [
            "iot:Subscribe"
        ],
        "Resource": [
            "arn:aws:iot:region:account:topicfilter/test/topic"
        ]
    },
    {
        "Effect": "Allow",
        "Action": [
            "iot:Connect"
        ],
        "Resource": [
            "arn:aws:iot:region:account:client/test-*"
        ]
    }
]
}
```

------
#### [ Linux/macOS ]

**To run the sample script on Linux/macOS**

1. In your command line window, navigate to the `~/aws-iot-device-sdk-python-v2/samples/node/pub_sub` directory that the SDK created by using these commands\.

   ```
   cd ~/aws-iot-device-sdk-python-v2/samples
   ```

1. In your command line window, replace *your\-iot\-endpoint* as indicated and run this command\. 

   ```
   python3 pubsub.py --endpoint your-iot-endpoint --root-ca ~/certs/Amazon-root-CA-1.pem --cert ~/certs/device.pem.crt --key ~/certs/private.pem.key
   ```

------
#### [ Windows ]

**To run the sample app on a Windows PC**

1. In your command line window, navigate to the `%USERPROFILE%\aws-iot-device-sdk-python-v2\samples` directory that the SDK created and install the sample app by using these commands\.

   ```
   cd %USERPROFILE%\aws-iot-device-sdk-python-v2\samples
   ```

1. In your command line window, replace *your\-iot\-endpoint* as indicated and run this command\.

   ```
   python3 pubsub.py --endpoint your-iot-endpoint --root-ca %USERPROFILE%\certs\Amazon-root-CA-1.pem --cert %USERPROFILE%\certs\device.pem.crt --key %USERPROFILE%\certs\private.pem.key
   ```

------

The sample script:

1. Connects to the AWS IoT service for your account\.

1. Subscribes to the message topic, **test/topic**, and displays the messages it receives on that topic\. 

1. Publishes 10 messages to the topic, **test/topic**\. 

1. Displays output similar to the following:

```
Publish received on topic test/topic
{"message":"Hello world!","sequence":1}
Publish received on topic test/topic
{"message":"Hello world!","sequence":2}
Publish received on topic test/topic
{"message":"Hello world!","sequence":3}
Publish received on topic test/topic
{"message":"Hello world!","sequence":4}
Publish received on topic test/topic
{"message":"Hello world!","sequence":5}
Publish received on topic test/topic
{"message":"Hello world!","sequence":6}
Publish received on topic test/topic
{"message":"Hello world!","sequence":7}
Publish received on topic test/topic
{"message":"Hello world!","sequence":8}
Publish received on topic test/topic
{"message":"Hello world!","sequence":9}
Publish received on topic test/topic
{"message":"Hello world!","sequence":10}
```

If you're having trouble running the sample app, review [Troubleshooting problems with the sample app](gs-device-troubleshoot.md)\.

You can also add the `--verbosity debug` parameter to the command line so the sample app displays detailed messages about what it’s doing\. That information might help you correct the problem\. 

## View messages from the sample app in the AWS IoT console<a name="gs-pc-view-msg"></a>

You can see the sample app's messages as they pass through the message broker by using the **MQTT client** in the **AWS IoT console**\. 

**To view the MQTT messages published by the sample app**

1. Review [View MQTT messages with the AWS IoT MQTT client](view-mqtt-messages.md)\. This helps you learn how to use the **MQTT client** in the **AWS IoT console** to view MQTT messages as they pass through the message broker\.

1. Open the **MQTT client** in the **AWS IoT console**\.

1. Subscribe to the topic, **test/topic**\.

1. In your command line window, run the sample app again and watch the messages in the **MQTT client** in the **AWS IoT console**\.

------
#### [ Linux/macOS ]

   ```
   cd ~/aws-iot-device-sdk-python-v2/samples
   python3 pubsub.py --topic test/topic --root-ca ~/certs/Amazon-root-CA-1.pem --cert ~/certs/device.pem.crt --key ~/certs/private.pem.key --endpoint your-iot-endpoint
   ```

------
#### [ Windows ]

   ```
   cd %USERPROFILE%\aws-iot-device-sdk-python-v2\samples
   python3 pubsub.py --topic test/topic --root-ca %USERPROFILE%\certs\Amazon-root-CA-1.pem --cert %USERPROFILE%\certs\device.pem.crt --key %USERPROFILE%\certs\private.pem.key --endpoint your-iot-endpoint
   ```

------