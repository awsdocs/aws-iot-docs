# Troubleshooting device fleet disconnects<a name="ota-troubleshooting-fleet-disconnects"></a>

AWS IoT device fleet disconnects can happen for multiple reasons\. This article explains how to diagnose a disconnect reason and how to handle disconnects caused by regular maintenance of AWS IoT service or a throttling limit\.

**To diagnose the disconnect reason**

You can check the [AWSIotLogsV2](https://console.aws.amazon.com/cloudwatch/home?region=us-east-1#logsV2:log-groups/log-group/AWSIotLogsV2) log group in [CloudWatch](https://docs.aws.amazon.com/iot/latest/developerguide/cwl-format.html) to identify the disconnect reason in the `disconnectReason` field of the log entry\. 

You can also use AWS IoT's [lifecycle events](https://docs.aws.amazon.com/iot/latest/developerguide/life-cycle-events.html) feature to identify the disconnect reason\. If you’ve subscribed to [lifecycle's disconnect event](https://docs.aws.amazon.com/iot/latest/developerguide/life-cycle-events.html#connect-disconnect) \(`$aws/events/presence/disconnected/clientId`\), you’ll get a notification from AWS IoT when the disconnect happens\. You can identify the disconnect reason in the `disconnectReason` field of the notification\. 

For more information, see [CloudWatch AWS IoT log entries](https://docs.aws.amazon.com/iot/latest/developerguide/cwl-format.html) and [Lifecycle events](https://docs.aws.amazon.com/iot/latest/developerguide/life-cycle-events.html)\.

**To troubleshoot disconnects due to AWS IoT service maintenance**

Disconnects caused by AWS IoT's service maintenance are logged as `SERVER_INITIATED_DISCONNECT` in AWS IoT's lifecycle event and CloudWatch\. To handle these disconnects, adjust your client\-side setup to make sure your devices can be automatically reconnected to the AWS IoT platform\. 

**To troubleshoot disconnects due to a throttling limit**

Disconnects caused by a throttling limit are logged as `THROTTLED` in AWS IoT's lifecycle event and CloudWatch\. To handle these disconnects, you can request [message broker limit increases](https://docs.aws.amazon.com/general/latest/gr/iot-core.html#message-broker-limits) as the device count grows\. 

For more information, see [AWS IoT Core Message Broker](https://docs.aws.amazon.com/general/latest/gr/iot-core.html#message-broker-limits)\.