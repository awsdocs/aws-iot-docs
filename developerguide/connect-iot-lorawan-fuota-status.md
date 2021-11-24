# Monitor and troubleshoot the status of your FUOTA task and devices added to the task<a name="connect-iot-lorawan-fuota-status"></a>

After you have provisioned the wireless devices and created any multicast groups that you might want to use, you can start a FUOTA session by performing the following steps\.

## FUOTA task status<a name="connect-iot-lorawan-fuota-task-status"></a>

Your FUOTA task can have one of the following status messages displayed in the AWS Management Console\. 
+ 

**Pending**  
This status indicates that you've created a FUOTA task, but it doesn't yet have a firmware update session\. You'll see this status message displayed when your task has been created\. During this time, you can update your FUOTA task, and associate or disassociate devices or multicast groups with your task\. After the status changes from **Pending**, additional devices can't be added to the task\.
+ 

**FUOTA session waiting**  
After your devices have been added successfully to the FUOTA task, when your task has a scheduled firmware update session, you'll see this status message displayed\. During this time, you can't update or add devices to your FUOTA session\. If you cancel your FUOTA session, the group status changes to **Pending**\. 
+ 

**In FUOTA session**  
When your FUOTA session begins, you'll see this status message displayed\. The fragmentation session starts and your end devices collect the fragments, reconstruct the firmware image, compare the new firmware version with the original version, and apply the new image\.
+ 

**FUOTA done**  
After your end devices report to AWS IoT Core for LoRaWAN that the new firmware image has been applied, or when the session times out, the FUOTA session is marked as done, and you'll see this status displayed\.

  You'll also see this status displayed in any of the following cases so be sure to check whether the firmware update was applied correctly to the devices\.
  + When the FUOTA task status was **FUOTA session waiting**, and there's an S3 bucket error, such as the link to the image file in the S3 bucket is incorrect or AWS IoT Core for LoRaWAN doesn't have sufficient permissions to access the file in the bucket\.
  + When the FUOTA task status was **FUOTA session waiting**, and there's a request to start a FUOTA session, but a response isn't received from the devices or multicast groups in your FUOTA task\.
  + When the FUOTA task status was **In FUOTA session**, and the devices or multicast groups haven't sent any fragments for a certain time period, which results in the session to timeout\.
+ 

**Delete waiting**  
If you delete your FUOTA task that's in any of the other states, you'll see this status displayed\. A deletion action is permanent and can't be undone\. This action can take time and the task status will be **Delete waiting** until the FUOTA task has been deleted\. After your FUOTA task enters this state, it can't transition to one of the other states\.

## Status of devices in a FUOTA task<a name="connect-iot-lorawan-fuota-device-status"></a>

The devices in your FUOTA task can have one of the following status messages displayed in the AWS Management Console\. You can hover over each status message to get more information about what it indicates\.
+ 

**Initial**  
When it's the start time of your FUOTA session, AWS IoT Core for LoRaWAN checks whether your device has the supported package for the firmware update\. If your device has the supported package, the FUOTA session for the device starts\. The firmware image is fragmented and the fragments are sent to your device\. When you see this status displayed, it indicates that the FUOTA session for the device hasn't started yet\.
+ 

**Package unsupported**  
If the device doesn't have the supported FUOTA package, you'll see this status displayed\. If the firmware update package isn't supported, the FUOTA session for your device can't start\. To resolve this error, check whether your device's firmware can receive firmware updates using FUOTA\.
+ 

**Fragmentation algorithm unsupported**  
At the start of your FUOTA session, AWS IoT Core for LoRaWAN sets up a fragmentation session for your device\. If you see this status displayed, it means that the type of fragmentation algorithm used can't be applied for your device's firmware update\. The error occurs because your device doesn't have the supported FUOTA package\. To resolve this error, check whether your device's firmware can receive firmware updates using FUOTA\.
+ 

**Not enough memory**  
After AWS IoT Core for LoRaWAN sends the image fragments, your end devices collect the image fragments and reconstruct the binary image from these fragments\. This status is displayed when your device doesn't have enough memory to assemble the incoming fragments of the firmware image, which can result in your firmware update session ending prematurely\. To resolve the error, check whether your device's hardware can receive this update\. If your device can't receive this update, use a delta image to update the firmware\.
+ 

**Fragmentation index unsupported**  
The fragmentation index identifies one of the four simultaneously possible fragmentation sessions\. If your device doesn't support the indicated fragmentation index value, this status is displayed \. To resolve this error, do one or more of the following\. 
  + Start a new FUOTA task for the device\. 
  + If the error persists, switch from unicast to multicast mode\.
  + If the error still isn't resolved, check your device firmware\.
+ 

**Memory error**  
This status indicates that your device has experienced a memory error when receiving the incoming fragments from AWS IoT Core for LoRaWAN\. If this error occurs, your device might not be capable of receiving this update\. To resolve the error, check whether your device's hardware can receive this update\. If needed, use a delta image to update the device firmware\.
+ 

**Wrong descriptor**  
Your device doesn't support the indicated descriptor\. The descriptor is a field that describes the file that will be transported during the fragmentation session\. If you see this error, contact [AWS Support Center](https://console.aws.amazon.com/support/home#/)\.
+ 

**Session count replay**  
This status indicates that your device has previously used this session count\. To resolve the error, start a new FUOTA task for the device\.
+ 

**Missing fragments**  
As your device collects the image fragments from AWS IoT Core for LoRaWAN, it reconstructs the new firmware image from the independent, coded fragments\. If your device hasn't received all the fragments, the new image can't be reconstructed, and you'll see this status\. To resolve the error, start a new FUOTA task for the device\.
+ 

**MIC error**  
When your device reconstructs the new firmware image from the collected fragments, it performs a MIC \(Message Integrity Check\) to verify the authenticity of your image and whether it's coming from the right source\. If your device detects a mismatch in the MIC after reassembling the fragments, this status is displayed\. To resolve the error, start a new FUOTA task for the device\.
+ 

**Successful**  
The FUOTA session for your device was successful\.
**Note**  
While this status message indicates that the devices have reconstructed the image from the fragments and verified it, the device firmware might not have been updated when the device reports the status to AWS IoT Core for LoRaWAN\. Check whether your device firmware has been updated\.

## Next steps<a name="connect-iot-lorawan-fuota-device-next"></a>

You've learned about the different statuses of the FUOTA task and its devices and how you can troubleshoot any issues\. For more information about each of these statuses, see the [LoRaWAN Fragmented Data Block Transportation Specification, TS004\-1\.0\.0](https://lora-alliance.org/wp-content/uploads/2020/11/fragmented_data_block_transport_v1.0.0.pdf)\.