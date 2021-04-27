# Add profiles to AWS IoT Core for LoRaWAN<a name="connect-iot-lorawan-define-profiles"></a>

Device and service profiles can be defined to describe common device configurations\. These profiles describe configuration parameters that are shared by devices to make it easier to add those devices\. AWS IoT Core for LoRaWAN supports device profiles and service profiles\.

 The configuration parameters and the values to enter into these profiles are provided by the device's manufacturer\.

**Device profiles**  
Device profiles define the device capabilities and boot parameters that the network server uses to set the LoRaWAN radio access service\. It includes selection of parameters such as LoRa frequency band, LoRa regional parameters version, and MAC version of the device\. To learn about the different frequency bands, see [Adding LoRaWAN gateways and devices](connect-iot-lorawan.md#connect-iot-lorawan-getting-started-overview)\.

When you onboard your wireless device by using the [ Wireless connectivity hub of the AWS IoT console](https://console.aws.amazon.com/iot/home/#/wireless/onboarding), you can choose from default device profiles or create a new device profile\.

**Service profiles**  
Service profiles describe the communication parameters the device needs to communicate with the application server\.