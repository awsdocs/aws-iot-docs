# Log entries for wireless gateways and wireless device resources<a name="connect-iot-lorawan-wireless-log-entries"></a>

After you've enabled logging, you can view log entries for your wireless gateways and wireless devices\. The following section describes the various kinds of log entries based on your resource and event types\.

## Wireless gateway log entries<a name="connect-iot-lorawan-gateway-log-entries"></a>

This section shows some of the sample log entries for your wireless gateway resources that you'll see in the [CloudWatch console](https://console.aws.amazon.com/cloudwatch)\. These log messages can have event type as `CUPS_Request` or `Certificate`, and can be configured to display a log level of `INFO`, `ERROR`, or `DISABLED` at the resource level or the event level\. If you want to see only error information, set the log level to `ERROR`\. The message in the `ERROR` log entry will contain information about why it failed\.

The log entries for your wireless gateway resource can be classified based on the following event types:
+ 

**CUPS\_Request**  
The LoRa Basics Station running on your gateway periodically sends a request to the Configuration and Update Server \(CUPS\) for updates\. For this event type, if you set log level to `INFO` when configuring the CLI for your wireless gateway resource, then in the logs:
  + If the event is successful, you'll see log messages that have a `logLevel` of `INFO`\. The messages will include details about the CUPS response sent to your gateway and the gateway details\. Following shows an example of this log entry\. For more information about the `logLevel` and other fields in the log entry, see [AWS IoT Core for LoRaWAN resources and log levels](connect-iot-lorawan-configure-resource-logging.md#connect-iot-lorawan-log-levels-resources)\.

    ```
    {
        "timestamp": "2021-05-13T16:56:08.853Z",
        "resource": "WirelessGateway",
        "wirelessGatewayId": "5da85cc8-3361-4c79-8be3-3360fb87abda",
        "wirelessGatewayType": "LoRaWAN",
        "gatewayEui": "feffff00000000e2",
        "event": "CUPS_Request",
        "logLevel": "INFO",
        "message": "Sending CUPS response of total length 3213 to GatewayEui: feffff00000000e2 with  TC Credentials,"
    }
    ```
  + If there is an error, you'll see log entries that have a `logLevel` of `ERROR`, and the messages will include details about the error\. Examples of when an error can occur for the `CUPS_Request` event include: missing CUPS CRC, mismatch in the gateway's TC Uri with AWS IoT Core for LoRaWAN, missing `IoTWirelessGatewayCertManagerRole`, or not able to obtain wireless gateway record\. Following example shows a missing CRC log entry\. To resolve the error, check your gateway setup to verify that you've entered the correct CUPS CRC\.

    ```
    {
        "timestamp": "2021-05-13T16:56:08.853Z",
        "resource": "WirelessGateway",
        "wirelessGatewayId": "5da85cc8-3361-4c79-8be3-3360fb87abda",
        "wirelessGatewayType": "LoRaWAN",
        "gatewayEui": "feffff00000000e2",
        "event": "CUPS_Request",
        "logLevel": "ERROR",
        "message": "The CUPS CRC is missing from the request. Check your gateway setup and enter the CUPS CRC,"
    }
    ```
+ 

**Certificate**  
These log entries will help you check whether your wireless gateway presented the correct certificate for authenticating connection to AWS IoT\. For this event type, if you set log level to `INFO` when configuring the CLI for your wireless gateway resource, then in the logs:
  + If the event is successful, you'll see log messages that have a `logLevel` of `INFO`\. The messages will include details about the Certificate ID and the Wireless gateway identifier\. Following shows an example of this log entry\. For more information about the `logLevel` and other fields in the log entry, see [AWS IoT Core for LoRaWAN resources and log levels](connect-iot-lorawan-configure-resource-logging.md#connect-iot-lorawan-log-levels-resources)\.

    ```
    {
        "resource": "WirelessGateway",
        "wirelessGatewayId": "5da85cc8-3361-4c79-8be3-3360fb87abda",
        "wirelessGatewayType": "LoRaWAN",
        "event": "Certificate",
        "logLevel": "INFO",
        "message": "Gateway connection authenticated. 
        (CertificateId: b5942a7aee973eda24314e416889227a5e0aa5ed87e6eb89239a83f515dea17c, WirelessGatewayId: 5da85cc8-3361-4c79-8be3-3360fb87abda)"
    }
    ```
  + If there is an error, you'll see log entries that have a `logLevel` of `ERROR`, and the messages will include details about the error\. Examples of when an error can occur for the `Certificate` event include an invalid Certificate ID, wireless gateway identifier, or a mismatch between the wireless gateway identifier and the Certificate ID\. Following example shows an `ERROR` due to invalid wireless gateway identifier\. To resolve the error, check the gateway identifiers\.

    ```
    {
        "resource": "WirelessGateway",
        "wirelessGatewayId": "5da85cc8-3361-4c79-8be3-3360fb87abda",
        "wirelessGatewayType": "LoRaWAN",
        "event": "Certificate",
        "logLevel": "INFO",
        "message": "The gateway connection couldn't be authenticated because a provisioned gateway associated with the certificate couldn't be found. 
        (CertificateId: 729828e264810f6fc7134daf68056e8fd848afc32bfe8082beeb44116d709d9e)"
    }
    ```

## Wireless device log entries<a name="connect-iot-lorawan-device-log-entries"></a>

This section shows some of the sample log entries for your wireless device resources that you'll see in the [CloudWatch console](https://console.aws.amazon.com/cloudwatch)\. The event type for these log messages depend on whether you're using a LoRaWAN or a Sidewalk device\. Each wireless device resource or event type can be configured to display a log level of `INFO`, `ERROR`, or `DISABLED`\. 

**Note**  
Your request must not contain both LoRaWAN and Sidewalk wireless metadata at the same time\. To avoid an `ERROR` log entry for this scenario, specify either LoRaWAN or Sidewalk wireless data\.

### LoRaWAN device log entries<a name="connect-iot-lorawan-lorawan-log-entries"></a>

The log entries for your LoRaWAN wireless device can be classified based on the following event types:
+ 

**`Join` and `Rejoin`**  
When you add a LoRaWAN device and connect it to AWS IoT Core for LoRaWAN, before your device can send uplink data, you must complete a process called `activation` or `join procedure`\. For more information, see [Add your wireless device to AWS IoT Core for LoRaWAN](connect-iot-lorawan-end-devices-add.md)\.

  For this event type, if you set log level to `INFO` when configuring the CLI for your wireless gateway resource, then in the logs:
  + If the event is successful, you'll see log messages that have a `logLevel` of `INFO`\. The messages will include details about the status of your join or rejoin request\. Following shows an example of this log entry\. For more information about the `logLevel` and other fields in the log entry, see [AWS IoT Core for LoRaWAN resources and log levels](connect-iot-lorawan-configure-resource-logging.md#connect-iot-lorawan-log-levels-resources)\.

    ```
    {
        "timestamp": "2021-05-13T16:56:08.853Z",
        "resource": "WirelessDevice",
        "wirelessDeviceType": "LoRaWAN",
        "WirelessDeviceId": "5da85cc8-3361-4c79-8be3-3360fb87abda",    
        "devEui": "feffff00000000e2",
        "event": "Rejoin",
        "logLevel": "INFO",
        "message": "Rejoin succeeded"
    }
    ```
  + If there is an error, you'll see log entries that have a `logLevel` of `ERROR`, and the messages will include details about the error\. Examples of when an error can occur for the `Join` and `Rejoin` events include invalid LoRaWAN region setting, or invalid Message Integrity Code \(MIC\) check\. Following example shows a join error due to MIC check\. To resolve the error, check whether you've entered the correct root keys\.

    ```
    {
        "timestamp": "2020-11-24T01:46:50.883481989Z",
        "resource": "WirelessDevice",
        "wirelessDeviceType": "LoRaWAN",
        "WirelessDeviceId": "cb4c087c-1be5-4990-8654-ccf543ee9fff",
        "devEui": "58a0cb000020255c",
        "event": "Join",
        "logLevel": "ERROR",
        "message": "invalid MIC. It's most likely caused by wrong root keys."
    }
    ```
+ 

**Uplink\_Data and Downlink\_Data**  
The event type `Uplink_Data` is used for messages that are generated by AWS IoT Core for LoRaWAN when the payload is sent from the wireless device to AWS IoT\. The event type `Downlink_Data` is used for messages that are related to downlink messages that are sent from AWS IoT to the wireless device\.
**Note**  
Events `Uplink_Data` and `Downlink_Data` apply to both LoRaWAN and Sidewalk devices\.

  For this event type, if you set log level to `INFO` when configuring the CLI for your wireless devices, then in the logs, you'll see: 
  + If the event is successful, you'll see log messages that have a `logLevel` of `INFO`\. The messages will include details about the status of the uplink or downlink message that was sent and the wireless device identifier\. Following shows an example of this log entry for a Sidewalk device\. For more information about the `logLevel` and other fields in the log entry, see [AWS IoT Core for LoRaWAN resources and log levels](connect-iot-lorawan-configure-resource-logging.md#connect-iot-lorawan-log-levels-resources)\.

    ```
    {
        "resource": "WirelessDevice",
        "wirelessDeviceId": "5371db88-d63d-481a-868a-e54b6431845d",
        "wirelessDeviceType": "Sidewalk",
        "event": "Downlink_Data",
        "logLevel": "INFO",
        "messageId": "8da04fa8-037d-4ae9-bf67-35c4bb33da71",
        "message": "Message delivery succeeded.  MessageId: 8da04fa8-037d-4ae9-bf67-35c4bb33da71. AWS IoT Core: {\"message\":\"OK\",\"traceId\":\"038b5b05-a340-d18a-150d-d5a578233b09\"}"
    }
    ```
  + If there is an error, you'll see log entries that have a `logLevel` of `ERROR`, and the messages will include details about the error, which will help you resolve it\. Examples of when an error can occur for the `Registration` event include: authentication issues, invalid or too many requests, unable to encrypt or decrypt the payload, or unable to find the wireless device using the specified ID\. Following example shows a permission error encountered while processing a message\.

    ```
    {
        "resource": "WirelessDevice",
        "wirelessDeviceId": "cb4c087c-1be5-4990-8654-ccf543ee9fff",
        "wirelessDeviceType": "LoRaWAN",
        "event": "Uplink_Data",
        "logLevel": "ERROR",
        "message": "Cannot assume role MessageId: ef38877f-3454-4c99-96ed-5088c1cd8dee. 
        Access denied: User: arn:aws:sts::005196538709:assumed-role/DataRoutingServiceRole/6368b35fd48c445c9a14781b5d5890ed is not authorized to perform: sts:AssumeRole on resource: arn:aws:iam::400232685877:role/ExecuteRules_Role\tstatus code: 403, request id: 471c3e35-f8f3-4e94-b734-c862f63f4edb"
    }
    ```

### Sidewalk device log entries<a name="connect-iot-lorawan-sidewalk-log-entries"></a>

The log entries for your Sidewalk device can be classified based on the following event types:
+ 

**`Registration`**  
These log entries will help you monitor the status of any Sidewalk devices that you're registering with AWS IoT Core for LoRaWAN\. For this event type, if you set log level to `INFO` when configuring the CLI for your wireless device resource, then in the logs, you'll see log messages that have a `logLevel` of `INFO` and `ERROR`\. The messages will include details about the registration progress from start to completion\. `ERROR` log messages will contain information about how to troubleshoot issues with registering your device\.

  Following shows an example for a log message with log level of `INFO`\. For more information about the `logLevel` and other fields in the log entry, see [AWS IoT Core for LoRaWAN resources and log levels](connect-iot-lorawan-configure-resource-logging.md#connect-iot-lorawan-log-levels-resources)\.

  ```
  {
      "resource": "WirelessDevice",
      "wirelessDeviceId": "8d0b2775-e19b-4b2a-a351-cb8a2734a504",
      "wirelessDeviceType": "Sidewalk",
      "event": "Registration",
      "logLevel": "INFO",
      "message": "Successfully completed device registration. Amazon SidewalkId = 2000000002"
  }
  ```
+ 

**Uplink\_Data and Downlink\_Data**  
The event types `Uplink_Data` and `Downlink_Data` for Sidewalk devices are similar to the corresponding event types for LoRaWAN devices\. For more information, refer to the **Uplink\_Data and Downlink\_Data** section described previously for LoRaWAN device log entries\.

### Next steps<a name="connect-iot-lorawan-cwl-format-next-steps"></a>

You've learned how to view log entries for your resources and the different log entries that you can view in the CloudWatch console after enabling logging for AWS IoT Core for LoRaWAN\. While you can create filter streams using **Log groups**, we recommend that you use CloudWatch Insights to create and use filter streams\. For more information, see [Use CloudWatch Insights to filter logs for AWS IoT Core for LoRaWAN](connect-iot-lorawan-cwl-insights.md)\.