# Connect a device to AWS IoT Core by using the AWS IoT Device SDK<a name="sdk-tutorials"></a>

This tutorial demonstrates how to connect a device to AWS IoT Core so that it can send and receive data to and from AWS IoT\. After you complete this tutorial, your device will be configured to connect to AWS IoT Core and you'll understand how devices communicate with AWS IoT\.

**In this tutorial, you’ll:**

1. [Prepare your device for AWS IoT](#sdk-tutorials-prepare)

1. [Review the MQTT protocol](#sdk-tutorials-mqtt-review)

1. [Review the pubsub\.py Device SDK sample app](#sdk-tutorials-explore-sample)

1. [Connect your device and communicate with AWS IoT Core](#sdk-tutorials-experiment)

1. [Review the results](#sdk-tutorials-conclusion)

This tutorial takes about an hour to complete\.

**Before you start this tutorial, make sure that you have:**
+ 

**Completed [Getting started with AWS IoT Core](iot-gs.md)**  
In the section of that tutorial where you must [Configure your device](configure-device.md), select the [Connect a Raspberry Pi or another device](connecting-to-existing-device.md) option for your device and use the Python language options to configure your device\.

  Be sure to keep open the terminal window you use in that tutorial because you'll also use it in this tutorial\.
+ 

**A device that can run the AWS IoT Device SDK v2 for Python\.**  
This tutorial shows how to connect a device to AWS IoT Core by using Python code examples, which require a relatively powerful device, as IoT and embedded devices go\.

  If you are working with resource\-constrained devices, these code examples might not work on them\. In that case, you might have more success by [Using the AWS IoT Device SDK for Embedded C](iot-embedded-c-sdk.md) tutorial\.

## Prepare your device for AWS IoT<a name="sdk-tutorials-prepare"></a>

In [Getting started with AWS IoT Core](iot-gs.md), you prepared your device and AWS account so they could communicate\. This section reviews the aspects of that preparation that apply to any device connection with AWS IoT Core\.

For a device to connect to AWS IoT Core:

1. You must have an **AWS account**\.

   The procedure in [Set up your AWS account](setting-up.md) describes how to create an AWS account if you don’t already have one\. 

1. In that account, you must have the following **AWS IoT resources** defined for the device in your AWS account and Region\.

   The procedure in [Create AWS IoT resources](create-iot-resources.md) describes how to create these resources for the device in your AWS account and Region\.
   + A **device certificate** registered with AWS IoT and activated to authenticate the device\.

     The certificate is often created with, and attached to, an **AWS IoT thing object**\. While a thing object is not required for a device to connect to AWS IoT, it makes additional AWS IoT features available to the device\.
   + A **policy** attached to the device certificate that authorizes it to connect to AWS IoT Core and perform all the actions that you want it to\.

1. An **internet connection** that can access your AWS account’s device endpoints\.

   The device endpoints are described in [AWS IoT device data and service endpoints](iot-connect-devices.md#iot-connect-device-endpoints) and can be seen in the [settings page of the AWS IoT console](https://console.aws.amazon.com/iot/home#/settings)\. 

1. **Communication software** such as the AWS IoT Device SDKs provide\. This tutorial uses the [ AWS IoT Device SDK v2 for Python](https://github.com/aws/aws-iot-device-sdk-python-v2#aws-iot-device-sdk-v2-for-python)\.

## Review the MQTT protocol<a name="sdk-tutorials-mqtt-review"></a>

Before we talk about the sample app, it helps to understand the MQTT protocol\. The MQTT protocol offers some advantages over other network communication protocols, such as HTTP, which makes it a popular choice for IoT devices\. This section reviews the key aspects of MQTT that apply to this tutorial\. For information about how MQTT compares to HTTP, see [Choosing a protocol for your device communication](protocols.md#protocol-selection)\.

**MQTT uses a publish/subscribe communication model**  
The MQTT protocol uses a publish/subscribe communication model with its host\. This model differs from the request/response model that HTTP uses\. With MQTT, devices establish a session with the host that is identified by a unique client ID\. To send data, devices publish messages identified by topics to a message broker in the host\. To receive messages from the message broker, devices subscribe to the topics they will receive by sending topic filters in subscription requests to the message broker\.

**MQTT supports persistent sessions**  
The message broker receives messages from devices and publishes messages to devices that have subscribed to them\. With [persistent sessions](mqtt.md#mqtt-persistent-sessions) —sessions that remain active even when the initiating device is disconnected—devices can retrieve messages that were published while they were disconnected\. On the device side, MQTT supports Quality of Service levels \([QoS](mqtt.md#mqtt-qos)\) that ensure the host receives messages sent by the device\.

## Review the pubsub\.py Device SDK sample app<a name="sdk-tutorials-explore-sample"></a>

This section reviews the `pubsub.py` sample app from the **AWS IoT Device SDK v2 for Python** used in this tutorial\. Here, we'll review how it connects to AWS IoT Core to publish and subscribe to MQTT messages\. The next section presents some exercises to help you explore how a device connects and communicates with AWS IoT Core\.

**The `pubsub.py` sample app demonstrates these aspects of an MQTT connection with AWS IoT Core:**
+ [Communication protocols](#sdk-tutorials-explore-protocols)
+ [Persistent sessions](#sdk-tutorials-explore-persistent)
+ [Quality of Service](#sdk-tutorials-explore-qos)
+ [Message publish](#sdk-tutorials-explore-publish)
+ [Message subscription](#sdk-tutorials-explore-subscribe)
+ [Device disconnection and reconnection](#sdk-tutorials-explore-connect)

### Communication protocols<a name="sdk-tutorials-explore-protocols"></a>

The `pubsub.py` sample demonstrates an MQTT connection using the MQTT and MQTT over WSS protocols\. The [AWS common runtime \(AWS CRT\)](https://github.com/awslabs/aws-crt-python#aws-crt-python) library provides the low\-level communication protocol support and is included with the AWS IoT Device SDK v2 for Python\.

#### MQTT<a name="sdk-tutorials-explore-mqtt"></a>

The `pubsub.py` sample calls `mtls_from_path` \(shown here\) in the [https://github.com/awslabs/aws-crt-python/blob/89207bcf1387177034e02fe29e8e469ca45e39b7/awscrt/awsiot_mqtt_connection_builder.py](https://github.com/awslabs/aws-crt-python/blob/89207bcf1387177034e02fe29e8e469ca45e39b7/awscrt/awsiot_mqtt_connection_builder.py) to establish a connection with AWS IoT Core by using the MQTT protocol\. `mtls_from_path` uses X\.509 certificates and TLS v1\.2 to authenticate the device\. The AWS CRT library handles the lower\-level details of that connection\.

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

`endpoint`  
Your AWS account’s IoT device endpoint  
In the sample app, this value is passed in from the command line\.

`cert_filepath`  
The path to the device’s certificate file  
In the sample app, this value is passed in from the command line\.

`pri_key_filepath`  
The path to the device’s private key file that was created with its certificate file  
In the sample app, this value is passed in from the command line\.

`ca_filepath`  
The path to the Root CA file\. Required only if the MQTT server uses a certificate that's not already in your trust store\.  
In the sample app, this value is passed in from the command line\.

`client_bootstrap`  
The common runtime object that handles socket communication activities  
In the sample app, this object is instantiated just prior to the call to `mqtt_connection_builder.mtls_from_path`\.

`on_connection_interrupted``on_connection_resumed`  
The callback functions to call when the device’s connection is interrupted and resumed

`client_id`  
The ID that uniquely identifies this device in the AWS Region  
In the sample app, this value is passed in from the command line\.

`clean_session`  
Whether to start a new persistent session, or, if one is present, reconnect to an existing one

`keep_alive_secs`  
The keep alive value, in seconds, to send in the `CONNECT` request\. A ping will automatically be sent at this interval\. The server assumes the connection is lost if it doesn't receive a ping after 1\.5 times this value\.

#### MQTT over WSS<a name="sdk-tutorials-explore-mqtt-wss"></a>

The `pubsub.py` sample calls `websockets_with_default_aws_signing` \(shown here\) in the [https://github.com/awslabs/aws-crt-python/blob/89207bcf1387177034e02fe29e8e469ca45e39b7/awscrt/awsiot_mqtt_connection_builder.py](https://github.com/awslabs/aws-crt-python/blob/89207bcf1387177034e02fe29e8e469ca45e39b7/awscrt/awsiot_mqtt_connection_builder.py) to establish a connection with AWS IoT Core using the MQTT protocol over WSS\. `websockets_with_default_aws_signing` creates an MQTT connection over WSS using [Signature V4](https://docs.aws.amazon.com/general/latest/gr/signature-version-4.html) to authenticate the device\.

```
mqtt_connection = mqtt_connection_builder.websockets_with_default_aws_signing(
    endpoint=args.endpoint,
    client_bootstrap=client_bootstrap,
    region=args.signing_region,
    credentials_provider=credentials_provider,
    websocket_proxy_options=proxy_options,
    ca_filepath=args.root_ca,
    on_connection_interrupted=on_connection_interrupted,
    on_connection_resumed=on_connection_resumed,
    client_id=args.client_id,
    clean_session=False,
    keep_alive_secs=6
)
```

`endpoint`  
Your AWS account’s IoT device endpoint  
In the sample app, this value is passed in from the command line\.

`client_bootstrap`  
The common runtime object that handles socket communication activities  
In the sample app, this object is instantiated just prior to the call to `mqtt_connection_builder.websockets_with_default_aws_signing`\.

`region`  
The AWS signing Region used by Signature V4 authentication\. In `pubsub.py`, it passes the parameter entered in the command line\.  
In the sample app, this value is passed in from the command line\.

`credentials_provider`  
The AWS credentials provided to use for authentication  
In the sample app, this object is instantiated just prior to the call to `mqtt_connection_builder.websockets_with_default_aws_signing`\.

`websocket_proxy_options`  
HTTP proxy options, if using a proxy host  
In the sample app, this value is initialized just prior to the call to `mqtt_connection_builder.websockets_with_default_aws_signing`\.

`ca_filepath`  
The path to the Root CA file\. Required only if the MQTT server uses a certificate that's not already in your trust store\.  
In the sample app, this value is passed in from the command line\.

`on_connection_interrupted``on_connection_resumed`  
The callback functions to call when the device’s connection is interrupted and resumed

`client_id`  
The ID that uniquely identifies this device in the AWS Region\.  
In the sample app, this value is passed in from the command line\.

`clean_session`  
Whether to start a new persistent session, or, if one is present, reconnect to an existing one

`keep_alive_secs`  
The keep alive value, in seconds, to send in the `CONNECT` request\. A ping will automatically be sent at this interval\. The server assumes the connection is lost if it doesn't receive a ping after 1\.5 times this value\.

#### HTTPS<a name="sdk-tutorials-explore-https"></a>

What about HTTPS? AWS IoT Core supports devices that publish HTTPS requests\. From a programming perspective, devices send HTTPS requests to AWS IoT Core as would any other application\. For an example of a Python program that sends an HTTP message from a device, see the [HTTPS code example](http.md#codeexample) using Python’s `requests` library\. This example sends a message to AWS IoT Core using HTTPS such that AWS IoT Core interprets it as an MQTT message\.

While AWS IoT Core supports HTTPS requests from devices, be sure to review the information about [Choosing a protocol for your device communication](protocols.md#protocol-selection) so that you can make an informed decision on which protocol to use for your device communications\.

### Persistent sessions<a name="sdk-tutorials-explore-persistent"></a>

In the sample app, setting the `clean_session` parameter to `False` indicates that the connection should be persistent\. In practice, this means that the connection opened by this call reconnects to an existing persistent session, if one exists\. Otherwise, it creates and connects to a new persistent session\.

With a persistent session, messages that are sent to the device are stored by the message broker while the device is not connected\. When a device reconnects to a persistent session, the message broker sends to the device any stored messages to which it has subscribed\.

Without a persistent session, the device will not receive messages that are sent while the device isn't connected\. Which option to use depends on your application and whether messages that occur while a device is not connected must be communicated\. For more information, see [Using MQTT persistent sessions](mqtt.md#mqtt-persistent-sessions)\.

### Quality of Service<a name="sdk-tutorials-explore-qos"></a>

When the device publishes and subscribes to messages, the preferred Quality of Service \(QoS\) can be set\. AWS IoT supports QoS levels 0 and 1 for publish and subscribe operations\. For more information about QoS levels in AWS IoT, see [MQTT Quality of Service \(QoS\) options](mqtt.md#mqtt-qos)\.

The AWS CRT runtime for Python defines these constants for the QoS levels that it supports:


**Python Quality of Service levels**  

| MQTT QoS level | Python symbolic value used by SDK | Description | 
| --- | --- | --- | 
| QoS level 0 | mqtt\.QoS\.AT\_MOST\_ONCE | Only one attempt to send the message will be made, whether it is received or not\. The message might not be sent at all, for example, if the device is not connected or there's a network error\. | 
| QoS level 1 | mqtt\.QoS\.AT\_LEAST\_ONCE | The message is sent repeatedly until a PUBACK acknowledgement is received\. | 

In the sample app, the publish and subscribe requests are made with a QoS level of 1 \(`mqtt.QoS.AT_LEAST_ONCE`\)\. 
+ 

**QoS on publish**  
When a device publishes a message with QoS level 1, it sends the message repeatedly until it receives a `PUBACK` response from the message broker\. If the device isn't connected, the message is queued to be sent after it reconnects\.
+ 

**QoS on subscribe**  
When a device subscribes to a message with QoS level 1, the message broker saves the messages to which the device is subscribed until they can be sent to the device\. The message broker resends the messages until it receives a `PUBACK` response from the device\.

### Message publish<a name="sdk-tutorials-explore-publish"></a>

After successfully establishing a connection to AWS IoT Core, devices can publish messages\. The `pubsub.py` sample does this by calling the `publish` method of the `mqtt_connection` object\.

```
mqtt_connection.publish(
    topic=args.topic,
    payload=message,
    qos=mqtt.QoS.AT_LEAST_ONCE
)
```

`topic`  
The message's topic name that identifies the message  
In the sample app, this is passed in from the command line\.

`payload`  
The message payload formatted as a string \(for example, a JSON document\)  
In the sample app, this is passed in from the command line\.  
A JSON document is a common payload format, and one that is recognized by other AWS IoT services; however, the data format of the message payload can be anything that the publishers and subscribers agree upon\. Other AWS IoT services, however, only recognize JSON, and CBOR, in some cases, for most operations\.

`qos`  
The QoS level for this message

### Message subscription<a name="sdk-tutorials-explore-subscribe"></a>

To receive messages from AWS IoT and other services and devices, devices subscribe to those messages by their topic name\. Devices can subscribe to individual messages by specifying a [topic name](topics.md#topicnames), and to a group of messages by specifying a [topic filter](topics.md#topicfilters), which can include wild card characters\. The `pubsub.py` sample uses the code shown here to subscribe to messages and register the callback functions to process the message after it’s received\.

```
subscribe_future, packet_id = mqtt_connection.subscribe(
    topic=args.topic,
    qos=mqtt.QoS.AT_LEAST_ONCE,
    callback=on_message_received
)
subscribe_result = subscribe_future.result()
```

`topic`  
The topic to subscribe to\. This can be a topic name or a topic filter\.  
In the sample app, this is passed in from the command line\.

`qos`  
Whether the message broker should store these messages while the device is disconnected\.  
A value of `mqtt.QoS.AT_LEAST_ONCE` \(QoS level 1\), requires a persistent session to be specified \(`clean_session=False`\) when the connection is created\.

`callback`  
The function to call to process the subscribed message\.

The `mqtt_connection.subscribe` function returns a future and a packet ID\. If the subscription request was initiated successfully, the packet ID returned is greater than 0\. To make sure the subscription was received and registered by the message broker, you must wait for the result of the asynchronous operation to return, as shown in the code example\.

**The callback function**  
The callback in the `pubsub.py` sample processes the subscribed messages as the device receives them\.

```
def on_message_received(topic, payload, **kwargs):
    print("Received message from topic '{}': {}".format(topic, payload))
    global received_count
    received_count += 1
    if received_count == args.count:
        received_all_event.set()
```

`topic`  
The message’s topic  
This is the specific topic name of the message received, even if you subscribed to a topic filter\.

`payload`  
The message payload  
The format for this is application specific\.

`kwargs`  
Possible additional arguments as described in [https://awslabs.github.io/aws-crt-python/api/mqtt.html#awscrt.mqtt.Connection.subscribe](https://awslabs.github.io/aws-crt-python/api/mqtt.html#awscrt.mqtt.Connection.subscribe)\.

In the `pubsub.py` sample, `on_message_received` only displays the topic and its payload\. It also counts the messages received to end the program after the limit is reached\.

Your app would evaluate the topic and the payload to determine what actions to perform\.

### Device disconnection and reconnection<a name="sdk-tutorials-explore-connect"></a>

The `pubsub.py` sample includes callback functions that are called when the device is disconnected and when the connection is re\-established\. What actions your device takes on these events is application specific\.

When a device connects for the first time, it must subscribe to topics to receive\. If a device's session is present when it reconnects, its subscriptions are restored, and any stored messages from those subscriptions are sent to the device after it reconnects\.

If a device's session no longer exists when it reconnects, it must resubscribe to its subscriptions\. Persistent sessions have a limited lifetime and can expire when the device is disconnected for too long\.

## Connect your device and communicate with AWS IoT Core<a name="sdk-tutorials-experiment"></a>

This section presents some exercises to help you explore different aspects of connecting your device to AWS IoT Core\. For these exercises, you’ll use the [MQTT test client](https://console.aws.amazon.com/iot/home#/test) in the AWS IoT console to see what your device publishes and to publish messages to your device\. These exercises use the [https://github.com/aws/aws-iot-device-sdk-python-v2/blob/master/samples/pubsub.py](https://github.com/aws/aws-iot-device-sdk-python-v2/blob/master/samples/pubsub.py) sample from the [AWS IoT Device SDK v2 for Python](https://github.com/aws/aws-iot-device-sdk-python-v2/tree/master/samples#sample-apps-for-the-aws-iot-device-sdk-v2-for-python) and build on your experience with [Getting started with AWS IoT Core](iot-gs.md) tutorials\. 

**Topics**
+ [Subscribe to wild card topic filters](#sdk-tutorials-experiment-wild)
+ [Process topic filter subscriptions](#sdk-tutorials-experiment-process)
+ [Publish messages from your device](#sdk-tutorials-experiment-publish)

For these exercises, you'll start from the `pubsub.py` sample program\.

**Note**  
These exercises assume that you completed the [Getting started with AWS IoT Core](iot-gs.md) tutorials and use the terminal window for your device from that tutorial\.

### Subscribe to wild card topic filters<a name="sdk-tutorials-experiment-wild"></a>

In this exercise, you’ll modify the command line used to call `pubsub.py` to subscribe to a wild card topic filter and process the messages received based on the message’s topic\.

#### Exercise procedure<a name="sdk-tutorials-experiment-wild-steps"></a>

For this exercise, imagine that your device contains a temperature control and a light control\. It uses these topic names to identify the messages about them\.

1. Before starting the exercise, try running this command from the [Getting started with AWS IoT Core](iot-gs.md) tutorials on your device to make sure everything is ready for the exercise\.

   ```
   cd ~/aws-iot-device-sdk-python-v2/samples
   python3 pubsub.py --topic topic_1 --root-ca ~/certs/Amazon-root-CA-1.pem --cert ~/certs/device.pem.crt --key ~/certs/private.pem.key —endpoint your-iot-endpoint
   ```

   You should see the same output as you saw in the [Getting started tutorial](connecting-to-existing-device.md#gs-device-node-app-run)\.

1. For this exercise, change these command line parameters\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/iot/latest/developerguide/sdk-tutorials.html)

   Making these changes to the initial command line results in this command line\. Enter this command in the terminal window for your device\.

   ```
   python3 pubsub.py --message "" --count 2 --topic device/+/details --root-ca ~/certs/Amazon-root-CA-1.pem --cert ~/certs/device.pem.crt --key ~/certs/private.pem.key —endpoint your-iot-endpoint
   ```

   The program should display something like this:

   ```
   Connecting to a3qexamplesffp-ats.iot.us-west-2.amazonaws.com with client ID 'test-24d7cdcc-cc01-458c-8488-2d05849691e1'...
   Connected!
   Subscribing to topic 'device/+/details'...
   Subscribed with QoS.AT_LEAST_ONCE
   Waiting for all messages to be received...
   ```

   If you see something like this on your terminal, your device is ready and listening for messages where the topic names start with `topic-1/` and end with `/detail`\. So, let's test that\.

1. Here are a couple of messages that your device might receive\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/iot/latest/developerguide/sdk-tutorials.html)

1. Using the MQTT test client in the AWS IoT console, send the messages described in the previous step to your device\.

   1. Open the [MQTT test client](https://console.aws.amazon.com/iot/home#/test) in the AWS IoT console\.

   1. In **Subscribe to a topic**, in the **Subscription topic field**, enter the topic filter: **device/\+/details**, and then choose **Subscribe to topic**\.

   1. In the **Subscriptions** column of the MQTT test client, choose **device/\+/details**\.

   1. For each of the topics in the preceding table, do the following in the MQTT test client:

      1. In **Publish**, enter the value from the **Topic name** column in the table\.

      1. In the message payload field below the topic name, enter the value from the **Message payload** column in the table\.

      1. Watch the terminal window where `pubsub.py` is running and, in the MQTT test client, choose **Publish to topic**\.

      You should see that the message was received by `pubsub.py` in the terminal window\.

#### Exercise result<a name="sdk-tutorials-experiment-wild-result"></a>

With this, `pubsub.py`, subscribed to the messages using a wild card topic filter, received them, and displayed them in the terminal window\. Notice how you subscribed to a single topic filter, and the callback function was called to process messages having two distinct topics\.

### Process topic filter subscriptions<a name="sdk-tutorials-experiment-process"></a>

Building on the previous exercise, modify the `pubsub.py` sample app to evaluate the message topics and process the subscribed messages based on the topic\.

#### Exercise procedure<a name="sdk-tutorials-experiment-process-steps"></a>

**To evaluate the message topic**

1. Copy `pubsub.py` to `pubsub2.py`\.

1. Open `pubsub2.py` in your favorite text editor or IDE\.

1. In `pubsub2.py`, find the `on_message_received` function\.

1. In `on_message_received`, insert the following code after the line that starts with `print("Received message` and before the line that starts with `global received_count`\.

   ```
       topic_parsed = False
       if "/" in topic:
           parsed_topic = topic.split("/")
           if len(parsed_topic) == 3:
               # this topic has the correct format
               if (parsed_topic[0] == 'device') and (parsed_topic[2] == 'details'):
                   # this is a topic we care about, so check the 2nd element
                   if (parsed_topic[1] == 'temp'):
                       print("Received temperature request: {}".format(payload))
                       topic_parsed = True
                   if (parsed_topic[1] == 'light'):
                       print("Received light request: {}".format(payload))
                       topic_parsed = True
       if not topic_parsed:
           print("Unrecognized message topic.")
   ```

1. Save your changes and run the modified program by using this command line\.

   ```
   python3 pubsub2.py --message "" --count 2 --topic device/+/details --root-ca ~/certs/Amazon-root-CA-1.pem --cert ~/certs/device.pem.crt --key ~/certs/private.pem.key —endpoint your-iot-endpoint
   ```

1. In the AWS IoT console, open the [MQTT test client](https://console.aws.amazon.com/iot/home#/test)\.

1. In **Subscribe to a topic**, in the **Subscription topic field**, enter the topic filter: **device/\+/details**, and then choose **Subscribe to topic**\.

1. In the **Subscriptions** column of the MQTT test client, choose **device/\+/details**\.

1. For each of the topics in this table, do the following in the MQTT test client:    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/iot/latest/developerguide/sdk-tutorials.html)

   1. In **Publish**, enter the value from the **Topic name** column in the table\.

   1. In the message payload field below the topic name, enter the value from the **Message payload** column in the table\.

   1. Watch the terminal window where `pubsub.py` is running and, in the MQTT test client, choose **Publish to topic**\.

   You should see that the message was received by `pubsub.py` in the terminal window\.

You should see something similar to this in your terminal window\.

```
Connecting to a3qexamplesffp-ats.iot.us-west-2.amazonaws.com with client ID 'test-af794be0-7542-45a0-b0af-0b0ea7474517'...
Connected!
Subscribing to topic 'device/+/details'...
Subscribed with QoS.AT_LEAST_ONCE
Waiting for all messages to be received...
Received message from topic 'device/light/details': b'{ "desiredLight": 100, "currentLight": 50 }'
Received light request: b'{ "desiredLight": 100, "currentLight": 50 }'
Received message from topic 'device/temp/details': b'{ "desiredTemp": 20, "currentTemp": 15 }'
Received temperature request: b'{ "desiredTemp": 20, "currentTemp": 15 }'
2 message(s) received.
Disconnecting...
Disconnected!
```

#### Exercise result<a name="sdk-tutorials-experiment-process-result"></a>

In this exercise, you added code so the sample app would recognize and process multiple messages in the callback function\. With this, your device could receive messages and act on them\.

Another way for your device to receive and process multiple messages would be to subscribe to different messages separately and assign each subscription to its own callback function\.

### Publish messages from your device<a name="sdk-tutorials-experiment-publish"></a>

You can use the pubsub\.py sample app to publish messages from your device\. While it will publish messages as it is, the messages can't be read as JSON documents\. This exercise modifies the sample app to be able to publish JSON documents in the message payload that can be read by AWS IoT Core\.

#### Exercise procedure<a name="sdk-tutorials-experiment-publish-steps"></a>

In this exercise, the following message will be sent with the `device/data` topic\.

```
{
    "timestamp": 1601048303,
    "sensorId": 28,
    "sensorData": [
        {
        "sensorName": "Wind speed",
        "sensorValue": 34.2211224
        }
    ]
}
```

**To prepare your MQTT test client to monitor the messages from this exercise**

1. In **Subscribe to a topic**, in the **Subscription topic field**, enter the topic filter: **device/data**, and then choose **Subscribe to topic**\.

1. In the **Subscriptions** column of the MQTT test client, choose **device/data**\.

1. Leave the MQTT test client window open to wait for messages from your device\.

**To send JSON documents with the pubsub\.py sample app**

1. On your device, copy `pubsub.py` to `pubsub3.py`\.

1. Edit `pubsub3.py` to change how it formats the messages it publishes\.

   1. Open `pubsub3.py` in a text editor\.

   1. Locate this line of code:

      `message = "{} [{}]".format(args.message, publish_count)`

   1. Change it to:

      `message = "{}".format(args.message)`

   1. Save your changes\.

1. On your device, run this command to send the message two times\.

   ```
   python3 pubsub3.py  --root-ca ~/certs/Amazon-root-CA-1.pem  --cert ~/certs/device.pem.crt  --key ~/certs/private.pem.key  --topic device/data  --count 2 --message '{"timestamp":1601048303,"sensorId":28,"sensorData":[{"sensorName":"Wind speed","sensorValue":34.2211224}]}'  --endpoint your-iot-endpoint
   ```

1. In the MQTT test client, check to see that it has interpreted and formatted the JSON document in the message payload, such as this:  
![\[Image showing how a JSON message payload is displayed in the MQTT client of the AWS IoT console.\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/mqtt-test-client-output.png)

By default, `pubsub3.py` also subscribes to the messages it sends\. You should see that it received the messages in the app’s output\. The terminal window should look something like this\.

```
Connecting to a3qj468xinsffp-ats.iot.us-west-2.amazonaws.com with client ID 'test-5cff18ae-1e92-4c38-a9d4-7b9771afc52f'...
Connected!
Subscribing to topic 'device/data'...
Subscribed with QoS.AT_LEAST_ONCE
Sending 2 message(s)
Publishing message to topic 'device/data': {"timestamp":1601048303,"sensorId":28,"sensorData":[{"sensorName":"Wind speed","sensorValue":34.2211224}]}
Received message from topic 'device/data': b'{"timestamp":1601048303,"sensorId":28,"sensorData":[{"sensorName":"Wind speed","sensorValue":34.2211224}]}'
Publishing message to topic 'device/data': {"timestamp":1601048303,"sensorId":28,"sensorData":[{"sensorName":"Wind speed","sensorValue":34.2211224}]}
Received message from topic 'device/data': b'{"timestamp":1601048303,"sensorId":28,"sensorData":[{"sensorName":"Wind speed","sensorValue":34.2211224}]}'
2 message(s) received.
Disconnecting...
Disconnected!
```

#### Exercise result<a name="sdk-tutorials-experiment-publish-result"></a>

With this, your device can generate messages to send to AWS IoT Core to test basic connectivity and provide device messages for AWS IoT Core to process\. For example, you could use this app to send test data from your device to test AWS IoT rule actions\.

## Review the results<a name="sdk-tutorials-conclusion"></a>

The examples in this tutorial gave you hands\-on experience with the basics of how devices can communicate with AWS IoT Core—a fundamental part of your AWS IoT solution\. When your devices are able to communicate with AWS IoT Core, they can pass messages to AWS services and other devices on which they can act\. Likewise, AWS services and other devices can process information that results in messages sent back to your devices\.

When you are ready to explore AWS IoT Core further, try these tutorials:
+ [Send an Amazon SNS notification](iot-sns-rule.md)
+ [Store device data in a DynamoDB table](iot-ddb-rule.md)
+ [Format a notification by using an AWS Lambda function](iot-lambda-rule.md)