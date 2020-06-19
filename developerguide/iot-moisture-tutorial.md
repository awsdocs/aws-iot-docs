# Monitoring Soil Moisture with AWS IoT and Raspberry Pi<a name="iot-moisture-tutorial"></a>

This tutorial shows you how to use a [Raspberry Pi](https://www.raspberrypi.org/), a moisture sensor, and AWS IoT to monitor the soil moisture level for a house plant or garden\. The Raspberry Pi runs code that reads the moisture level and temperature from the sensor and then sends the data to AWS IoT\. You create a rule in AWS IoT that sends an email to an address subscribed to an Amazon SNS topic when the moisture level falls below a threshold\.

**Contents**
+ [Prerequisites](#iot-moisture-prereqs)
+ [Setting Up AWS IoT](iot-moisture-setup.md)
  + [Create the AWS IoT Policy](iot-moisture-policy.md)
  + [Create the AWS IoT Thing, Certificate, and Private Key](iot-moisture-create-thing.md)
  + [Create an Amazon SNS Topic and Subscription](iot-moisture-create-sns-topic.md)
  + [Create an AWS IoT Rule to Send an Email](iot-moisture-create-rule.md)
+ [Setting Up Your Raspberry Pi and Moisture Sensor](iot-moisture-raspi-setup.md)

## Prerequisites<a name="iot-moisture-prereqs"></a>

To complete this tutorial, you need:
+ An AWS account\.
+ An IAM user with administrator permissions\.
+ A development computer running Windows, macOS, Linux, or Unix to access the [AWS IoT console](https://console.aws.amazon.com/iot/home)\.
+ A [Raspberry Pi 3B or 4B](https://www.raspberrypi.org/products/) running [Raspbian Buster](https://www.raspberrypi.org/downloads/raspbian/)\. For installation instructions, see [Installing operating system images](https://www.raspberrypi.org/documentation/installation/installing-images/README.md) on the Rasberry Pi website\. 
+ A monitor, keyboard, mouse, and Wi\-Fi network or Ethernet connection for your Raspberry Pi\.
+ A Raspberry Pi\-compatible moisture sensor\. The sensor used in this tutorial is an [Adafruit STEMMA I2C Capacitive Moisture Sensor](https://www.adafruit.com/product/4026) with a [JST 4\-pin to female socket cable header](https://www.adafruit.com/product/3950)\. 