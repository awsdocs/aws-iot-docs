# Add your wireless device to AWS IoT Core for LoRaWAN<a name="connect-iot-lorawan-end-devices-add"></a>

If you're adding your wireless device for the first time, we recommend that you use the console\. Navigate to the [AWS IoT Core for LoRaWAN](https://console.aws.amazon.com/iot/home#/wireless/landing) **Intro** page of the AWS IoT console, choose **Get started**, and then choose **Add device**\. If you've already added a device, choose **View device** to view the gateway that you added\. If you would like to add more devices, choose **Add device**\. You can also add wireless devices from the [ Devices](https://console.aws.amazon.com/iot/home#/wireless/devices) page of the AWS IoT console\.

## Add your wireless device specification to AWS IoT Core for LoRaWAN<a name="connect-iot-lorawan-end-device-spec"></a>

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

## Add device and service profiles to AWS IoT Core for LoRaWAN<a name="connect-iot-lorawan-define-profiles"></a>

Device and service profiles can be defined to describe common device configurations\. These profiles describe configuration parameters that are shared by devices to make it easier to add those devices\. AWS IoT Core for LoRaWAN supports device profiles and service profiles\.

 The configuration parameters and the values to enter into these profiles are provided by the device's manufacturer\.

**Device profiles**  
Device profiles define the device capabilities and boot parameters that the network server uses to set the LoRaWAN radio access service\. It includes selection of parameters such as LoRa frequency band, LoRa regional parameters version, and MAC version of the device\. To learn about the different frequency bands, see [Consider selection of LoRa frequency bands for your gateways and device connection](connect-iot-lorawan-rfregion-permissions.md#connect-iot-lorawan-frequency-bands)\.

You can choose from default device profiles or create a new device profile\. We recommend that you use the default device profiles\. If your application requires you to create a device profile, provide a **Device profile name**, select a **Frequency band \(RfRegion\)** that you're using for the device and gateway, and leave other settings to the default values, unless specified otherwise in the device documentation\.

**Service profiles**  
Service profiles describe the communication parameters the device needs to communicate with the application server\. We recommend that you leave the setting **AddGWMetaData** enabled so that you'll receive additional gateway metadata for each payload, such as RSSI and SNR for the data transmission\.