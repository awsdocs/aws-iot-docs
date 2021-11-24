# Create multicast groups and add devices to the group<a name="connect-iot-lorawan-create-multicast-groups"></a>

You can create multicast groups by using the console or the CLI\. If you're creating your multicast group for the first time, we recommend that you use the console to add your multicast group\. When you want to manage your multicast group and add or remove devices from your group, you can use the CLI\.

After exchanging signaling with the end devices you added, AWS IoT Core for LoRaWAN establishes the shared keys with the end devices and sets up the parameters for the data transfer\.

## Prerequisites<a name="connect-iot-lorawan-create-multicast-groups-prereq"></a>

Before you can create multicast groups and add devices to the group:
+ Prepare your devices for multicast and FUOTA setup by specifying the FUOTA configuration parameters `GenAppKey` and `FPorts`\. For more information, see [Prepare devices for multicast and FUOTA configuration](connect-iot-lorawan-prepare-devices-multicast.md)\.
+ Check whether the devices support class B or class C modes of operation\. Depending on the device class that your device supports, choose a device profile that has one or both **Supports Class B** or **Supports Class C** modes enabled\. For information about device profiles, see [Add profiles to AWS IoT Core for LoRaWAN](connect-iot-lorawan-define-profiles.md)\.

  At the start of the multicast session, a class B or class C distribution window is used to send downlink messages to the devices in your group\.

## Create multicast groups by using the console<a name="connect-iot-lorawan-create-multicast-groups-console"></a>

To create multicast groups by using the console, go to the [Multicast groups](https://console.aws.amazon.com/iot/home#/wireless/multicastGroups) page of the AWS IoT console and choose **Create multicast group**\.

1. 

**Create a multicast group**

   To create your multicast group, specify the multicast properties and tags for your group\.

   1. 

**Specify multicast properties**  
To specify multicast properties, enter the following information for your multicast group\.
      + **Name**: Enter a unique name for your multicast group\. The name must contain only letters, numbers, hyphens, and underscores\. It can't contain spaces\.
      + **Description**: You can provide an optional description for your multicast group\. The description length can be up to 2,048 characters\.

   1. 

**Tags for multicast group**  
You can optionally provide any key\-value pairs as **Tags** for your multicast group\. To continue creating your multicast group, choose **Next**\.

1. 

**Add devices to a multicast group**

   You can add individual devices or a group of devices to your multicast group\. To add devices:

   1. 

**Specify RFRegion**  
Specify the **RFRegion** or frequency band for your multicast group\. The **RFRegion** for your multicast group must match the **RFRegion** of devices that you add to the multicast group\. For more information about the **RFRegion**, see [Consider selection of LoRa frequency bands for your gateways and device connection](connect-iot-lorawan-rfregion-permissions.md#connect-iot-lorawan-frequency-bands)\. 

   1. 

**Select a multicast device class**  
Choose whether you want devices in the multicast group to switch to a class B or class C mode at the start of the multicast session\. A class B session can receive downlink messages at regular downlink slots and a class C session can receive downlink messages at anytime\.

   1. 

**Choose the devices you want to add to the group**  
Choose whether you want to add devices individually or in bulk to the multicast group\.
      + To add devices individually, enter the wireless device ID of each device that you want to add to your group\.
      + To add devices in bulk, you can filter the devices you want to add by device profile or tags\. For device profile, you can add devices with a profile that supports class B, class C, or both device classes\.

   1. To create your multicast group, choose **Create** \.

      The multicast group details and the devices you added appear in the group\. For information about the status of the multicast group and your devices and for troubleshooting any issues, see [Monitor and troubleshoot status of your multicast group and devices in the group](connect-iot-lorawan-multicast-status.md)\.

After creating a multicast group, you can choose **Action** to edit, delete, or add devices to the multicast group\. After you've added the devices, you can schedule a session for the downlink payload to be sent to the devices in your group\.

## Create multicast groups by using the API<a name="connect-iot-lorawan-create-multicast-groups-api"></a>

To create multicast groups and add devices to the group by using the API:

1. 

**Create a multicast group**

   To create your multicast group, use the [https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_CreateMulticastGroup.html](https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_CreateMulticastGroup.html) API operation or the [https://docs.aws.amazon.com/cli/latest/reference/iotwireless/create-multicast-group.html](https://docs.aws.amazon.com/cli/latest/reference/iotwireless/create-multicast-group.html) CLI command\. You can provide an `input.json` file as input to the `create-multicast-group` command\.

   ```
   aws iotwireless create-multicast-group \
       --cli-input-json file://input.json
   ```

   where:

   **Contents of input\.json**

   ```
   {   
      "Description": "Multicast group to send downlink payload and perform FUOTA updates.",
      "LoRaWAN": { 
         "DlClass": "ClassB",
         "RfRegion": "US915"
      },
      "Name": "MC_group_FUOTA"  
   }
   ```

   After you create your multicast group, you can use the following API operations or CLI commands to update, delete, or get information about your multicast groups\.
   + [https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_UpdateMulticastGroup](https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_UpdateMulticastGroup) or [https://docs.aws.amazon.com/cli/latest/reference/iotwireless/update-multicast-group.html](https://docs.aws.amazon.com/cli/latest/reference/iotwireless/update-multicast-group.html) 
   + [https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_UpdateMulticastGroup](https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_UpdateMulticastGroup) or [https://docs.aws.amazon.com/cli/latest/reference/iotwireless/get-multicast-group.html](https://docs.aws.amazon.com/cli/latest/reference/iotwireless/get-multicast-group.html) 
   + [https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_ListMulticastGroups](https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_ListMulticastGroups) or [https://docs.aws.amazon.com/cli/latest/reference/iotwireless/list-multicast-groups.html](https://docs.aws.amazon.com/cli/latest/reference/iotwireless/list-multicast-groups.html) 
   + [https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_DeleteMulticastGroup](https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_DeleteMulticastGroup) or [https://docs.aws.amazon.com/cli/latest/reference/iotwireless/delete-multicast-group.html](https://docs.aws.amazon.com/cli/latest/reference/iotwireless/delete-multicast-group.html) 

1. 

**Add devices to a multicast group**

   You can add devices to your multicast group individually or in bulk\.
   + To add devices in bulk to your multicast group, use the [https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_StartBulkAssociateWirelessDeviceWithMulticastGroup.html](https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_StartBulkAssociateWirelessDeviceWithMulticastGroup.html) API operation or the [https://docs.aws.amazon.com/cli/latest/reference/iotwireless/start-bulk-associate-wireless-device-with-multicast-group.html](https://docs.aws.amazon.com/cli/latest/reference/iotwireless/start-bulk-associate-wireless-device-with-multicast-group.html) CLI command\. To filter the devices you want to associate in bulk to your multicast group, provide a query string\. The following shows how you can add a group of devices that has a device profile with the specified ID linked to it\.

     ```
     aws iotwireless start-bulk-associate-wireless-device-with-multicast-group \
         --id "12abd34e-5f67-89c2-9293-593b1bd862e0" \
         --cli-input-json file://input.json
     ```

     where:

     **Contents of input\.json**

     ```
     {
          "QueryString": "DeviceProfileName: MyWirelessDevice AND DeviceProfileId: d6d8ef8e-7045-496d-b3f4-ebcaa1d564bf",
          "Tags": [
             {
                 "Key": "Multicast",
                 "Value": "ClassB"
             }
         ]
     }
     ```

     Here, `multicast-groups/d6d8ef8e-7045-496d-b3f4-ebcaa1d564bf/bulk` is the URL that's used to associate devices with the group\.
   + To add devices individually to your multicast group, use the [https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_AssociateWirelessDeviceWithMulticastGroup.html](https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_AssociateWirelessDeviceWithMulticastGroup.html) API operation or the [https://docs.aws.amazon.com/cli/latest/reference/iotwireless/associate-wireless-device-with-multicast-group.html](https://docs.aws.amazon.com/cli/latest/reference/iotwireless/associate-wireless-device-with-multicast-group.html) CLI\. Provide the wireless device ID for each device you want to add to your group\.

     ```
     aws iotwireless associate-wireless-device-with-multicast-group \
         --id "12abd34e-5f67-89c2-9293-593b1bd862e0" \
         --wireless-device-id "ab0c23d3-b001-45ef-6a01-2bc3de4f5333"
     ```

   After you create your multicast group, you can use the following API operations or CLI commands to get information about your multicast group or to disassociate devices\.
   + [https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_DisassociateWirelessDeviceFromMulticastGroup.html](https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_DisassociateWirelessDeviceFromMulticastGroup.html) or [https://docs.aws.amazon.com/cli/latest/reference/iotwireless/disassociate-wireless-device-from-multicast-group.html](https://docs.aws.amazon.com/cli/latest/reference/iotwireless/disassociate-wireless-device-from-multicast-group.html) 
   + [https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_StartBulkDisassociateWirelessDeviceFromMulticastGroup.html](https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_StartBulkDisassociateWirelessDeviceFromMulticastGroup.html) or [https://docs.aws.amazon.com/cli/latest/reference/iotwireless/start-bulk-disassociate-wireless-device-from-multicast-group.html](https://docs.aws.amazon.com/cli/latest/reference/iotwireless/start-bulk-disassociate-wireless-device-from-multicast-group.html) 
   + [https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_ListWirelessDevices](https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_ListWirelessDevices) or [https://docs.aws.amazon.com/cli/latest/reference/iotwireless/list-wireless-devices.html](https://docs.aws.amazon.com/cli/latest/reference/iotwireless/list-wireless-devices.html) 
**Note**  
The `ListWirelessDevices` API operation can be used to list wireless devices in general, and wireless devices that are associated with a multicast group or a FUOTA task\.  
To list wireless devices associated with a multicast group, use the `ListWirelessDevices` API operation with `MulticastGroupID` as the filter\.
To list wireless devices associated with a FUOTA task, use the `ListWirelessDevices` API operation with `FuotaTaskID` as the filter\.

## Next steps<a name="connect-iot-lorawan-create-multicast-next-steps"></a>

After you've created a multicast group and added devices, you can continue adding devices and monitor the status of the multicast group and your devices\. If your devices have been added successfully to the group, you can configure and schedule a downlink message to be sent to the devices\. Before you can send a downlink message, your devices' status must be **Multicast setup ready**\. After you schedule a downlink message, the status changes to **Session attempting**\. For more information, see [Schedule a downlink message to send to devices in your multicast group](connect-iot-lorawan-multicast-schedule-downlink.md)\. 

If you want to update the firmware of the devices in the multicast group, you can perform Firmware Updates Over\-The\-Air \(FUOTA\) with AWS IoT Core for LoRaWAN\. For more information, see [Firmware Updates Over\-The\-Air \(FUOTA\) for AWS IoT Core for LoRaWAN devices](connect-iot-lorawan-mc-fuota-overview.md)\. 

If your devices weren't added or if you see an error in the multicast group or device statuses, you can hover over the error to get more information and resolve it\. If you still see an error, for information about how to troubleshoot and resolve the issue, see [Monitor and troubleshoot status of your multicast group and devices in the group](connect-iot-lorawan-multicast-status.md)\. 