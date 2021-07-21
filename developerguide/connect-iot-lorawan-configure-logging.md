# Configure Logging for AWS IoT Core for LoRaWAN<a name="connect-iot-lorawan-configure-logging"></a>

Before you can monitor and log AWS IoT activity, first enable logging for AWS IoT Core for LoRaWAN resources by using either the CLI or API\.

When considering how to configure your AWS IoT Core for LoRaWAN logging, the default logging configuration determines how AWS IoT activity will be logged unless you specify otherwise\. Starting out, you might want to obtain detailed logs with a default log level of `INFO`\.

After reviewing the initial logs, you can change the default log level to `ERROR`, which is less verbose, and set a more verbose, resource\-specific log level on resources that might need more attention\. Log levels can be changed whenever you want\.

The following topics show how to configure logging for AWS IoT Core for LoRaWAN resources\.

**Topics**
+ [Create logging role and policy for AWS IoT Core for LoRaWAN](connect-iot-lorawan-create-logging-role-policy.md)
+ [Configure logging for AWS IoT Core for LoRaWAN resources](connect-iot-lorawan-configure-resource-logging.md)