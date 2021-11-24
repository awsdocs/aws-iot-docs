# Add resources and update the network analyzer configuration<a name="connect-iot-lorawan-network-analyzer-resources"></a>

Before you can activate trace messaging, you must add resources to your configuration\. You can use only a single, default network analyzer configuration\. AWS IoT Core for LoRaWAN assigns the name, **NetworkAnalyzerConfig\_Default**, to this configuration, and this field can't be edited\. This configuration is automatically added to your AWS account when you use network analyzer from the console\.

You can add the resources that you want to monitor to this default configuration\. Resources can be either or both LoRaWAN devices and LoRaWAN gateways\. To add each individual resource to the configuration, use the wireless gateway and wireless device identifiers\.

## Configuration settings<a name="connect-iot-lorawan-network-analyzer-resources-config-settings"></a>

To configure settings, first add resources to your default configuration and activate trace messaging\. After you've received the trace message logs, you can also customize the following parameters to update your default configuration and filter the log stream\.
+ 

**Frame info**  
This setting is the frame info of your wireless device resources for trace messages\. the frame info is enabled by default, and can be used to debug the communication between your network server and the end devices\.
+ 

**Log levels**  
You can view Info or Error logs, or you can turn off logging\.
  + 

**Info**  
Logs with a log level of **Info** are more verbose and contain log streams that are both informative and contain errors\. The informative logs can be used to view changes to the state of a device or gateway\.
**Note**  
Collecting more verbose log streams can incur additional costs\. For more information about pricing, see [AWS IoT Core pricing](http://aws.amazon.com/iot-core/pricing/)\. 
  + 

**Error**  
Logs with a log level of **Error** are less verbose and display only error information\. You can use these logs when an application has an error, such as a device connection error\. By using the information from the log stream, you can identify and troubleshoot errors for resources in your fleet\. 

## Prerequisites<a name="connect-iot-lorawan-network-analyzer-resources-prereq"></a>

Before you can add resources, you must have onboarded the gateways and devices that you want to monitor to AWS IoT Core for LoRaWAN\. For more information, see [Connecting gateways and devices to AWS IoT Core for LoRaWAN](connect-iot-lorawan-getting-started.md)\.

## Add resources and update the network analyzer configuration by using the console<a name="connect-iot-lorawan-network-analyzer-add-resources-console"></a>

You can add resources and customize the optional parameters by using the AWS IoT console or the AWS IoT Wireless API\. In addition to resources, you can also edit your configuration settings and save the updated configuration\.

**To add resources to your configuration \(console\)**  


1. Open the [Network Analyzer hub of the AWS IoT console](https://console.aws.amazon.com/iot/home#/wireless/networkAnalyzer) and choose the network analyzer configuration, **NetworkAnalyzerConfig\_Default**\.

1. Choose **Add resources**\.

1. Add the resources you want to monitor by using the wireless gateway and wireless device identifiers\. You can add up to 250 wireless gateways or wireless devices\. To add your resource:

   1. Use the **View gateways** or **View devices** tab to see the list of gateways and devices that you've added to your AWS account\.

   1. Copy the `WirelessDeviceID` or the `WirelessGatewayID` of the device or gateway that you want to monitor and enter the identifier value for the corresponding resource\.

   1. To continue adding resources, choose **Add gateway** or **Add device** and add your wireless gateway or device\. If you added a resource that you no longer want to monitor, choose **Remove resource**\.

1. After you've added all the resources, choose **Add**\.

   You'll see the number of gateways and devices that you added in the **Network Analyzer hub page**\. You can still continue adding gateways and devices until you activate the trace messaging session\. After the session has been activated, to add resources, you'll have to deactivate the session\.

**To edit the network analyzer configuration \(console\)**  
You can also edit the network analyzer configuration and choose whether to disable frame info and the log level for your trace message logs\. 

1. Open the [Network Analyzer hub of the AWS IoT console](https://console.aws.amazon.com/iot/home#/wireless/networkAnalyzer) and choose the network analyzer configuration, **NetworkAnalyzerConfig\_Default**\.

1. Choose **Edit**\.

1. Choose whether to disable frame info and use **Select log levels** to choose the log levels that you want to use for your trace message logs\. Choose **Save**\.

   You'll see the configuration settings that you specified in the details page of your network analyzer configuration\.

## Add resources and update the network analyzer configuration by using the API<a name="connect-iot-lorawan-network-analyzer-add-resources-api"></a>

You can use the [AWS IoT Wireless API operations](https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/) or the [AWS IoT Wireless CLI commands](https://docs.aws.amazon.com/cli/latest/reference/iotwireless/index.html) to add resources and update the configuration settings for your network analyzer configuration\.
+ To add resources and update your network analyzer configuration, use the [UpdateNetworkAnalyzerConfiguration](https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_UpdateNetworkAnalyzerConfiguration.html) API or the [update\-network\-analyzer\-configuration](https://docs.aws.amazon.com/cli/latest/reference/iotwireless/update-network-analyzer-configuration.html) CLI\.
  + 

**Add resources**  
For the wireless devices you want to add, use `WirelessDevicesToAdd` to enter the `WirelessDeviceID` for the devices as an array of strings\. For the wireless gateways you want to add, use `WirelessGatewaysToAdd` to enter the `WirelessGatewayID` for the gateways as an array of strings\.
  + 

**Edit configuration**  
To edit your network analyzer configuration, use the `TraceContent` parameter to specify whether `WirelessDeviceFrameInfo` should be `ENABLED` or `DISABLED`, and whether the `LogLevel` parameter should be `INFO`, `ERROR`, or `DISABLED`\. 

  ```
  {
     "TraceContent": { 
        "LogLevel": "string",
        "WirelessDeviceFrameInfo": "string"
     },
     "WirelessDevicesToAdd": [ "string" ],
     "WirelessDevicesToRemove": [ "string" ],
     "WirelessGatewaysToAdd": [ "string" ],
     "WirelessGatewaysToRemove": [ "string" ]
  }
  ```
+ To get information about the configuration and the resources that you've added, use the [GetNetworkAnalyzerConfiguration](https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_UpdateNetworkAnalyzerConfiguration.html) API operation or the [get\-network\-analyzer\-configuration](https://docs.aws.amazon.com/cli/latest/reference/iotwireless/get-network-analyzer-configuration.html) command\. Provide the name of the network analyzer configuration, `NetworkAnalyzerConfig_Default`, as input\.

## Next steps<a name="connect-iot-lorawan-network-analyzer-resources-next"></a>

Now that you've added resources and specified any optional configuration settings for your configuration, you can use the WebSocket protocol to establish a connection with AWS IoT Core for LoRaWAN for using network analyzer\. You can then activate trace messaging and start receiving trace messages for your resources\. For more information, see [Stream network analyzer trace messages with WebSockets](connect-iot-lorawan-network-analyzer-api.md)\. 