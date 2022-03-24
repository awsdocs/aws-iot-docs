# Connect your Sidewalk device and view uplink metadata format<a name="iot-sidewalk-connect-uplink-metadata"></a>

After you've added your Sidewalk credentials and added the destination, you can provision your Sidewalk endpoints and connect your device\.

## Connect your Sidewalk device<a name="iot-sidewalk-connect-device"></a>

You can provision your device as a Sidewalk endpoint by generating the device certificates and application server certificates from the [Sidewalk Developer Service \(SDS\) console](http://developer.amazon.com/acs-devices/console/Sidewalk)\. For more information, see [Provision and Configure your Sidewalk Endpoints](https://developer.amazon.com/acs-devices/console/sidewalk/docs/group__qsg__step5.html)\.

After you connect your device, you'll see your Sidewalk device in the [ Devices](https://console.aws.amazon.com/iot/home#/wireless/devices?tab=sidewalk) page of the AWS IoT console, on the **Sidewalk** tab\. When your device is connected and starts sending data, you'll see the date and time for the **Last uplink received at** field\. 

## View format of uplink messages<a name="iot-sidewalk-uplink-metadata"></a>

After you've connected your device, you can subscribe to the topic \(for example, `project/sensor/observed`\) that you specified when creating the Sidewalk destination rule, and observe uplink messages from the device\. To subscribe to the topic, go to the [MQTT test client](https://console.aws.amazon.com/iot/home#/test) on the **Test** page of the AWS IoT console, enter the topic name \(for example, `project/sensor/observed`\), and then choose **Subscribe**\.

The following example shows the format of the uplink messages that are sent from Sidewalk devices to AWS IoT\. The `WirelessMetadata` contains metadata about the message request\.

```
{
   "PayloadData":"ZjRlNjY1ZWNlNw==",
   "WirelessDeviceId":"6dc562bf-31c5-37e0-b230-716ea8148c3d",
   "TransmitMode": "0",
   "WirelessMetadata":{
      "Sidewalk":{
         "CmdExStatus":"Cmd",
         "SidewalkId":"device-id",
         "Seq":0,
         "MessageType":"messageType"
      }
```

The following table shows a definition of the different parameters in the uplink metadata\. The `device-id` is the ID of the wireless device, such as `ABCDEF1234` and the `messageType` is the type of upllink message that's received from the device\.


**Sidewalk uplink metadata parameters**  

| Parameter | Description | Type | Required | 
| --- | --- | --- | --- | 
| PayloadData |  The message payload that is sent from the wireless device\.   | String | Yes | 
| WirelessDeviceID | The identifier of the wireless device that's sending the data | String | Yes | 
| TransmitMode |  The transmission mode for data that is sent from the wireless device\. Can be `0` for `unconfirmed` mode, `1` for `confirmed`, and `2` for `unused`\.   | Integer | Yes | 
| Sidewalk\.CmdExStatus |  Command runtime status\. Response\-type messages shall include the status code, `COMMAND_EXEC_STATUS_SUCCESS`\. However, notifications might not include the status code\.  | Enumeration | No | 
| Sidewalk\.NackExStatus |  Response nack status, which can be `RADIO_TX_ERROR` or `MEMORY_ERROR`\.   | Array of strings | No | 