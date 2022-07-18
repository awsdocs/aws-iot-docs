# Onboard your gateways to AWS IoT Core for LoRaWAN<a name="connect-iot-lorawan-onboard-gateways"></a>

If you're using AWS IoT Core for LoRaWAN for the first time, you can add your first LoRaWAN gateway and device by using the console\. 

**Before onboarding your gateway**  
Before you onboard your gateway to AWS IoT Core for LoRaWAN, we recommend that you:
+ Use gateways that are qualified for use with AWS IoT Core for LoRaWAN\. These gateways connect to AWS IoT Core without any additional configuration settings and have a compatible version of the [ LoRa Basics Station](https://lora-developers.semtech.com/build/software/lora-basics/lora-basics-for-gateways) software running on them\. For more information, see [Managing gateways with AWS IoT Core for LoRaWAN](connect-iot-lorawan-manage-gateways.md)\.
+ Consider the naming convention of the resources that you create so that you can more easily manage them\. For more information, see [Describe your AWS IoT Core for LoRaWAN resources](connect-iot-lorawan-describe-resource.md)\.
+ Have the configuration parameters that are unique to each gateway ready to enter in advance, which makes entering the data into the console go more smoothly\. The wireless gateway configuration parameters that AWS IoT requires to communicate with and manage the gateway include the gateway's EUI and its LoRa frequency band\.

**Topics**
+ [Consider frequency band selection and add necessary IAM role](connect-iot-lorawan-rfregion-permissions.md)
+ [Add a gateway to AWS IoT Core for LoRaWAN](connect-iot-lorawan-onboard-gateway-add.md)
+ [Connect your LoRaWAN gateway and verify its connection status](connect-iot-lorawan-gateway-connection-status.md)