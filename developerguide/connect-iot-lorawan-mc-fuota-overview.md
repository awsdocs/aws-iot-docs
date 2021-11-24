# Firmware Updates Over\-The\-Air \(FUOTA\) for AWS IoT Core for LoRaWAN devices<a name="connect-iot-lorawan-mc-fuota-overview"></a>

Use Firmware Updates Over\-The\-Air \(FUOTA\) to deploy firmware updates to AWS IoT Core for LoRaWAN devices\.

Using FUOTA, you can send firmware updates to individual devices or to a group of devices\. You can also send firmware updates to multiple devices by creating a multicast group\. First add your devices to the multicast group, and then send your firmware update image to all those devices\. We recommend that you digitally sign the firmware images so that devices receiving the images can verify that they're coming from the right source\.

With AWS IoT Core for LoRaWAN's FUOTA updates, you can:
+ Deploy new firmware images or delta images to a single device or a group of devices\.
+ Verify the authenticity and integrity of new firmware after it's deployed to devices\.
+ Monitor the progress of a deployment and debug issues in case of a failed deployment\.

AWS IoT Core for LoRaWAN's support for FUOTA and multicast groups is based on the [LoRa Alliance's](https://lora-alliance.org/about-lorawan) following specifications:
+ LoRaWAN Remote Multicast Setup Specification, TS005\-2\.0\.0
+ LoRaWAN Fragmented Data Block Transportation Specification, TS004\-2\.0\.0
+ LoRaWAN Application Layer Clock Synchronization Specification, TS003\-2\.0\.0

**Note**  
AWS IoT Core for LoRaWAN automatically performs the clock synchronization according to the LoRa Alliance specification\. It uses the function `AppTimeReq` to reply the server\-side time to the devices that request it using ClockSync signaling\.

**Topics**
+ [FUOTA process overview](connect-iot-lorawan-fuota-mc-process.md)
+ [Create FUOTA task and provide firmware image](connect-iot-lorawan-fuota-create-task.md)
+ [Add devices and multicast groups to a FUOTA task and schedule a FUOTA session](connect-iot-lorawan-fuota-add-devices.md)
+ [Monitor and troubleshoot the status of your FUOTA task and devices added to the task](connect-iot-lorawan-fuota-status.md)