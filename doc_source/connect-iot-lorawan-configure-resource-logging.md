# Configure logging for AWS IoT Core for LoRaWAN resources<a name="connect-iot-lorawan-configure-resource-logging"></a>

To configure logging for AWS IoT Core for LoRaWAN resources, you can use either the API or the CLI\. When starting to monitor AWS IoT Core for LoRaWAN resources, you can use the default configuration\. To do this, you can skip this topic and proceed to [Monitor AWS IoT Core for LoRaWAN using CloudWatch Logs](connect-iot-lorawan-cloud-watch-logs.md) to monitor your logs\.

After you start monitoring the logs, you can use the CLI to change the log levels to a more verbose option, such as providing `INFO` and `ERROR` information and enabling logging for more resources\.

## AWS IoT Core for LoRaWAN resources and log levels<a name="connect-iot-lorawan-log-levels-resources"></a>

Before you use the API or CLI, use the following table to learn about the different log levels and the resources that you can configure logging for\. The table shows parameters that you see in the CloudWatch logs when you monitor the resources\. How you configure the logging for your resources will determine the logs you see in the console\.

For information about what a sample CloudWatch logs looks like and how you can use these parameters to log useful information about the AWS IoT Core for LoRaWAN resources, see [View CloudWatch AWS IoT Core for LoRaWAN log entries](connect-iot-lorawan-cwl-format.md)\.


**Log levels and resources**  

| Name | Possible values | Description | 
| --- | --- | --- | 
| logLevel |  `INFO`, `ERROR`, or `DISABLED`  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/iot/latest/developerguide/connect-iot-lorawan-configure-resource-logging.html)  | 
| resource |  `WirelessGateway` or `WirelessDevice`  |  The type of the resource, which can be `WirelessGateway` or `WirelessDevice`\.  | 
| wirelessGatewayType | LoRaWAN | The type of the wireless gateway, when resource is WirelessGateway, which is always LoRaWAN\. | 
| wirelessDeviceType | LoRaWAN or Sidewalk | The type of the wireless device, when resource is WirelessDevice, which can be LoRaWAN or Sidewalk\. | 
| wirelessGatewayId | \- | The identifier of the wireless gateway, when resource is WirelessGateway\. | 
| wirelessDeviceId | \- | The identifier of the wireless device, when resource is WirelessDevice\. | 
| event | Join, Rejoin, Registration, Uplink\_data, Downlink\_data, CUPS\_Request, and Certificate | The type of event being logged, which depends on whether the resource that you're logging is a wireless device or a wireless gateway\. For more information, see [View CloudWatch AWS IoT Core for LoRaWAN log entries](connect-iot-lorawan-cwl-format.md)\. | 

## AWS IoT Wireless logging API<a name="connect-iot-lorawan-logging-api-roles"></a>

You can use the following API actions to configure logging of resources\. The table also shows a sample IAM policy that you must create for using the API actions\. The following section describes how you can use the APIs to configure log levels of your resources\.


**Logging API actions**  

| API name | Description | Sample IAM policy | 
| --- | --- | --- | 
|  [GetLogLevelsByResourceTypes](https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_GetLogLevelsByResourceTypes.html)  |  Returns current default log levels, or log levels by resource types, which can include log options for wireless devices or wireless gateways\.  | <pre>{    <br />    "Version": "2012-10-17",    <br />    "Statement": [        <br />        {            <br />            "Effect": "Allow",            <br />            "Action": [                <br />                 "iotwireless:GetLogLevelsByResourceTypes"            <br />            ],            <br />            "Resource": [                <br />                "*"            <br />            ]        <br />        }<br />    ]<br />}</pre>  | 
|  [GetResourceLogLevel](https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_GetResourceLogLevel.html)  | Returns the log\-level override for a given resource identifier and resource type\. The resource can be a wireless device or a wireless gateway\.  | <pre>{    <br />    "Version": "2012-10-17",    <br />    "Statement": [        <br />        {            <br />            "Effect": "Allow",            <br />            "Action": [                <br />                 "iotwireless:GetResourceLogLevel"            <br />            ],            <br />            "Resource": [                <br />                 "arn:aws:iotwireless:us-east-1:123456789012:WirelessDevice/012bc537-ab12-cd3a-d00e-1f0e20c1204a",           <br />            ]        <br />        }<br />    ]<br />}</pre> | 
|  [PutResourceLogLevel](https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_PutResourceLogLevel.html)  | Sets the log\-level override for a given resource identifier and resource type\. The resource can be a wireless gateway or a wireless device\.  This API has a limit of 200 log\-level overrides per account\.   | <pre>{    <br />    "Version": "2012-10-17",    <br />    "Statement": [        <br />        {            <br />            "Effect": "Allow",            <br />            "Action": [                <br />                 "iotwireless:PutResourceLogLevel"            <br />            ],            <br />            "Resource": [                <br />                "arn:aws:iotwireless:us-east-1:123456789012:WirelessDevice/012bc537-ab12-cd3a-d00e-1f0e20c1204a",            <br />            ]        <br />        }<br />    ]<br />}</pre> | 
|  [ResetAllResourceLogLevels](https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_ResetAllResourceLogLevels.html)  |  Removes the log\-level overrides for all resources, which includes both wireless gateways and wireless devices\.  This API doesn't affect the log levels that are set using the `UpdateLogLevelsByResourceTypes` API\.   | <pre>{    <br />    "Version": "2012-10-17",    <br />    "Statement": [        <br />        {            <br />            "Effect": "Allow",            <br />            "Action": [                <br />                 "iotwireless:ResetAllResourceLogLevels"            <br />            ],            <br />            "Resource": [                <br />                "arn:aws:iotwireless:us-east-1:123456789012:WirelessDevice/*", <br />                "arn:aws:iotwireless:us-east-1:123456789012:WirelessGateway/*<br />            ]        <br />        }<br />    ]<br />}</pre> | 
|  [ResetResourceLogLevel](https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_ResetResourceLogLevel.html)  |  Removes the log\-level override for a given resource identifier and resource type\. The resource can be a wireless gateway or a wireless device\.  | <pre>{    <br />    "Version": "2012-10-17",    <br />    "Statement": [        <br />        {            <br />            "Effect": "Allow",            <br />            "Action": [                <br />                 "iotwireless:ResetResourceLogLevel"            <br />            ],            <br />            "Resource": [                <br />                "arn:aws:iotwireless:us-east-1:123456789012:WirelessDevice/012bc537-ab12-cd3a-d00e-1f0e20c1204a",            <br />            ]        <br />        }<br />    ]<br />}</pre> | 
|  [UpdateLogLevelsByResourceTypes](https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_UpdateLogLevelsByResourceTypes.html)  |  Set default log level, or log levels by resource types\. You can use this API for log options for wireless devices or wireless gateways, and control the log messages that'll be displayed in CloudWatch\.  Events are optional and the event type is tied to the resource type\. For more information, see [Events and resource types](connect-iot-lorawan-cwl-format.md#connect-iot-lorawan-cwl-format-events-resources)\.   | <pre>{    <br />    "Version": "2012-10-17",    <br />    "Statement": [        <br />        {            <br />            "Effect": "Allow",            <br />            "Action": [                <br />                 "iotwireless:UpdateLogLevelsByResourceTypes"            <br />            ],            <br />            "Resource": [                <br />                "*"            <br />            ]        <br />        }<br />    ]<br />}</pre> | 

## Configure log levels of resources using the CLI<a name="connect-iot-lorawan-configure-logging-api"></a>

This section describes how to configure log levels for AWS IoT Core for LoRaWAN resources by using the API or AWS CLI\.

**Before you use the CLI:**
+ Make sure you created the IAM policy for the API for which you want to run the CLI command, as described previously\.
+ You need the Amazon Resource Name \(ARN\) of the role you want to use\. If you need to create a role to use for logging, see [Create logging role and policy for AWS IoT Core for LoRaWAN](connect-iot-lorawan-create-logging-role-policy.md)\.

**Why use the AWS CLI**  
By default, if you create the IAM role, `IoTWirelessLogsRole`, as described in [Create logging role and policy for AWS IoT Core for LoRaWAN](connect-iot-lorawan-create-logging-role-policy.md), you'll see CloudWatch logs in the AWS Management Console that have a default log level of `ERROR`\. To change the default log level for all your resources or for specific resources, use the AWS IoT Wireless logging API or CLI\.

**How to use the AWS CLI**  
The API actions can be categorized into the following types depending on whether you want to configure log levels for all resources or for specific resources:
+ API actions `GetLogLevelsByResourceTypes` and `UpdateLogLevelsByResourceTypes` can retrieve and update the log levels for all resources in your account that are of a specific type, such as a wireless gateway, or a LoRaWAN or Sidewalk device\.
+ API actions `GetResourceLogLevel`, `PutResourceLogLevel`, and `ResetResourceLogLevel` can retrieve, update, and reset log levels of individual resources that you specify using a resource identifier\.
+ API action `ResetAllResourceLogLevels` resets the log\-level override to `null` for all resources for which you specified a log\-level override using the `PutResourceLogLevel` API\.

**To use the CLI to configure resource\-specific logging for AWS IoT**
**Note**  
You can also perform this procedure with the API by using the methods in the AWS API that correspond to the CLI commands shown here\. 

1. By default, all resources have log level set to `ERROR`\. To set the default log levels, or log levels by resource types for all resources in your account, use the [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/iotwireless/update-log-levels-by-resource-types.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/iotwireless/update-log-levels-by-resource-types.html) command\. The following example shows how you can create a JSON file, `Input.json`, and provide it as an input to the CLI command\. You can use this command to selectively disable logging or override the default log level for specific types of resources and events\.

   ```
   {
       "DefaultLogLevel": "INFO",
       "WirelessDeviceLogOptions": 
        [
           {
            "Type": "Sidewalk",
            "LogLevel": "INFO",
            "Events": 
             [
               {
                 "Event": "Registration",
                 "LogLevel": "DISABLED"
               }
             ]
           }, 
           {
            "Type": "LoRaWAN",
            "LogLevel": "INFO",
            "Events": 
             [
               {
                "Event": "Join",
                "LogLevel": "DISABLED"
               }, 
               {
                "Event": "Rejoin",
                "LogLevel": "ERROR"
               }
             ]
           }
         ]
        "WirelessGatewayLogOptions": 
         [
           {
            "Type": "LoRaWAN",
            "LogLevel": "INFO",
            "Events": 
             [
               {
                "Event": "CUPS_Request",
                "LogLevel": "DISABLED"
               }, 
               {
                 "Event": "Certificate",
                 "LogLevel": "ERROR"
               }
             ]
           }
         ]
   }
   ```

   where:  
WirelessDeviceLogOptions  
The list of log options for a wireless device\. Each log option includes the wireless device type \(Sidewalk or LoRaWAN\), and a list of wireless device event log options\. Each wireless device event log option can optionally include the event type and its log level\.  
WirelessGatewayLogOptions  
The list of log options for a wireless gateway\. Each log option includes the wireless gateway type \(LoRaWAN\), and a list of wireless gateway event log options\. Each wireless gateway event log option can optionally include the event type and its log level\.  
DefaultLogLevel  
The log level to use for all your resources\. Valid values are: `ERROR`, `INFO`, and `DISABLED`\. The default value is `INFO`\.  
LogLevel  
The log level you want to use for individual resource types and events\. These log levels override the default log level, such as the log level `INFO` for the LoRaWAN gateway, and log levels `DISABLED` and `ERROR` for the two event types\.

   Run the following command to provide the `Input.json` file as input to the command\. This command doesn't produce any output\.

   ```
   aws iotwireless update-log-levels-by-resource-types \ 
       --cli-input-json Input.json
   ```

   If you want to remove the log options for both wireless devices and wireless gateways, run the following command\.

   ```
   {
       "DefaultLogLevel":"DISABLED",
       "WirelessDeviceLogOptions": [],
       "WireslessGatewayLogOptions":[]
   }
   ```

1. The update\-log\-levels\-by\-resource\-types command doesn't return any output\. Use the [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/iotwireless/get-log-levels-by-resource-types.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/iotwireless/get-log-levels-by-resource-types.html) command to retrieve resource\-specific logging information\. The command returns the default log level, and the wireless device and wireless gateway log options\.
**Note**  
The get\-log\-levels\-by\-resource\-types command can't directly retrieve the log levels in the CloudWatch console\. You can use the get\-log\-levels\-by\-resource\-types command to get the latest log\-level information that you've specified for your resources using the update\-log\-levels\-by\-resource\-types command\.

   ```
   aws iotwireless get-log-levels-by-resource-types 
   ```

   When you run the following command, it returns the latest logging information you specified with update\-log\-levels\-by\-resource\-types\. For example, if you remove the wireless device log options, then running the get\-log\-levels\-by\-resource\-types will return this value as `null`\. 

   ```
   {
       "DefaultLogLevel": "INFO",
       "WirelessDeviceLogOptions": null,
        "WirelessGatewayLogOptions": 
         [
           {
            "Type": "LoRaWAN",
            "LogLevel": "INFO",
            "Events": 
             [
               {
                "Event": "CUPS_Request",
                "LogLevel": "DISABLED"
               }, 
               {
                 "Event": "Certificate",
                 "LogLevel": "ERROR"
               }
             ]
           }
         ]
   }
   ```

1. To control log levels for individual wireless gateways or wireless device resources, use the following CLI commands:
   + [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/iotwireless/put-resource-log-level.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/iotwireless/put-resource-log-level.html)
   + [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/iotwireless/get-resource-log-level.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/iotwireless/get-resource-log-level.html)
   + [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/iotwireless/reset-resource-log-level.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/iotwireless/reset-resource-log-level.html)

   For an example for when to use these CLIs, say that you have a large number of wireless devices or gateways in your account that are being logged\. If you want to troubleshoot errors for only some of your wireless devices, you can disable logging for all wireless devices by setting the `DefaultLogLevel` to `DISABLED`, and use the put\-resource\-log\-level to set the `LogLevel` to `ERROR` for only those devices in your account\.

   ```
   aws iotwireless put-resource-log-level \ 
       --resource-identifier 
       --resource-type WirelessDevice
       --log-level ERROR
   ```

   In this example, the command sets the log level to `ERROR` only for the specified wireless device resource and the logs for all other resources are disabled\. This command doesn't produce any output\. To retrieve this information and verify that the log levels were set, use the get\-resource\-log\-level command\.

1. In the previous step, after you've debugged the issue and resolved the error, you can run the reset\-resource\-log\-level command to reset the log level for that resource to `null`\. If you used the `put-resource-log-level` command to set the log\-level override for more than one wireless device or gateway resource, such as for troubleshooting errors for multiple devices, you can reset the log\-level overrides back to `null` for all those resources using the [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/iotwireless/reset-all-resource-log-levels.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/iotwireless/reset-all-resource-log-levels.html) command\.

   ```
   aws iotwireless reset-all-resource-log-levels
   ```

   This command doesn't produce any output\. To retrieve the logging information for the resources, run the get\-resource\-log\-level command\.

## Next Steps<a name="connect-iot-lorawan-configure-logging-next-steps"></a>

You've learned how to create the logging role and use the AWS IoT Wireless API to configure logging for your AWS IoT Core for LoRaWANresources\. Next, to learn about monitoring your log entries, go to [Monitor AWS IoT Core for LoRaWAN using CloudWatch Logs](connect-iot-lorawan-cloud-watch-logs.md)\.