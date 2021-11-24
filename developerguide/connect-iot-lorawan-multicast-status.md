# Monitor and troubleshoot status of your multicast group and devices in the group<a name="connect-iot-lorawan-multicast-status"></a>

After you've added devices and created your multicast group, open the AWS Management Console\. Navigate to the [Multicast groups](https://console.aws.amazon.com/iot/home#/wireless/multicastGroups) page of the AWS IoT console and choose the multicast group you created to view its details\. You'll see information about the multicast group, the number of devices that have been added, and device status details\. You can use the status information to track progress of your multicast session and troubleshoot any errors\.

## Multicast group status<a name="connect-iot-lorawan-multicast-group-status"></a>

Your multicast group can have one of the following status messages displayed in the AWS Management Console\. 
+ 

**Pending**  
This status indicates that you've created a multicast group but it doesn't yet have a multicast session\. You'll see this status message displayed when your group has been created\. During this time, you can update your multicast group, and associate or disassociate devices with your group\. After the status changes from **Pending**, additional devices can't be added to the group\.
+ 

**Session attempting**  
After your devices have been added successfully to the multicast group, when your group has a scheduled multicast session, you'll see this status message displayed\. During this time, you can't update or add devices to your multicast group\. If you cancel your multicast session, the group status changes to **Pending**\. 
+ 

**In session**  
When it's the earliest session time for your multicast session, you'll see this status message displayed\. A multicast group also continues to be in this state when it's associated with a FUOTA task that has an ongoing firmware update session\.

  If you don't have an associated FUOTA task in session, and if the multicast session is canceled because the session time exceeded the time out or you canceled your multicast session, the group status changes to **Pending**\.
+ 

**Delete waiting**  
If you delete your multicast group, its group status changes to **Delete waiting**\. Deletions are permanent and can't be undone\. This action can take time and the group status will be **Delete\_Waiting** until the multicast group has been deleted\. After your multicast group enters this state, it can't transition to one of the other states\.

## Status of devices in multicast group<a name="connect-iot-lorawan-multicast-device-status"></a>

The devices in your multicast group can have one of the following status messages displayed in the AWS Management Console\. You can hover over each status message to get more information about what it indicates\.
+ 

**Package attempting**  
After your devices have been associated with the multicast group, the device status is **Package attempting**\. This status indicates that AWS IoT Core for LoRaWAN has not yet confirmed whether the device supports multicast setup and operation\.
+ 

**Package unsupported**  
After your devices have been associated with the multicast group, AWS IoT Core for LoRaWAN checks whether your device's firmware is capable of multicast setup and operation\. If your device doesn't have the supported multicast package, its status is **Package unsupported**\. To resolve the error, check whether your device's firmware is capable of multicast setup and operation\.
+ 

**Multicast setup attempting**  
If the devices associated with your multicast group are capable of multicast setup and operation, the status is **Multicast setup attempting**\. This status indicates that the device hasn't completed the multicast setup yet\.
+ 

**Multicast setup ready**  
Your device has completed the multicast setup and has been added to the multicast group\. This status indicates that the devices are ready for a multicast session and a downlink message can be sent to those devices\. The status also indicates when you can use FUOTA to update the firmware of the devices in the group\.
+ 

**Session attempting**  
A multicast session has been scheduled for the devices in your multicast group\. At the start of a multicast group session, the device status is **Session attempting**, and requests are sent for whether a class B or class C distribution window can be initiated for the session\. If the time it takes to set up the multicast session exceeds the timeout or if you cancel the multicast session, the status changes to **Multicast setup done**\.
+ 

**In session**  
This status indicates that a class B or class C distribution window has been initiated and your device has an ongoing multicast session\. During this time, downlink messages can be sent from AWS IoT Core for LoRaWAN to devices in the multicast group\. If you update your session time, it overrides the current session and the status changes to **Session attempting**\. When the session time ends or if you cancel the multicast session, the status changes to **Multicast setup ready**\.

## Next steps<a name="connect-iot-lorawan-multicast-status-next"></a>

Now that you've learned the different statuses of your multicast group and the devices in your group, and how you can troubleshoot any issues such as when a device is not capable of multicast setup, you can schedule a downlink message to be sent to the devices and your multicast group will be in **In session**\. For information about scheduling a downlink message, see [Schedule a downlink message to send to devices in your multicast group](connect-iot-lorawan-multicast-schedule-downlink.md)\.