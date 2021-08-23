# Onboard your devices to AWS IoT Core for LoRaWAN<a name="connect-iot-lorawan-onboard-end-devices"></a>

After you have onboarded your gateway to AWS IoT Core for LoRaWAN and verified its connection status, you can onboard your wireless devices\. For information about how to onboard your gateways, see [Onboard your gateways to AWS IoT Core for LoRaWAN](connect-iot-lorawan-onboard-gateways.md)\.

LoRaWAN devices use a LoRaWAN protocol to exchange data with cloud\-hosted applications\. AWS IoT Core for LoRaWAN supports devices that comply to 1\.0\.x or 1\.1 LoRaWAN specifications standardized by LoRa Alliance\.

A LoRaWAN device typically contains one or more sensors and actors\. The devices send uplink telemetry data through LoRaWAN gateways to AWS IoT Core for LoRaWAN\. Cloud\-hosted applications can control the sensors by sending downlink commands to LoRaWAN devices through LoRaWAN gateways\.

**Before onboarding your wireless device**  
Before you onboard your wireless device to AWS IoT Core for LoRaWAN, you need to have the following information ready in advance:
+ 

**LoRaWAN specification and wireless device configuration**  
Having the configuration parameters that are unique to each device ready to enter in advance makes entering the data into the console go more smoothly\. The specific parameters that you need to enter depend on the LoRaWAN specification that the device uses\. For the complete listing of its specifications and configuration parameters, see each device's documentation\.
+ 

**Device name and description \(optional\)**  
The information in these optional fields comes from how you organize and describe the elements in your wireless system\. For more information about naming and describing your resources, see [Describe your AWS IoT Core for LoRaWAN resources](connect-iot-lorawan-describe-resource.md)\.
+ 

**Device and service profiles**  
Have some wireless device configuration parameters ready that are shared by many devices and can be stored in AWS IoT Core for LoRaWAN as device and service profiles\. The configuration parameters are found in the device's documentation or on the device itself\. You'll want to identify a device profile that matches the configuration parameters of the device, or create one if necessary, before you add the device\. For more information, see [Add profiles to AWS IoT Core for LoRaWAN](connect-iot-lorawan-define-profiles.md)\.
+ 

**AWS IoT Core for LoRaWAN destination**  
Each device must be assigned to a destination that will process its messages to send to AWS IoT and other services\. The AWS IoT rules that process and send the device messages are specific to the device's message format\. To process the messages from the device and send them to the correct service, identify the destination you'll create to use with the device's messages and assign it to the device\.

**Topics**
+ [Add your wireless device to AWS IoT Core for LoRaWAN](connect-iot-lorawan-end-devices-add.md)
+ [Add profiles to AWS IoT Core for LoRaWAN](connect-iot-lorawan-define-profiles.md)
+ [Add destinations to AWS IoT Core for LoRaWAN](connect-iot-lorawan-create-destinations.md)
+ [Create rules to process LoRaWAN device messages](connect-iot-lorawan-destination-rules.md)
+ [Connect your LoRaWAN device and verify its connection status](connect-iot-lorawan-device-connection-status.md)