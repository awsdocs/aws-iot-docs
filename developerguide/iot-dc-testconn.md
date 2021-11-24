# Tutorial: Demonstrate MQTT message communication with the AWS IoT Device Client<a name="iot-dc-testconn"></a>

This tutorial demonstrates how the AWS IoT Device Client can subscribe to and publish MQTT messages, which are commonly used in IoT solutions\.

**To start this tutorial:**
+ Have your local host computer and Raspberry Pi configured as used in [the previous section](iot-dc-install-dc.md)\.

  If you saved the microSD card image after installing the AWS IoT Device Client, you can use a microSD card with that image with your Raspberry Pi\.
+ If you have run this demo before, review [Step 2: Cleaning up your AWS account after building demos with the AWS IoT Device Client](iot-dc-cleanup.md#iot-dc-cleanup-cloud) to delete all AWS IoT resources that you created in earlier runs to avoid duplicate resource errors\.

This tutorial takes about 45 minutes to complete\.

**When you're finished with this topic:**
+ You'll have demonstrated different ways that your IoT device can subscribe to MQTT messages from AWS IoT and publish MQTT messages to AWS IoT\.

**Required equipment:**
+ Your local development and testing environment from [the previous section](iot-dc-install-dc.md)
+ The Raspberry Pi that you used in [the previous section](iot-dc-install-dc.md)
+ The microSD memory card from the Raspberry Pi that you used in [the previous section](iot-dc-install-dc.md)

**Topics**
+ [Step 1: Prepare the Raspberry Pi to demonstrate MQTT message communication](iot-dc-testconn-provision.md)
+ [Step 2: Demonstrate publishing messages with the AWS IoT Device Client](iot-dc-testconn-publish.md)
+ [Step 3: Demonstrate subscribing to messages with the AWS IoT Device Client](iot-dc-testconn-subscribe.md)