# Retaining device state while the device is offline with Device Shadows<a name="iot-shadows-tutorial"></a>

These tutorials show you how to use the AWS IoT Device Shadow service to store and update the state information of a device\. The Shadow document, which is a JSON document, shows the change in the device's state based on the messages published by a device, local app, or service\. In this tutorial, the Shadow document shows the change in the color of a light bulb\. These tutorials also show how the shadow stores this information even when the device is disconnected from the internet, and passes the latest state information back to the device when it comes back online and requests this information\.

We recommend that you try these tutorials in the order they're shown here, starting with the AWS IoT resources you need to create and the necessary hardware setup, which also helps you learn the concepts incrementally\. These tutorials show how to configure and connect a Raspberry Pi device for use with AWS IoT\. If you don't have the required hardware, you can follow these tutorials by adapting them to a device of your choice or by [creating a virtual device with Amazon EC2](creating-a-virtual-thing.md)\.

**Tutorial scenario overview**  
The scenario for these tutorials is a local app or service that changes the color of a light bulb and that publishes its data to reserved shadow topics\. These tutorials are similar to the Device Shadow functionality described in the [interactive getting started tutorial](interactive-demo.md) and are implemented on a Raspberry Pi device\. The tutorials in this section focus on a single, classic shadow while showing how you might accommodate named shadows or multiple devices\.

The following tutorials will help you learn how to use the AWS IoT Device Shadow service\.
+ 

**[Tutorial: Preparing your Raspberry Pi to run the shadow application](create-resources-shadow.md)**  
This tutorial shows how to set up a Raspberry Pi device for connecting with AWS IoT\. You'll also create an AWS IoT policy document and a thing resource, download the certificates, and then attach the policy to that thing resource\. This tutorial takes about 30 minutes to complete\.
+ 

**[Tutorial: Installing the Device SDK and running the sample application for Device Shadows](lightbulb-shadow-application.md)**  
This tutorial shows how to install the required tools, software, and the AWS IoT Device SDK for Python, and then run the sample shadow application\. This tutorial builds on concepts presented in [Connect a Raspberry Pi or another device](connecting-to-existing-device.md) and takes 20 minutes to complete\.
+ 

**[Tutorial: Interacting with Device Shadow using the sample app and the MQTT test client](interact-lights-device-shadows.md)**  
This tutorial shows how you use the `shadow.py` sample app and **AWS IoT console** to observe the interaction between AWS IoT Device Shadows and the state changes of the light bulb\. The tutorial also shows how to send MQTT messages to the Device Shadow's reserved topics\. This tutorial can take 45 minutes to complete\.

**AWS IoT Device Shadow overview**  
A Device Shadow is a persistent, virtual representation of a device that is managed by a [thing resource](iot-thing-management.md) you create in the AWS IoT registry\. The Shadow document is a JSON or a JavaScript notation doc that is used to store and retrieve the current state information for a device\. You can use the shadow to get and set the state of a device over MQTT topics or HTTP REST APIs, regardless of whether the device is connected to the internet\.

A Shadow document contains a `state` property that describes these aspects of the device's state\.
+ `desired`: Apps specify the desired states of device properties by updating the `desired` object\.
+ `reported`: Devices report their current state in the `reported` object\.
+ `delta`: AWS IoT reports differences between the desired and the reported state in the `delta` object\.

Here is an example of a Shadow state document\.

```
{
  "state": {
    "desired": {
      "color": "green"
      },
    "reported": {
      "color": "blue"
      },
    "delta": {
      "color": "green"
      }
   }
}
```

To update a device's Shadow document, you can use the [reserved MQTT topics](reserved-topics.md#reserved-topics-shadow), the [Device Shadow REST APIs](device-shadow-rest-api.md) that support the `GET`, `UPDATE`, and `DELETE` operations with HTTP, and the [AWS IoT CLI](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/iot-data/index.html)\.

In the previous example, say you want to change the `desired` color to `yellow`\. To do this, send a request to the [UpdateThingShadow](device-shadow-rest-api.md#API_UpdateThingShadow) API or publish a message to the [Update](device-shadow-mqtt.md#update-pub-sub-topic) topic, `$aws/things/THING_NAME/shadow/update`\.

```
{
  "state": {
    "desired": {
      "color": yellow
    }
  }
}
```

Updates affect only the fields specified in the request\. After successfully updating the Device Shadow, AWS IoT publishes the new `desired` state to the `delta` topic, `$aws/things/THING_NAME/shadow/delta`\. The Shadow document in this case looks like this:

```
{
  "state": {
    "desired": {
      "color": yellow
    },
    "reported": {
      "color": green
    },
    "delta": {
      "color": yellow
      }
  }
}
```

The new state is then reported to the AWS IoT Device Shadow using the `Update` topic `$aws/things/THING_NAME/shadow/update` with the following JSON message: 

```
{
  "state": {
    "reported": {
      "color": yellow
    }
  }
}
```

If you want to get the current state information, send a request to the [GetThingShadow](device-shadow-rest-api.md#API_GetThingShadow) API or publish an MQTT message to the [Get](device-shadow-mqtt.md#get-pub-sub-topic) topic, `$aws/things/THING_NAME/shadow/get`\.

For more information about using the Device Shadow service, see [AWS IoT Device Shadow service](iot-device-shadows.md)\.

For more information about using Device Shadows in devices, apps, and services, see [Using shadows in devices](device-shadow-comms-device.md) and [Using shadows in apps and services](device-shadow-comms-app.md)\.

For information about interacting with AWS IoT shadows, see [Interacting with shadows](device-shadow-data-flow.md)\.

For information about the MQTT reserved topics and HTTP REST APIs, see [Device Shadow MQTT topics](device-shadow-mqtt.md) and [Device Shadow REST API](device-shadow-rest-api.md)\.