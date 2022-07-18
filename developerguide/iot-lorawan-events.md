# Event notifications for LoRaWAN resources<a name="iot-lorawan-events"></a>

You can use the AWS Management Console or AWS IoT Wireless API operations to notify you of events for your LoRaWAN devices and gateways\. For information about event notifications and how to enable them, see [Event notifications for AWS IoT Wireless](iot-wireless-event-messages.md) and [Enable events for wireless resources](iot-wireless-control-events.md)\. 

## Event types for LoRaWAN resources<a name="iot-lorawan-event-types"></a>

Events that you can enable for your LoRaWAN resources include:
+ Join events that notify you of join events for your LoRaWAN device\. You'll receive notifications when a device joins with AWS IoT Core for LoRaWAN, or when a rejoin request of type 0 or type 2 is received\.
+ Connection status events that notify you when the connection status of your LoRaWAN gateway changes to connected or disconnected\.

The following sections contain more information about the events for your LoRaWAN resources:

**Topics**
+ [Event types for LoRaWAN resources](#iot-lorawan-event-types)
+ [LoRaWAN join events](iot-lorawan-join-events.md)
+ [Connection status events](iot-lorawan-gateway-events.md)