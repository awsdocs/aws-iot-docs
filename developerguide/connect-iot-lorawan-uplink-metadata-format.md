# View format of uplink messages sent from LoRaWAN devices<a name="connect-iot-lorawan-uplink-metadata-format"></a>

After you've connected your LoRaWAN device to AWS IoT Core for LoRaWAN, you can observe the format of the uplink message that you'll receive from your wireless device\.

## Before you can observe the uplink messages<a name="connect-iot-lorawan-uplink-metadata-prerequisites"></a>

You must have onboarded your wireless device and connected your device to AWS IoT so that it can transmit and receive data\. For information about onboarding your device to AWS IoT Core for LoRaWAN, see [Onboard your devices to AWS IoT Core for LoRaWAN](connect-iot-lorawan-onboard-end-devices.md)\.

## What do the uplink messages contain?<a name="connect-iot-lorawan-uplink-metadata-contains"></a>

LoRaWAN devices connect to AWS IoT Core for LoRaWAN by using LoRaWAN gateways\. The uplink message that you receive from the device will contain the following information\.
+ Payload data that corresponds to the encrypted payload message that is sent from the wireless device\.
+ Wireless metadata that includes:
  + Device information such as DevEui, the data rate, and the frequency channel in which the device is operating\.
  + Optional additional parameters and the gateway information for gateways that are connected to the device\. The gateway parameters include the gateway's EUI, the SNR, and RSSi\.

  By using the wireless metadata, you can obtain useful information about the wireless device and the data that is transmitted between your device and AWS IoT\. For example, you can use the `AckedMessageId` parameter to check whether the last confirmed downlink message has been received by the device\. Optionally, if you choose to include the gateway information, you can identify whether you want to switch to a stronger gateway channel that's closer to your device\.

## How to observe the uplink messages?<a name="connect-iot-lorawan-uplink-metadata-observe"></a>

After you've onboarded your device, you can use the [MQTT test client](https://console.aws.amazon.com/iot/home#/test) on the **Test** page of the AWS IoT console to subscribe to the topic that you specified when creating your destination\. You'll start to see messages after your device is connected and starts sending payload data\.

This diagram identifies the key elements in a LoRaWAN system connected to AWS IoT Core for LoRaWAN, which shows the primary data plane and how data flows through the system\.

![\[Image showing how AWS IoT Core for LoRaWAN data is passed from a wireless device to AWS IoT and other services.\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/iot-lorawan-data-flow.png)

When the wireless device starts sending uplink data, AWS IoT Core for LoRaWAN wraps the wireless metadata information with the payload and then sends it to your AWS applications\.

## Uplink message example<a name="connect-iot-lorawan-uplink-metadata-example"></a>

The following example shows the format of the uplink message received from your device\.

**Note**  
If your devices send an uplink message without a value for `Fport`, AWS IoT Core for LoRaWAN will add the value 225 to the `Fport` in the uplink message received\.

```
{
    "WirelessDeviceId": "5b58245e-146c-4c30-9703-0ca942e3ff35", 
    "PayloadData": "Cc48AAAAAAAAAAA=",    
    "WirelessMetadata":
    {
        "LoRaWAN":
        {
            "ADR": false,
            "Bandwidth": 125,
            "ClassB": false,
            "CodeRate": "4/5",
            "DataRate": "0",
            "DevAddr": "00b96cd4",
            "DevEui": "58a0cb000202c99",            
            "FOptLen": 2,
            "FCnt": 1,
            "Fport": 136,   
            "Frequency": "868100000",     
            "Gateways": [
             {
                    "GatewayEui": "80029cfffe5cf1cc",      
                    "Snr": -29,
                    "Rssi": 9.75
             }
             ],  
            "MIC": "7255cb07",  
            "MType": "UnconfirmedDataUp",
            "Major": "LoRaWANR1",
            "Modulation": "LORA", 
            "PolarizationInversion": false,    
            "SpreadingFactor": 12,                         
            "Timestamp": "2021-05-03T03:24:29Z"
            
        }
    }
}
```

The following table shows a description of fields used in the uplink metadata:


**LoRaWAN uplink message fields**  

| Parameter | Description | Type | Required | 
| --- | --- | --- | --- | 
| WirelessDeviceID | ID of the wireless device sending the data\. | String | Yes | 
| PayloadData | The binary message received from the device, encoded in base64\. | String | Yes | 
| WirelessMetadata | Metadata about the LoRaWAN device and the message request\. This includes information such as the device identifiers, data and code rate, the message timestamp, whether ADR \(adaptive data rate\) is enabled, and the gateway metadata\. | Enumeration | No | 

### Exclude gateway metadata from uplink metadata<a name="connect-iot-lorawan-uplink-metadata-example2"></a>

If you want to exclude the gateway metadata information from your uplink metadata, disable the **AddGwMetadata** parameter when you create the service profile\. For information about disabling this parameter, see [Add service profiles](connect-iot-lorawan-define-profiles.md#connect-iot-lorawan-service-profiles)\.

In this case, you won't see the `Gateways` section in the uplink metadata, as illustrated in the following example\.

```
{  
    "WirelessDeviceId": "0d9a439b-e77a-4573-a791-49d5c0f4db95",
    "PayloadData": "AAAAAAAA//8=",
    "WirelessMetadata": {
        "LoRaWAN": {
            "ClassB": false,
            "CodeRate": "4/5",
            "DataRate": "1",
            "DevAddr": "01920f27",
            "DevEui": "ffffff10000163b0",
            "FCnt": 1,
            "FPort": 5,
            "Timestamp": "2021-04-29T05:19:43.646Z"
    }
  }
}
```