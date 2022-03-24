# Connect your LoRaWAN gateway and verify its connection status<a name="connect-iot-lorawan-gateway-connection-status"></a>

Before you can check the gateway connection status, you must have already added your gateway and connected it to AWS IoT Core for LoRaWAN\. For information about how to add your gateway, see [Add a gateway to AWS IoT Core for LoRaWAN](connect-iot-lorawan-onboard-gateway-add.md)\.

## Connect your gateway to AWS IoT Core for LoRaWAN<a name="connect-iot-lorawan-connect-gateway"></a>

After you've added your gateway, connect to the configuration interface of your gateway to enter the configuration information and trust certificates\.

After adding the gateway's information to AWS IoT Core for LoRaWAN, add some AWS IoT Core for LoRaWAN information to the gateway device\. The documentation provided by the gateway's vendor should describe the process for uploading the certificate files to the gateway and configuring the gateway device to communicate with AWS IoT Core for LoRaWAN\.

**Gateways qualified for use with AWS IoT Core for LoRaWAN**  
For instructions on how to configure your LoRaWAN gateway, refer to the [ configure gateway device](https://iotwireless.workshop.aws/en/200_gateway/400_configuregateway.html) section of the AWS IoT Core for LoRaWAN workshop\. Here, you'll find information about instructions for connecting gateways that are qualified for use with AWS IoT Core for LoRaWAN\.

**Gateways that support CUPS protocol**  
The following instructions show how you can connect your gateways that support the CUPS protocol\.

1. Upload the following files that you obtained when adding your gateway\.
   + Gateway device certificate and private key files\.
   + Trust certificate file for CUPS endpoint, `cups.trust`\.

1. Specify the CUPS endpoint URL that you obtained previously\. The endpoint will be of the format `prefix.cups.lorawan.region.amazonaws.com:443`\.

For details about how to obtain this information, see [Add a gateway to AWS IoT Core for LoRaWAN](connect-iot-lorawan-onboard-gateway-add.md)\.

**Gateways that support LNS protocol**  
The following instructions show how you can connect your gateways that support the LNS protocol\.

1. Upload the following files that you obtained when adding your gateway\.
   + Gateway device certificate and private key files\.
   + Trust certificate file for LNS endpoint, `lns.trust`\.

1. Specify the LNS endpoint URL that you obtained previously\. The endpoint will be of the format `prefix.lns.lorawan.region.amazonaws.com:443`\.

For details about how to obtain this information, see [Add a gateway to AWS IoT Core for LoRaWAN](connect-iot-lorawan-onboard-gateway-add.md)\.

After that you've connected your gateway to AWS IoT Core for LoRaWAN, you can check the status of your connection and get information about when the last uplink was received by using the console or the API\.

## Check gateway connection status using the console<a name="connect-iot-lorawan-connection-status-console"></a>

To check the connection status using the console, navigate to the [https://console.aws.amazon.com/iot/home#/wireless/gateways](https://console.aws.amazon.com/iot/home#/wireless/gateways) page of the AWS IoT console and choose the gateway you've added\. In the **LoRaWAN specific details** section of the Gateway details page, you'll see the connection status and the date and time the last uplink was received\.

## Check gateway connection status using the API<a name="connect-iot-lorawan-connection-status-api"></a>

To check the connection status using the API, use the `GetWirelessGatewayStatistics` API\. This API doesn't have a request body and only contains a response body that shows whether the gateway is connected and when the last uplink was received\.

```
HTTP/1.1 200
Content-type: application/json

{
  "ConnectionStatus": "Connected",
  "LastUplinkReceivedAt": "2021-03-24T23:13:08.476015749Z",
  "WirelessGatewayId": "30cbdcf3-86de-4291-bfab-5bfa2b12bad5"
}
```