# Tutorial: Installing the Device SDK and running the sample application for Device Shadows<a name="lightbulb-shadow-application"></a>

This section shows how you can install the required software and the AWS IoT Device SDK for Python and run the `shadow.py` sample application to edit the Shadow document and control the shadow's state\. 

**In this tutorial, you'll learn how to:**
+ Use the installed software and AWS IoT Device SDK for Python to run the sample app\.
+ Learn how entering a value using the sample app publishes the desired value in the AWS IoT console\.
+ Review the `shadow.py` sample app and how it uses the MQTT protocol to update the shadow's state\.

**Before you run this tutorial:**  
You must have set up your AWS account, configured your Raspberry Pi device, and created an AWS IoT thing and policy that gives the device permissions to publish and subscribe to the MQTT reserved topics of the Device Shadow service\. For more information, see [Tutorial: Preparing your Raspberry Pi to run the shadow application](create-resources-shadow.md)\.

You must have also installed Git, Python, and the AWS IoT Device SDK for Python\. This tutorial builds on the concepts presented in the tutorial [Connect a Raspberry Pi or another device](connecting-to-existing-device.md)\. If you haven't tried that tutorial, we recommend that you follow the steps described in that tutorial to install the certificate files and Device SDK and then come back to this tutorial to run the `shadow.py` sample app\.

**Topics**
+ [Step 1: Run the shadow\.py sample app](#run-sample-application-shadows)
+ [Step 2: Review the shadow\.py Device SDK sample app](#review-shadow-sample-code)
+ [Step 3: Troubleshoot problems with the `shadow.py` sample app](#shadow-sample-app-troubleshoot)
+ [Step 4: Review the results and next steps](#sample-app-shadow-review)

This tutorial takes about 20 minutes to complete\.

## Step 1: Run the shadow\.py sample app<a name="run-sample-application-shadows"></a>

Before you run the `shadow.py` sample app, you'll need the following information in addition to the names and location of the certificate files that you installed\.


**Application parameter values**  

|  Parameter  |  Where to find the value  | 
| --- | --- | 
| your\-iot\-thing\-name |  Name of the AWS IoT thing that you created earlier in [Step 2: Create a thing resource and attach the policy to the thing](shadow-provision-cloud.md#create-thing-shadow)\. To find this value, in the [AWS IoT console](https://console.aws.amazon.com/iot/home), choose **Manage**, and then choose **Things**\.  | 
| your\-iot\-endpoint |   The *your\-iot\-endpoint* value has a format of: `endpoint_id-ats.iot.region.amazonaws.com`, for example, `a3qj468EXAMPLE-ats.iot.us-west-2.amazonaws.com`\. To find this value: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/iot/latest/developerguide/lightbulb-shadow-application.html)  | 

**Install and run the sample app**

1. Navigate to the sample app directory\.

   ```
   cd ~/aws-iot-device-sdk-python-v2/samples
   ```

1. In the command line window, replace *your\-iot\-endpoint* and *your\-iot\-thing\-name* as indicated and run this command\.

   ```
   python3 shadow.py --root-ca ~/certs/Amazon-root-CA-1.pem --cert ~/certs/device.pem.crt --key ~/certs/private.pem.key --endpoint your-iot-endpoint --thing-name your-iot-thing-name
   ```

1. Observe that the sample app:

   1. Connects to the AWS IoT service for your account\.

   1. Subscribes to `Delta` events and `Update` and `Get` responses\.

   1. Prompts you to enter a desired value in the terminal\.

   1. Displays output similar to the following:

   ```
   Connecting to a3qEXAMPLEffp-ats.iot.us-west-2.amazonaws.com with client ID 'test-0c8ae2ff-cc87-49d2-a82a-ae7ba1d0ca5a'...
   Connected!
   Subscribing to Delta events...
   Subscribing to Update responses...
   Subscribing to Get responses...
   Requesting current shadow state...
   Launching thread to read user input...
   Finished getting initial shadow state.
   Shadow contains reported value 'off'.
   Enter desired value:
   ```

**Note**  
If you're having trouble running the `shadow.py` sample app, review [Step 3: Troubleshoot problems with the `shadow.py` sample app](#shadow-sample-app-troubleshoot)\. To get additional information that might help you correct the problem, add the `--verbosity debug` parameter to the command line so the sample app displays detailed messages about what it’s doing\.

**Enter values and observe the updates in Shadow document**  
You can enter values in the terminal to specify the `desired` value, which also updates the `reported` value\. Say you enter the color `yellow` in the terminal\. The `reported` value is also updated to the color `yellow`\. The following shows the messages displayed in the terminal:

```
Enter desired value:
yellow
Changed local shadow value to 'yellow'.
Updating reported shadow value to 'yellow'...
Update request published.
Finished updating reported shadow value to 'yellow'.
```

When you publish this update request, AWS IoT creates a default, classic shadow for the thing resource\. You can observe the update request that you published to the `reported` and `desired` values in the AWS IoT console by looking at the Shadow document for the thing resource that you created \(for example, `My_light_bulb`\)\. To see the update in the Shadow document:

1. In the AWS IoT console, choose **Manage** and then choose **Things**\.

1. In the list of things displayed, select the thing that you created, choose **Shadows**, and then choose **Classic Shadow**\.

The Shadow document should look similar to the following, showing the `reported` and `desired` values set to the color `yellow`\. You see these values in the **Shadow state** section of the document\.

```
{
"desired": {
  "welcome": "aws-iot",
  "color": "yellow"
},
"reported": {
  "welcome": "aws-iot",
  "color": "yellow"
}
}
```

You also see a **Metadata** section that contains the timestamp information and version number of the request\.

You can use the state document version to ensure you are updating the most recent version of a device's Shadow document\. If you send another update request, the version number increments by 1\. When you supply a version with an update request, the service rejects the request with an HTTP 409 conflict response code if the current version of the state document doesn't match the version supplied\. 

```
{
"metadata": {
  "desired": {
    "welcome": {
      "timestamp": 1620156892
    },
    "color": {
      "timestamp": 1620156893
    }
  },
  "reported": {
    "welcome": {
      "timestamp": 1620156892
    },
    "color": {
      "timestamp": 1620156893
    }
  }
},
"version": 10
}
```

To learn more about the Shadow document and observe changes to the state information, proceed to the next tutorial [Tutorial: Interacting with Device Shadow using the sample app and the MQTT test client](interact-lights-device-shadows.md) as described in the [Step 4: Review the results and next steps](#sample-app-shadow-review) section of this tutorial\. Optionally, you can also learn about the `shadow.py` sample code and how it uses the MQTT protocol in the following section\.

## Step 2: Review the shadow\.py Device SDK sample app<a name="review-shadow-sample-code"></a>

This section reviews the `shadow.py` sample app from the **AWS IoT Device SDK v2 for Python** used in this tutorial\. Here, we'll review how it connects to AWS IoT Core by using the MQTT and MQTT over WSS protocol\. The [AWS common runtime \(AWS\-CRT\)](https://github.com/awslabs/aws-crt-python#aws-crt-python) library provides the low\-level communication protocol support and is included with the AWS IoT Device SDK v2 for Python\.

While this tutorial uses MQTT and MQTT over WSS, AWS IoT supports devices that publish HTTPS requests\. For an example of a Python program that sends an HTTP message from a device, see the [HTTPS code example](http.md#codeexample) using Python’s `requests` library\. 

For information about how you can make an informed decision about which protocol to use for your device communications, review the [Choosing a protocol for your device communicationConnection duration limits](protocols.md#protocol-selection)\.

**MQTT**  
The `shadow.py` sample calls `mtls_from_path` \(shown here\) in the [https://github.com/awslabs/aws-crt-python/blob/89207bcf1387177034e02fe29e8e469ca45e39b7/awscrt/awsiot_mqtt_connection_builder.py](https://github.com/awslabs/aws-crt-python/blob/89207bcf1387177034e02fe29e8e469ca45e39b7/awscrt/awsiot_mqtt_connection_builder.py) to establish a connection with AWS IoT Core by using the MQTT protocol\. `mtls_from_path` uses X\.509 certificates and TLS v1\.2 to authenticate the device\. The AWS\-CRT library handles the lower\-level details of that connection\.

```
mqtt_connection = mqtt_connection_builder.mtls_from_path(
  endpoint=args.endpoint,
  cert_filepath=args.cert,
  pri_key_filepath=args.key,
  ca_filepath=args.root_ca,
  client_bootstrap=client_bootstrap,
  on_connection_interrupted=on_connection_interrupted,
  on_connection_resumed=on_connection_resumed,
  client_id=args.client_id,
  clean_session=False,
  keep_alive_secs=6
)
```
+ `endpoint` is your AWS IoT endpoint that you passed in from the command line and `client_id` is the ID that uniquely identifies this device in the AWS Region\.
+ `cert_filepath`, `pri_key_filepath`, and `ca_filepath` are the paths to the device's certificate and private key files, and the root CA file\. 
+ `client_bootstrap` is the common runtime object that handles socket communication activities, and is instantiated prior to the call to `mqtt_connection_builder.mtls_from_path`\.
+ `on_connection_interrupted` and `on_connection_resumed` are callback functions to call when the device’s connection is interrupted and resumed\.
+ `clean_session` is whether to start a new, persistent session, or if one is present, reconnect to an existing one\. `keep_alive_secs` is the keep alive value, in seconds, to send in the `CONNECT` request\. A ping will automatically be sent at this interval\. The server assumes that the connection is lost if it doesn't receive a ping after 1\.5 times this value\.

The `shadow.py` sample also calls `websockets_with_default_aws_signing` in the [https://github.com/awslabs/aws-crt-python/blob/89207bcf1387177034e02fe29e8e469ca45e39b7/awscrt/awsiot_mqtt_connection_builder.py](https://github.com/awslabs/aws-crt-python/blob/89207bcf1387177034e02fe29e8e469ca45e39b7/awscrt/awsiot_mqtt_connection_builder.py) to establish a connection with AWS IoT Core using MQTT protocol over WSS\. MQTT over WSS also uses the same parameters as MQTT and takes these additional parameters:
+ `region` is the AWS signing Region used by Signature V4 authentication, and `credentials_provider` is the AWS credentials provided to use for authentication\. The Region is passed from the command line, and the `credentials_provider` object is instantiated just prior to the call to `mqtt_connection_builder.websockets_with_default_aws_signing`\.
+ `websocket_proxy_options` is the HTTP proxy options, if using a proxy host\. In the `shadow.py` sample app, this value is instantiated just prior to the call to `mqtt_connection_builder.websockets_with_default_aws_signing`\.

**Subscribe to Shadow topics and events**  
The `shadow.py` sample attempts to establish a connection and waits to be fully connected\. If it's not connected, commands are queued up\. Once connected, the sample subscribes to delta events and update and get messages, and publishes messages with a Quality of Service \(QoS\) level of 1 \(`mqtt.QoS.AT_LEAST_ONCE`\)\. 

When a device subscribes to a message with QoS level 1, the message broker saves the messages that the device is subscribed to until they can be sent to the device\. The message broker resends the messages until it receives a `PUBACK` response from the device\. 

For more information about the MQTT protocol, see [Review the MQTT protocol](sdk-tutorials.md#sdk-tutorials-mqtt-review) and [MQTT](mqtt.md)\.

For more information about how MQTT, MQTT over WSS, persistent sessions, and QoS levels that are used in this tutorial, see [Review the pubsub\.py Device SDK sample app](sdk-tutorials.md#sdk-tutorials-explore-sample)\.

## Step 3: Troubleshoot problems with the `shadow.py` sample app<a name="shadow-sample-app-troubleshoot"></a>

When you run the `shadow.py` sample app, you should see some messages displayed in the terminal and a prompt to enter a `desired` value\. If the program throws an error, then to debug the error, you can start by checking whether you ran the correct command for your system\.

In some cases, the error message might indicate connection issues and look similar to: `Host name was invalid for dns resolution` or `Connection was closed unexpectedly`\. In such cases, here are some things you can check:
+ 

**Check the endpoint address in the command**  
Review the `endpoint` argument in the command you entered to run the sample app, \(for example, `a3qEXAMPLEffp-ats.iot.us-west-2.amazonaws.com`\) and check this value in the **AWS IoT console**\.

  To check whether you used the correct value:

  1. In the **AWS IoT console**, choose **Manage** and then choose **Things**\.

  1. Choose the thing you created for your sample app \(for example, **My\_light\_bulb**\) and then choose **Interact**\.

  On the thing details page, your endpoint is displayed in the **HTTPS** section\. You should also see a message that says: `This thing already appears to be connected.`
+ 

**Check certificate activation**  
Certificates authenticate your device with AWS IoT Core\.

  To check whether your certificate is active:

  1. In the **AWS IoT console**, choose **Manage** and then choose **Things**\.

  1. Choose the thing you created for your sample app \(for example, **My\_light\_bulb**\) and then choose **Security**\.

  1. Select the certificate and then, from the certificate's details page, choose Select the certificate and then, from the certificate's details page, choose **Actions**\.

  If in the dropdown list **Activate** isn't available and you can only choose **Deactivate**, your certificate is active\. If not, choose **Activate** and rerun the sample program\.

  If the program still doesn't run, check the certificate file names in the `certs` folder\.
+ 

**Check the policy attached to the thing resource**  
While certificates authenticate your device, AWS IoT policies permit the device to perform AWS IoT operations, such as subscribing or publishing to MQTT reserved topics\.

  To check whether the correct policy is attached:

  1. Find the certificate as described previously, and then choose **Policies**\.

  1. Choose the policy displayed and check whether it describes the `connect`, `subscribe`, `receive`, and `publish` actions that give the device permission to publish and subscribe to the MQTT reserved topics\.

     For a sample policy, see [Step 1: Create an AWS IoT policy for the Device Shadow](shadow-provision-cloud.md#create-policy-shadow)\.

  If you see error messages that indicate trouble connecting to AWS IoT, it could be because of the permissions you're using for the policy\. If that's the case, we recommend that you start with a policy that provides full access to AWS IoT resources and then rerun the sample program\. You can either edit the current policy, or choose the current policy, choose **Detach**, and then create another policy that provides full access and attach it to your thing resource\. You can later restrict the policy to only the actions and policies you need to run the program\.

  ```
  {
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "iot:*"
            ],
            "Resource": "*"
        }
    ]
  }
  ```
+ 

**Check your Device SDK installation**  
If the program still doesn't run, you can reinstall the Device SDK to make sure that your SDK installation is complete and correct\.

## Step 4: Review the results and next steps<a name="sample-app-shadow-review"></a>

**In this tutorial, you learned how to:**
+ Install the required software, tools, and the AWS IoT Device SDK for Python\.
+ Understand how the sample app, `shadow.py`, uses the MQTT protocol for retrieving and updating the shadow's current state\.
+ Run the sample app for Device Shadows and observe the update to the Shadow document in the AWS IoT console\. You also learned to troubleshoot any issues and fix errors when running the program\.

**Next steps**  
You can now run the `shadow.py` sample application and use Device Shadows to control the state\. You can observe the updates to the Shadow document in the AWS IoT Console and observe delta events that the sample app responds to\. Using the MQTT test client, you can subscribe to the reserved shadow topics and observe messages received by the topics when running the sample program\. For more information about how to run this tutorial, see [Tutorial: Interacting with Device Shadow using the sample app and the MQTT test client](interact-lights-device-shadows.md)\.