# Schedule a downlink message to send to devices in your multicast group<a name="connect-iot-lorawan-multicast-schedule-downlink"></a>

After you've successfully added devices to your multicast group, you can start a multicast session and configure a downlink message to be sent to those devices\. The downlink message must be scheduled within 48 hours and the start time for the multicast must be at least 30 minutes later than the current time\.

**Note**  
Devices in a multicast group can't acknowledge when a downlink message has been received\.

## Prerequisites<a name="connect-iot-lorawan-multicast-downlink-prereq"></a>

Before you can send a downlink message, you must have created a multicast group and successfully added devices to the group for which you want to send a downlink message\. You can't add more devices after a start time has been scheduled for your multicast session\. For more information, see [Create multicast groups and add devices to the group](connect-iot-lorawan-create-multicast-groups.md)\.

If any of the devices weren't added successfully, the multicast group and device status will contain information to help you resolve the errors\. If the errors still persist, for information about troubleshooting these errors, see [Monitor and troubleshoot status of your multicast group and devices in the group](connect-iot-lorawan-multicast-status.md)\.

## Schedule a downlink message by using the console<a name="connect-iot-lorawan-multicast-downlink-console"></a>

To send a downlink message by using the console, go to the [Multicast groups](https://console.aws.amazon.com/iot/home#/wireless/multicastGroups) page of the AWS IoT console and choose the multicast group you created\. In the multicast group details page, choose **Schedule downlink message** and then choose **Schedule downlink session**\.

1. 

**Schedule downlink message window**

   You can set up a time window for a downlink message to be sent to the devices in your multicast group\. The downlink message must be scheduled within 48 hours\.

   To schedule your multicast session, specify the following parameters:
   + **Start date** and **Start time**: The start date and time must be at least 30 minutes after and 48 hours before the current time\.
**Note**  
The time that you specify is in UTC so consider checking the time difference with your time zone when scheduling the downlink window\. 
   + **Session timeout**: The time after which you want the multicast session to timeout if no downlink message has been received\. The minimum timeout allowed is 60 seconds\. The maximum timeout value is 2 days for class B multicast groups and 18 hours for class C multicast groups\.

1. 

**Configure your downlink message**

   To configure your downlink message, specify the following parameters:
   + **Data rate**: Choose a data rate for your downlink message\. The data rate depends on RFRegion and payload size\. The default data rate is 8 for the US915 region and 0 for the EU868 region\.
   + **Frequency**: Choose a frequency for sending your downlink message\. To avoid messaging conflicts, choose an available frequency depending on the RFRegion\.
   + **FPort**: Choose an available frequency port for sending the downlink message to your devices\.
   + **Payload**: Specify the maximum size of your payload depending on the data rate\. Using the default data rate, you can have a maximum payload size of 33 bytes in the US915 RfRegion and 51 bytes in the EU868 RfRegion\. Using larger data rates, you can transfer up to a maximum payload size of 242 bytes\.

   To schedule your downlink message, choose **Schedule**\.

## Schedule a downlink message by using the API<a name="connect-iot-lorawan-multicast-downlink-api"></a>

To schedule a downlink message by using the API, use the [https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_StartMulticastGroupSession.html](https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_StartMulticastGroupSession.html) API operation or the [https://docs.aws.amazon.com/cli/latest/reference/iotwireless/start-multicast-group-session](https://docs.aws.amazon.com/cli/latest/reference/iotwireless/start-multicast-group-session) CLI command\.

You can use the following API operations or CLI commands to get information about a multicast group and to delete a multicast group\.
+ [https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_GetMulticastGroupSession.html](https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_GetMulticastGroupSession.html) or [https://docs.aws.amazon.com/cli/latest/reference/iotwireless/get-multicast-group-session](https://docs.aws.amazon.com/cli/latest/reference/iotwireless/get-multicast-group-session)
+ [https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_DeleteMulticastGroupSession.html](https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_DeleteMulticastGroupSession.html) or [https://docs.aws.amazon.com/cli/latest/reference/iotwireless/delete-multicast-group-session](https://docs.aws.amazon.com/cli/latest/reference/iotwireless/delete-multicast-group-session)

To send data to a multicast group after the session has been started, use the [https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_SendDataToMulticastGroup.html](https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_SendDataToMulticastGroup.html) API operation or the [https://docs.aws.amazon.com/cli/latest/reference/iotwireless/send-data-to-multicast-group](https://docs.aws.amazon.com/cli/latest/reference/iotwireless/send-data-to-multicast-group) CLI command\.

## Next steps<a name="connect-iot-lorawan-multicast-downlink-next"></a>

After you've configured a downlink message to be sent to the devices, the message is sent at the start of the session\. The devices in a multicast group can't confirm whether the message has been received\.

### Configure additional downlink messages<a name="connect-iot-lorawan-multicast-downlink-additional"></a>

You can also configure additional downlink messages to be sent to the devices in your multicast group:
+ To configure additional downlink messages from the console:

  1. Go to the [Multicast groups](https://console.aws.amazon.com/iot/home#/wireless/multicastGroups) page of the AWS IoT console and choose the multicast group you created\.

  1. In the multicast group details page, choose **Schedule downlink message** and then choose **Configure additional downlink message**\.

  1. Specify the parameters **Data rate**, **Frequency**, **FPort**, and **Payload**, similar to how you configured these parameters for your first downlink message\.
+ To configure additional downlink messages using the API or CLI, call the [https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_SendDataToMulticastGroup.html](https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_SendDataToMulticastGroup.html) API operation or the [https://docs.aws.amazon.com/cli/latest/reference/iotwireless/send-data-to-multicast-group](https://docs.aws.amazon.com/cli/latest/reference/iotwireless/send-data-to-multicast-group) CLI command for each additional downlink message\.

### Update session schedule<a name="connect-iot-lorawan-multicast-downlink-update"></a>

You can also update the session schedule to use a new start date and time for your multicast session\. The new session schedule will override the previously scheduled session\.

**Note**  
Update your multicast session only when required\. These updates can cause a group of devices to wake up for a long duration and drain the battery\.
+ To update the session schedule from the console:

  1. Go to the [Multicast groups](https://console.aws.amazon.com/iot/home#/wireless/multicastGroups) page of the AWS IoT console and choose the multicast group you created\.

  1. In the multicast group details page, choose **Schedule downlink message** and then choose **Update session schedule**\. 

  1. Specify the parameters **State date**, **Start time**, and **Session timeout**, similar to how you specified these parameters for your first downlink message\.
+ To update the session schedule from the API or CLI, use the [https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_StartMulticastGroupSession.html](https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_StartMulticastGroupSession.html) API operation or the [https://docs.aws.amazon.com/cli/latest/reference/iotwireless/start-multicast-group-session](https://docs.aws.amazon.com/cli/latest/reference/iotwireless/start-multicast-group-session) CLI command\.