# Choosing gateways to receive the LoRaWAN downlink data traffic<a name="connect-iot-lorawan-gateway-participate"></a>

When you send a downlink message from AWS IoT Core for LoRaWAN to your device, you can choose the gateways you want to use for the downlink data traffic\. You can specify an individual gateway or choose from a list of gateways to receive the downlink traffic\.

## How to specify the gateway list<a name="connect-iot-lorawan-participate-how"></a>

You can specify an individual gateway or the list of gateways to use when sending a downlink message from AWS IoT Core for LoRaWAN to your device using the [https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_SendDataToWirelessDevice.html](https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_SendDataToWirelessDevice.html) API operation\. When you invoke the API operation, specify the following parameters using the `ParticipatingGateways` object for your gateways\. 

**Note**  
The list of gateways you want to use isn't available in the AWS IoT console\. You can specify this list of gateways to use only when using the `SendDataToWirelessDevice` API operation or the CLI\.
+ `DownlinkMode`: Indicates whether to send the downlink message in sequential mode or concurrent mode\. For class A devices, specify `UsingUplinkGateway` to use only the chosen gateways from the previous uplink message transmission\.
+ `GatewayList`: The list of gateways that you want to use for sending the downlink data traffic\. The downlink payload will be sent to the specified gateways with the specified frequency\. This is indicated using a list of `GatewayListItem` objects, that consists of `GatewayId` and `DownlinkFrequency` pairs\.
+ `TransmissionInterval`: The duration of time for which AWS IoT Core for LoRaWAN will wait before transmitting the payload to the next gateway\.

**Note**  
You can specify this list of gateways to use only when sending the downlink message to a class B or a class C wireless device\. If you use a class A device, the gateway that you chose when sending the uplink message will be used when a downlink message is sent to the device\.

The following example shows how you specify these parameters for the gateway\. The `input.json` file will contain additional details\. For more information about sending a downlink message using the `SendDataToWirelessDevice` API operation, see [Perform downlink queue operations by using the API](connect-iot-lorawan-downlink-queue.md#connect-iot-lorawan-downlink-queue-api)\.

**Note**  
The parameters for specifying the list of participating gateways aren't available when you send a downlink message from AWS IoT Core for LoRaWAN using the AWS IoT console\.

```
aws iotwireless send-data-to-wireless-device \
    --id "11aa5eae-2f56-4b8e-a023-b28d98494e49" \
    --transmit-mode "1" \
    --payload-data "SGVsbG8gVG8gRGV2c2lt" \
    --cli-input-json file://input.json
```

The following shows the contents of the `input.json` file\.

**Contents of input\.json**

```
{
    "WirelessMetadata": {
        "LoRaWAN": {
            "FPort": "1", 
            "ParticipatingGateways": {
                "DownlinkMode": "SEQUENTIAL", 
                "TransmissionInterval": 1200,
                "GatewayList": [
                    {
                        "DownlinkFrequency": 100000000,
                        "GatewayID": a01b2c34-d44e-567f-abcd-0123e445663a
                    },
                    {
                        "DownlinkFrequency": 100000101,
                        "GatewayID": 12345678-a1b2-3c45-67d8-e90fa1b2c34d
                    }
                ]
            }
        }
    }
}
```

The output of running this command generates a `MessageId` for the downlink message\. In some cases, even if you receive the `MessageId`, packets can get dropped\. For more information about how you can resolve the error, see [Troubleshoot downlink message queue errors](connect-iot-lorawan-downlink-queue.md#connect-iot-lorawan-downlink-queue-troubleshoot)\.

```
{
    MessageId: "6011dd36-0043d6eb-0072-0008"
}
```

## Get information about the list of participating gateways<a name="connect-iot-lorawan-participate-get"></a>

You can get information about the list of gateways that are participating in receiving the downlink message by listing messages in the downlink queue\. To list messages, use the [https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_ListQueuedMessages.html](https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_ListQueuedMessages.html) API\.

```
aws iotwireless list-queued-messages \
    --wireless-device-type "LoRaWAN"
```

Running this command returns information about the messages in the queue and their parameters\.