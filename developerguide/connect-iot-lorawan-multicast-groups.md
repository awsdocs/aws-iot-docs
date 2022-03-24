# Create multicast groups to send a downlink payload to multiple devices<a name="connect-iot-lorawan-multicast-groups"></a>

To send a downlink payload to multiple devices, create a multicast group\. Using multicast, a source can send data to a single multicast address, which is then distributed to an entire group of recipient devices\.

Devices in a multicast group share the same multicast address, session keys, and frame counter\. By using the same session keys, devices in a multicast group can decrypt the message when a downlink transmission is initiated\. A multicast group only supports downlink\. It doesn't confirm whether the downlink payload has been received by the devices\.

With AWS IoT Core for LoRaWAN's multicast groups, you can:
+ Filter your list of devices by using the device profile, RFRegion, or device class, and then add these devices to a multicast group\.
+ Schedule and send one or more downlink payload messages to devices in a multicast group, within a 48\-hour distribution window\. 
+ Have devices temporarily switch to Class B or class C mode at the start of your multicast session for receiving the downlink message\.
+ Monitor your multicast group setup and the state of its devices, and also troubleshoot any issues\.
+ Use Firmware Updates\-Over\-The\-Air \(FUOTA\) to securely deploy firmware updates to devices in a multicast group\.

AWS IoT Core for LoRaWAN's support for FUOTA and multicast groups is based on the [LoRa Alliance's](https://lora-alliance.org/about-lorawan) following specifications:
+ LoRaWAN Remote Multicast Setup Specification, TS005\-2\.0\.0
+ LoRaWAN Fragmented Data Block Transportation Specification, TS004\-2\.0\.0
+ LoRaWAN Application Layer Clock Synchronization Specification, TS003\-2\.0\.0

**Note**  
AWS IoT Core for LoRaWAN automatically performs the clock synchronization for the device according to the LoRa Alliance specification\. Using function `AppTimeReq`, it replies the server\-side time to the devices that request it using ClockSync signaling\.

The following shows how to create your multicast group and schedule a downlink message\.

**Topics**
+ [Prepare devices for multicast and FUOTA configuration](connect-iot-lorawan-prepare-devices-multicast.md)
+ [Create multicast groups and add devices to the group](connect-iot-lorawan-create-multicast-groups.md)
+ [Monitor and troubleshoot status of your multicast group and devices in the group](connect-iot-lorawan-multicast-status.md)
+ [Schedule a downlink message to send to devices in your multicast group](connect-iot-lorawan-multicast-schedule-downlink.md)