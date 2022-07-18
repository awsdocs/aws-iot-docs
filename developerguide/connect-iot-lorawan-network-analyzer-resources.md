# Add resources and update the network analyzer configuration<a name="connect-iot-lorawan-network-analyzer-resources"></a>

Before you can activate trace messaging, add the resources that you want to monitor to your network analyzer configuration\. Resources can be either or both LoRaWAN devices and LoRaWAN gateways\. 

## Prerequisites<a name="connect-iot-lorawan-network-analyzer-resources-prereq"></a>

Before you can add resources:

1. You must have onboarded the gateways and devices that you want to monitor to AWS IoT Core for LoRaWAN\. For more information, see [Connecting gateways and devices to AWS IoT Core for LoRaWAN](connect-iot-lorawan-getting-started.md)\.

1. You must have created a network analyzer configuration for which you'll be adding resources\. For more information, see [Create a network analyzer configuration](connect-iot-lorawan-network-analyzer-create.md)\.

## Add resources and update configuration settings by using the console<a name="connect-iot-lorawan-network-analyzer-add-resources-console"></a>

You can add resources and customize the optional parameters by using the AWS IoT console or the AWS IoT Wireless API\. In addition to resources, you can also edit your configuration settings and save the updated configuration\.

**Add resources to your configuration**  


1. Open the [Network Analyzer hub of the AWS IoT console](https://console.aws.amazon.com/iot/home#/wireless/networkAnalyzer) and choose the configuration for which you want to add resources\.

1. Choose **Actions** and then choose **Add resources**\.

1. Add the resources you want to monitor by using the wireless gateway and wireless device identifiers\. You can choose multiple resources, and add up to 250 wireless gateways or wireless devices\. 

1. After you've added all the resources, choose **Add**\.

   You'll see the number of gateways and devices that you added in the **Network Analyzer hub page**\. You can continue adding and removing resources until you activate the trace messaging session\. After the session has been activated, to add resources, you'll have to deactivate the session\.

**Update your configuration settings**  


1. Open the [Network Analyzer hub of the AWS IoT console](https://console.aws.amazon.com/iot/home#/wireless/networkAnalyzer) and choose the configuration for which you want to update the settings\.

1. Choose **Actions** and then choose **Edit**\.

1. Choose whether to disable frame info and use **Select log levels** to choose the log levels that you want to use for your trace message logs\. Choose **Save**\.

   You'll see the configuration settings that you specified in the details page of your network analyzer configuration\.

## Add resources and update configuration settings by using the API<a name="connect-iot-lorawan-network-analyzer-add-resources-api"></a>

To add resources or update your configuration settings, use the [UpdateNetworkAnalyzerConfiguration](https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_UpdateNetworkAnalyzerConfiguration.html) API or the [update\-network\-analyzer\-configuration](https://docs.aws.amazon.com/cli/latest/reference/iotwireless/update-network-analyzer-configuration.html) CLI\.
+ 

**Update configuration settings**  
To update your configuration settings, use the `TraceContent` parameter to specify the log level and whether to enable frame info\. For example, the following command updates the configuration settings by disabling the frame info and by setting the log level to `ERROR`\.

  ```
  aws iotwireless update-network-analyzer-configuration \ 
      --configuration-name NetworkAnalyzerConfig_Default \ 
      --trace-content WirelessDeviceFrameInfo=DISABLED,LogLevel="ERROR"
  ```
+ 

**Add resources**  
To add resources, use the `WirelessDevicesToAdd` and `WirelessGatewaysToAdd` parameters to specify the gateways or devices or both that you want to add to your configuration\. For example, the following command updates the configuration settings and adds to your configuration the wireless resources, specified by their `WirelessGatewayID` and `WirelessDeviceID`\.

  ```
  aws iotwireless update-network-analyzer-configuration \ 
      --configuration-name NetworkAnalyzerConfig_Default \ 
      --trace-content WirelessDeviceFrameInfo=DISABLED,LogLevel="ERROR" \ 
      --wireless-gateways-to-add "12345678-a1b2-3c45-67d8-e90fa1b2c34d" "90123456-de1f-2b3b-4c5c-bb1112223cd1"   
      --wireless-devices-to-add "1ffd32c8-8130-4194-96df-622f072a315f"
  ```

  To remove devices or gateways, use the `WirelessDevicesToRemove` and `WirelessGatewaysToRemove` parameters of the API\.

**Get information about the configuration**  
Running the `UpdateNetworkAnalyzerConfiguration` API doesn't produce any output\. To view your configuration settings and the gateways or devices that you've added, use the [GetNetworkAnalyzerConfiguration](https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_UpdateNetworkAnalyzerConfiguration.html) API operation or the [get\-network\-analyzer\-configuration](https://docs.aws.amazon.com/cli/latest/reference/iotwireless/get-network-analyzer-configuration.html) command\. Provide the name of the network analyzer configuration as input\.

```
aws iotwireless get-network-analyzer-configuration \ 
    --configuration-name NetworkAnalyzerConfig_Default
```

Running this command produces the following output\.

```
{
    "TraceContent": {
        "WirelessDeviceFrameInfo": "DISABLED",
        "LogLevel": "ERROR"
    },
    "WirelessDevices": [],
    "WirelessGateways": [
        "a0dd70e5-8f15-41a5-89cf-310284e691a1",
        "41682155-de4f-4f8f-84bf-bb5557221fc8"
    ]
}
```

## Next steps<a name="connect-iot-lorawan-network-analyzer-resources-next"></a>

Now that you've added resources and specified any optional configuration settings for your configuration, you can use the WebSocket protocol to establish a connection with AWS IoT Core for LoRaWAN for using network analyzer\. You can then activate trace messaging and start receiving trace messages for your resources\. For more information, see [Stream network analyzer trace messages with WebSockets](connect-iot-lorawan-network-analyzer-api.md)\. 