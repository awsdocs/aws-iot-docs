# Manage communication between your LoRaWAN devices and AWS IoT<a name="connect-iot-lorawan-device-cloud-communication"></a>

After you've connected your LoRaWAN device to AWS IoT Core for LoRaWAN, your devices can start sending messages to the cloud\. Uplink messages are messages that are sent from your device and received by AWS IoT Core for LoRaWAN\. Your LoRaWAN devices can send uplink messages at any time, which are then forwarded to other AWS services and cloud\-hosted applications\. Messages that are sent from AWS IoT Core for LoRaWAN and other AWS services and applications to your devices are called downlink messages\.

The following shows how you can view and manage uplink and downlink messages that are sent between your devices and the Cloud\. You can maintain a queue of downlink messages and send these messages to your devices in the order in which they were added to the queue\.

**Topics**
+ [View format of uplink messages sent from LoRaWAN devices](connect-iot-lorawan-uplink-metadata-format.md)
+ [Queue downlink messages to send to LoRaWAN devices](connect-iot-lorawan-downlink-queue.md)