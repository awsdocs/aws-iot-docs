# Add devices and multicast groups to a FUOTA task and schedule a FUOTA session<a name="connect-iot-lorawan-fuota-add-devices"></a>

After you've created a FUOTA task, you can add devices to your task for which you want to update the firmware\. After your devices have been added successfully to the FUOTA task, you can schedule a FUOTA session to update the device firmware\. 
+ If you have only a small number of devices, you can add those devices directly to your FUOTA task\.
+ If you have a large number of devices that you want to update firmware for, you can add these devices to your multicast groups, and then add the multicast groups to your FUOTA task\. For information about creating and using multicast groups, see [Create multicast groups to send a downlink payload to multiple devices](connect-iot-lorawan-multicast-groups.md)\.

**Note**  
You can add either individual devices or multicast groups to the FUOTA task\. You can't add both devices and multicast groups to the task\.

After you've added your devices or multicast groups, you can start a firmware update session\. AWS IoT Core for LoRaWAN collects the firmware image, fragments the images, and then stores the fragments in an encrypted format\. Your end devices collect the fragments and apply the new firmware image\. The time that it takes for the firmware update depends on the image size and how the images were fragmented\. After the firmware update is complete, the encrypted fragments of the firmware image stored by AWS IoT Core for LoRaWAN is deleted\. You can still find the firmware image in the S3 bucket\.

## Prerequisites<a name="connect-iot-lorawan-fuota-devices-prereq"></a>

Before you can add devices or multicast groups to your FUOTA task, do the following\.
+ You must have already created the FUOTA task and provided your firmware image\. For more information, see [Create FUOTA task and provide firmware image](connect-iot-lorawan-fuota-create-task.md)\.
+ Provision the wireless devices that you want to update the device firmware for\. For more information about onboarding your device, see [Onboard your devices to AWS IoT Core for LoRaWAN](connect-iot-lorawan-onboard-end-devices.md)\.
+ To update the firmware of multiple devices, you can add them to a multicast group\. For more information, see [Create multicast groups to send a downlink payload to multiple devices](connect-iot-lorawan-multicast-groups.md)\.
+ When you onboard the devices to AWS IoT Core for LoRaWAN, specify the FUOTA configuration parameter `FPorts`\. If you're using a LoRaWAN v1\.0\.x device, you must also specify the `GenAppKey`\. For more information about the FUOTA configuration parameters, see [Prepare devices for multicast and FUOTA configuration](connect-iot-lorawan-prepare-devices-multicast.md)\.

## Add devices to a FUOTA task and schedule a FUOTA session by using the console<a name="connect-iot-lorawan-fuota-devices-console"></a>

To add devices or multicast groups and schedule a FUOTA session by using the console, go to the [FUOTA tasks](https://console.aws.amazon.com/iot/home#/wireless/fuotaTasks) tab of the console\. Then, choose the FUOTA task that you want to add devices to and perform the firmware update\.

**Add devices and multicast groups**

1. You can add either individual devices or multicast groups to your FUOTA task\. However, you can't add both individual devices and multicast groups to the same FUOTA task\. To add devices using the by console, do the following\.

   1. In the **FUOTA task details**, choose **Add device**\.

   1. Choose the frequency band or **RFRegion** for the devices you add to the task\. This value must match the **RFRegion** that you chose for the FUOTA task\.

   1. Choose whether you want to add individual devices or multicast groups to the task\.
      + To add individual devices, choose **Add individual devices** and enter the device ID of each device that you want to add to your FUOTA task\.
      + To add multicast groups, choose **Add multicast groups** and add your multicast groups to the task\. You can filter the multicast groups you want to add to the task by using the device profile or tags\. When you filter by device profile, you can choose multicast groups that with devices that have a profile with **Supports Class B** or **Supports Class C** enabled\.

1. 

**Schedule FUOTA session**

   After your devices or multicast groups have been added successfully, you can schedule a FUOTA session\. To schedule a session, do the following\.

   1. Choose the FUOTA task for which you want to update the device firmware and then choose **Schedule FUOTA session**\.

   1. Specify a **Start date** and **Start time** for your FUOTA session\. Make sure that the start time is 30 minutes or later from the current time\.

## Add devices to a FUOTA task and schedule a FUOTA session by using the API<a name="connect-iot-lorawan-fuota-devices-api"></a>

You can use the AWS IoT Wireless API or the CLI to add your wireless devices or multicast groups to your FUOTA task\. You can then schedule a FUOTA session\. 

1. 

**Add devices and multicast groups**

   You can associate either wireless devices or multicast groups with your FUOTA task\.
   + To associate individual devices to your FUOTA task, use the [https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_AssociateWirelessDeviceWithFuotaTask.html](https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_AssociateWirelessDeviceWithFuotaTask.html) API operation or the [https://docs.aws.amazon.com/cli/latest/reference/iotwireless/associate-wireless-device-with-fuota-task.html](https://docs.aws.amazon.com/cli/latest/reference/iotwireless/associate-wireless-device-with-fuota-task.html) CLI command, and provide the `WirelessDeviceID` as input\.

     ```
     aws iotwireless associate-wireless-device-with-fuota-task \
         --id "01a23cde-5678-4a5b-ab1d-33456808ecb2"
         --wireless-device-id "ab0c23d3-b001-45ef-6a01-2bc3de4f5333"
     ```
   + To associate multicast groups to your FUOTA task, use the [https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_AssociateMulticastGroupWithFuotaTask.html](https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_AssociateMulticastGroupWithFuotaTask.html) API operation or the [https://docs.aws.amazon.com/cli/latest/reference/iotwireless/associate-multicast-group-with-fuota-task.html](https://docs.aws.amazon.com/cli/latest/reference/iotwireless/associate-multicast-group-with-fuota-task.html) CLI command, and provide the `MulticastGroupID` as input\.

     ```
     aws iotwireless associate-multicast-group-with-FUOTA-task \
         --id 01a23cde-5678-4a5b-ab1d-33456808ecb2"
         --multicast-group-id
     ```

   After you've associated your wireless devices or multicast group to your FUOTA task, use the following API operations or CLI commands to list your devices or multicast groups or to disassociate them from your task\.
   + [https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_DisassociateWirelessDeviceFromFuotaTask.html](https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_DisassociateWirelessDeviceFromFuotaTask.html) or [https://docs.aws.amazon.com/cli/latest/reference/iotwireless/disassociate-wireless-device-from-fuota-task.html](https://docs.aws.amazon.com/cli/latest/reference/iotwireless/disassociate-wireless-device-from-fuota-task.html) 
   + [https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_DisassociateMulticastGroupFromFuotaTask.html](https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_DisassociateMulticastGroupFromFuotaTask.html) or [https://docs.aws.amazon.com/cli/latest/reference/iotwireless/disassociate-multicast-group-from-fuota-task.html](https://docs.aws.amazon.com/cli/latest/reference/iotwireless/disassociate-multicast-group-from-fuota-task.html) 
   + [https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_ListWirelessDevices.html](https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_ListWirelessDevices.html) or [https://docs.aws.amazon.com/cli/latest/reference/iotwireless/delete-multicast-group.html](https://docs.aws.amazon.com/cli/latest/reference/iotwireless/delete-multicast-group.html) 
   + [https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_ListMulticastGroups.html](https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_ListMulticastGroups.html) or [https://docs.aws.amazon.com/cli/latest/reference/iotwireless/list-multicast-groups.html](https://docs.aws.amazon.com/cli/latest/reference/iotwireless/list-multicast-groups.html) 
**Note**  
The API:  
`ListWirelessDevices` can list wireless devices in general, and devices associated with a multicast group, when `MulticastGroupID` is used as the filter\. The API lists wireless devices associated with a FUOTA task when `FuotaTaskID` is used as the filter\.
`ListMulticastGroups` can list multicast groups in general and multicast groups associated with a FUOTA task when `FuotaTaskID` is used as the filter\.

1. 

**Schedule FUOTA session**

   After your devices or multicast groups have been successfully added to the FUOTA task, you can start a FUOTA session to update the device firmware\. The start time must be 30 minutes or later from the current time\. To schedule a FUOTA session by using the API or CLI, use the [https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_StartFuotaTask.html](https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_StartFuotaTask.html) API operation or the [https://docs.aws.amazon.com/cli/latest/reference/iotwireless/start-fuota-task.html](https://docs.aws.amazon.com/cli/latest/reference/iotwireless/start-fuota-task.html) CLI command\.

   After you've started a FUOTA session, You can no longer add devices or multicast groups to the task\. You can get information about the status of your FUOTA session by using the [https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_GettFuotaTask.html](https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_GettFuotaTask.html) API operation or the [https://docs.aws.amazon.com/cli/latest/reference/iotwireless/get-fuota-task.html](https://docs.aws.amazon.com/cli/latest/reference/iotwireless/get-fuota-task.html) CLI command\.