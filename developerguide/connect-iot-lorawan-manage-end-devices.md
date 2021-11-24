# Managing devices with AWS IoT Core for LoRaWAN<a name="connect-iot-lorawan-manage-end-devices"></a>

LoRaWAN devices communicate with AWS IoT Core for LoRaWAN through LoRaWAN gateways\. Adding devices to AWS IoT Core for LoRaWAN lets AWS IoT process the messages received from the devices for use by AWS IoT and other services\.

Following are some important considerations when using your devices with AWS IoT Core for LoRaWAN\. For information about how to add your device to AWS IoT Core for LoRaWAN, see [Onboard your devices to AWS IoT Core for LoRaWAN](connect-iot-lorawan-onboard-end-devices.md)\.

## Device considerations<a name="connect-iot-lorawan-devices-criteria"></a>

When selecting a device that you want to use for communicating with AWS IoT Core for LoRaWAN, consider the following\.
+ Available sensors
+ Battery capacity
+ Energy consumption
+ Cost
+ Antenna type and transmission range

## Using devices with gateways qualified for AWS IoT Core for LoRaWAN<a name="connect-iot-lorawan-devices-qualified-gateways"></a>

The devices that you use can be paired with wireless gateways that are qualified for use with AWS IoT Core for LoRaWAN\. You can find these gateways and developer kits in the [AWS Partner Device Catalog](https://devices.amazonaws.com/search?page=1&sv=iotclorawan)\. We also recommend that you consider proximity of these devices to your gateways\. For more information, see [Using qualified gateways from the AWS Partner Device Catalog](connect-iot-lorawan-manage-gateways.md#connect-iot-lorawan-qualified-gateways)\.

## LoRaWAN version<a name="connect-iot-lorawan-lorawan-version"></a>

AWS IoT Core for LoRaWAN supports all devices that comply to 1\.0\.x or 1\.1 LoRaWAN specifications standardized by LoRa Alliance\.

## Activation modes<a name="connect-iot-lorawan-activation-modes"></a>

Before your LoRaWAN device can send uplink data, you must complete a process called *activation* or *join* procedure\. To activate your device, you can either use OTAA \(Over the air activation\) or ABP \(Activation by personalization\)\. We recommend that you use OTAA to activate your device because new session keys are generated for each activation, which makes it more secure\.

Your wireless device specification is based on the LoRaWAN version and activation mode, which determines the root keys and session keys generated for each activation\. For more information, see [Add your wireless device specification to AWS IoT Core for LoRaWAN using the console](connect-iot-lorawan-end-devices-add.md#connect-iot-lorawan-end-device-spec-console)\.

## Device classes<a name="connect-iot-lorawan-device-classes"></a>

LoRaWAN devices can send uplink messages at any time\. Listening to downlink messages consumes battery capacity and reduces battery duration\. The LoRaWAN protocol specifies three classes of LoRaWAN devices\.
+ Class A devices sleep most of the time and listen for downlink messages only for a short period of time\. These devices are mostly battery\-powered sensors with a battery lifetime of up to 10 years\.
+ Class B devices can receive messages in scheduled downlink slots\. These devices are mostly battery\-powered actuators\.
+ Class C devices never sleep and continuously listen to incoming messages and so there isn't much delay in receiving the messages\. These devices are mostly mains\-powered actuators\.

For more information about these wireless device considerations, refer to the resources mentioned in [Learn more about LoRaWAN](connect-iot-lorawan-what-is-lorawan.md#connect-iot-lorawan-lorawan-learn-more)\.