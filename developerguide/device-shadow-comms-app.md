# Using shadows in apps and services<a name="device-shadow-comms-app"></a>

This section describes how an app or service interacts with the AWS IoT Device Shadow service\. This example assumes the app or service is interacting only with the shadow and, through the shadow, the device\. This example doesn't include any management actions, such as creating or deleting shadows\. 

This example uses the AWS IoT Device Shadow service's REST API to interact with shadows\. Unlike the example used in [Using shadows in devices](device-shadow-comms-device.md), which uses a publish/subscribe communications model, this example uses the request/response communications model of the REST API\. This means the app or service must make a request before it can receive a response from AWS IoT\. A disadvantage of this model, however, is that it does not support notifications\. If your app or service requires timely notifications of device state changes, consider the MQTT or MQTT over WSS protocols, which support the publish/subscribe communication model, as described in [Using shadows in devices](device-shadow-comms-device.md)\.

**Important**  
Make sure that your app's or service's use of the shadows is consistent with and supported by the corresponding implementations in your devices\. Consider, for example, how shadows are created, updated, and deleted, and how updates are handled in the device and the apps or services that access the shadow\. Your design should clearly specify how the device's state is updated and reported, and how your apps and services interact with the device and its shadows\.

The REST API's URL for a named shadows is:

```
https://endpoint/things/thingName/shadow?name=shadowName
```

and for an unnamed shadow:

```
https://endpoint/things/thingName/shadow
```

where:

endpoint  
The endpoint returned by the CLI command:  

```
aws iot describe-endpoint --endpoint-type IOT:Data-ATS
```

thingName  
The name of the thing object to which the shadow belongs

shadowName  
The name of the named shadow\. This parameter is not used with unnamed shadows\.

## Initializing the app or service on connection to AWS IoT<a name="device-shadow-comms-app-first-connect"></a>

When the app first connects to AWS IoT, it should send an HTTP GET request to the URLs of the shadows it uses to get the current state of the shadows it's using\. This allows it to sync the app or service to the shadow\.

## Processing state changes while the app or service is connected to AWS IoT<a name="device-shadow-comms-app-while-connected"></a>

While the app or service is connected to AWS IoT, it can query the current state periodically by sending an HTTP GET request on the URLs of the shadows it uses\.

When an end user interacts with the app or service to change the state of the device, the app or service can send an HTTP POST request to the URLs of the shadows it uses to update the `desired` state of the shadow\. This request returns the change that was accepted, but you might have to poll the shadow by making HTTP GET requests until the device has updated the shadow with its new state\.

## Detecting a device is connected<a name="thing-connection"></a>

To determine if a device is currently connected, include a `connected` property in the shadow document and use an MQTT Last Will and Testament \(LWT\) message to set the `connected` property to `false` if a device is disconnected due to an error\.

**Note**  
MQTT LWT messages sent to AWS IoT reserved topics \(topics that begin with $\) are ignored by the AWS IoT Device Shadow service\. However, they are processed by subscribed clients and by the AWS IoT rules engine, so you will need to create an LWT message that is sent to a non\-reserved topic and a rule that republishes the MQTT LWT message as a shadow update message to the shadow's reserved update topic, `ShadowTopicPrefix/update`\. 

**To send the Device Shadow service an LWT message**

1. Create a rule that republishes the MQTT LWT message on the reserved topic\. The following example is a rule that listens for a messages on the `my/things/myLightBulb/update` topic and republishes it to `$aws/things/myLightBulb/shadow/update`\.

   ```
   {
       "rule": {
       "ruleDisabled": false,
       "sql": "SELECT * FROM 'my/things/myLightBulb/update'",
       "description": "Turn my/things/ into $aws/things/",
       "actions": [
           {
           "republish": {
               "topic": "$$aws/things/myLightBulb/shadow/update",
               "roleArn": "arn:aws:iam:123456789012:role/aws_iot_republish"
               }
           }
        ]
      }
   }
   ```

1. When the device connects to AWS IoT, it registers an LWT message to a non\-reserved topic for the republish rule to recognize\. In this example, that topic is `my/things/myLightBulb/update` and it sets the connected property to `false`\.

   ```
   {
       "state": {        
           "reported": {
               "connected":"false"
           }
       }
   }
   ```

1. After connecting, the device publishes a message on its shadow update topic, `$aws/things/myLightBulb/shadow/update`, to report its current state, which includes setting its `connected` property to `true`\.

   ```
   {
        "state": {        
           "reported": {
               "connected":"true"
           }
       }
   }
   ```

1. Before the device disconnects gracefully, it publishes a message on its shadow update topic, `$aws/things/myLightBulb/shadow/update`, to report its latest state, which include setting its `connected` property to `false`\.

   ```
   {
       "state": {        
           "reported": {
               "connected":"false"
           }
       }
   }
   ```

1. If the device disconnects due to an error, the AWS IoT message broker publishes the device's LWT message on behalf of the device\. The republish rule detects this message and publishes the shadow update message to update the `connected` property of the device shadow\.