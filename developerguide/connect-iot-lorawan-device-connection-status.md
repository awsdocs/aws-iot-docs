# Connect your LoRaWAN device and verify its connection status<a name="connect-iot-lorawan-device-connection-status"></a>

Before you can check the device connection status, you must have already added your device and connected it to AWS IoT Core for LoRaWAN\. For information about how to add your device, see [Add your wireless device to AWS IoT Core for LoRaWAN](connect-iot-lorawan-end-devices-add.md)\.

After you've added your device, refer to your device's user manual to learn how to initiate sending an uplink message from your LoRaWAN device\.

## Check device connection status using the console<a name="connect-iot-lorawan-device-connection-status-console"></a>

To check the connection status using the console, navigate to the [https://console.aws.amazon.com/iot/home#/wireless/devices](https://console.aws.amazon.com/iot/home#/wireless/devices) page of the AWS IoT console and choose the device you've added\. In the **Details** section of the Wireless devices details page, you'll see the date and time the last uplink was received\.

## Check device connection status using the API<a name="connect-iot-lorawan-device-connection-status-api"></a>

To check the connection status using the API, use the `GetWirelessDeviceStatistics` API\. This API doesn't have a request body and only contains a response body that shows when the last uplink was received\.

```
HTTP/1.1 200
Content-type: application/json

{  
  “LastUplinkReceivedAt”: “2021-03-24T23:13:08.476015749Z”,
  "LoRaWAN": {
        "DataRate": 5,
        "DevEui": "647fda0000006420",
        "Frequency": 868100000
        "Gateways": [ 
         { 
            "GatewayEui": "c0ee40ffff29df10",
            "Rssi": -67,
            "Snr": 9.75
         }
      ],
  “WirelessDeviceId”: “30cbdcf3-86de-4291-bfab-5bfa2b12bad5"
}
```