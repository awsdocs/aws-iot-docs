# Add your wireless device to AWS IoT Core for LoRaWAN<a name="connect-iot-lorawan-end-devices-add"></a>

If you're adding your wireless device for the first time, we recommend that you use the console\. Navigate to the [AWS IoT Core for LoRaWAN](https://console.aws.amazon.com/iot/home#/wireless/landing) **Intro** page of the AWS IoT console, choose **Get started**, and then choose **Add device**\. If you've already added a device, choose **View device** to view the gateway that you added\. If you would like to add more devices, choose **Add device**\.

Alternatively, you can also add wireless devices from the [ Devices](https://console.aws.amazon.com/iot/home#/wireless/devices) page of the AWS IoT console\.

## Add your wireless device specification to AWS IoT Core for LoRaWAN using the console<a name="connect-iot-lorawan-end-device-spec-console"></a>

Choose a **Wireless device specification** based on your activation method and the LoRaWAN version\. Once selected, your data is encrypted with a key that AWS owns and manages for you\. 

**OTAA and ABP activation modes**  
Before your LoRaWAN device can send uplink data, you must complete a process called *activation* or *join procedure*\. To activate your device, you can either use OTAA \(Over the air activation\) or ABP \(Activation by personalization\)\.

ABP doesn't require a join procedure and uses static keys\. When you use OTAA, your LoRaWAN device sends a join request and the Network Server can allow the request\. We recommend that you use OTAA to activate your device because new session keys are generated for each activation, which makes it more secure\.

**LoRaWAN version**  
When you use OTAA, your LoRaWAN device and cloud\-hosted applications share the root keys\. These root keys depend on whether you're using version v1\.0\.x or v1\.1\. v1\.0\.x has only one root key, **AppKey** \(Application Key\) whereas v1\.1 has two root keys, **AppKey** \(Application Key\) and **NwkKey** \(Network Key\)\. The session keys are derived based on the root keys for each activation\. Both the **NwkKey** and **AppKey** are 32\-digit hexadecimal values that your wireless vendor provided\.

**Wireless Device EUIs**  
After you select the **Wireless device specification**, you see the EUI \(Extended Unique Identifier\) parameters for the wireless device displayed on the console\. You can find this information from the documentation for the device or the wireless vendor\.
+ **DevEUI**: 16\-digit hexademical value that is unique to your device and found on the device label or its documentation\.
+ **AppEUI**: 16\-digit hexademical value that is unique to the join server and found in the device documentation\. In LoRaWAN version v1\.1, the **AppEUI** is called as **JoinEUI**\.

For more information about the unique identifiers, session keys, and root keys, refer to the [ LoRa Alliance](https://lora-alliance.org/about-lorawan) documentation\.

## Add your wireless device specification to AWS IoT Core for LoRaWAN by using the API<a name="connect-iot-lorawan-end-device-spec-api"></a>

If you're adding a wireless device using the API, you must create your device profile and service profile first before creating the wireless device\. You'll use the device profile and service profile ID when creating the wireless device\. For information about how to create these profiles using the API, see [Add a device profile by using the API](connect-iot-lorawan-define-profiles.md#connect-iot-lorawan-device-profile-api)\.

The following lists describe the API actions that perform the tasks associated with adding, updating, or deleting a service profile\.

**AWS IoT Wireless API actions for service profiles**
+ [CreateWirelessDevice](https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_CreateWirelessDevice.html)
+ [GetWirelessDevice](https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_GetWirelessDevice.html)
+ [ListWirelessDevices](https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_ListWirelessDevices.html)
+ [ UpdateWirelessDevice](https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_UpdateWirelessDevice.html)
+ [DeleteWirelessDevice](https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_DeleteWirelessDevice.html)

For the complete list of the actions and data types available to create and manage AWS IoT Core for LoRaWAN resources, see the [AWS IoT Wireless API reference](https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/welcome.html)\.

**How to use the AWS CLI to create a wireless device**  
You can use the AWS CLI to create a wireless device by using the [create\-wireless\-device](https://docs.aws.amazon.com/cli/latest/reference/iotwireless/create-device-profile.html) command\. The following example creates a wireless device by using an input\.json file to input the parameters\.

**Note**  
You can also perform this procedure with the API by using the methods in the AWS API that correspond to the CLI commands shown here\. 

**Contents of input\.json**

```
{
    "Description": "My LoRaWAN wireless device"
    "DestinationName": "IoTWirelessDestination"
    "LoRaWAN": {
        "DeviceProfileId": "ab0c23d3-b001-45ef-6a01-2bc3de4f5333",
        "ServiceProfileId": "fe98dc76-cd12-001e-2d34-5550432da100",
        "OtaaV1_1": {
            "AppKey": "3f4ca100e2fc675ea123f4eb12c4a012",
            "JoinEui": "b4c231a359bc2e3d",
            "NwkKey": "01c3f004a2d6efffe32c4eda14bcd2b4"
        },
        "DevEui": "ac12efc654d23fc2"
    },
    "Name": "SampleIoTWirelessThing"
    "Type": LoRaWAN
}
```

You can provide this file as input to the `create-wireless-device` command\.

```
aws iotwireless create-wireless-device \
    --cli-input-json file://input.json
```

For information about the CLIs that you can use, see [AWS CLI reference](https://docs.aws.amazon.com/cli/latest/reference/iotwireless/index.html) 