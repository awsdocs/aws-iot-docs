# Tutorial: Monitoring soil moisture with AWS IoT and Raspberry Pi<a name="iot-moisture-tutorial"></a>

This tutorial shows you how to use a [Raspberry Pi](https://www.raspberrypi.org/), a moisture sensor, and AWS IoT to monitor the soil moisture level for a house plant or garden\. The Raspberry Pi runs code that reads the moisture level and temperature from the sensor and then sends the data to AWS IoT\. You create a rule in AWS IoT that sends an email to an address subscribed to an Amazon SNS topic when the moisture level falls below a threshold\.

**Note**  
This tutorial might not be up to date\. Some references might have been superseded since this topic was originally published\.

**Contents**
+ [Prerequisites](#iot-moisture-prereqs)
+ [Setting up AWS IoT](iot-moisture-setup.md)
  + [Step 1: Create the AWS IoT policy](iot-moisture-policy.md)
  + [Step 2: Create the AWS IoT thing, certificate, and private key](iot-moisture-create-thing.md)
  + [Step 3: Create an Amazon SNS topic and subscription](iot-moisture-create-sns-topic.md)
  + [Step 4: Create an AWS IoT rule to send an email](iot-moisture-create-rule.md)
+ [Setting up your Raspberry Pi and moisture sensor](iot-moisture-raspi-setup.md)

## Prerequisites<a name="iot-moisture-prereqs"></a>

To complete this tutorial, you need:
+ An AWS account\.
+ An IAM user with administrator permissions\.
+ A development computer running Windows, macOS, Linux, or Unix to access the [AWS IoT console](https://console.aws.amazon.com/iot/home)\.
+ A [Raspberry Pi 3B or 4B](https://www.raspberrypi.org/products/) running the latest [Raspbian OS](https://www.raspberrypi.org/software/operating-systems/)\. For installation instructions, see [Installing operating system images](https://www.raspberrypi.org/documentation/installation/installing-images/README.md) on the Rasberry Pi website\. 
+ A monitor, keyboard, mouse, and Wi\-Fi network or Ethernet connection for your Raspberry Pi\.
+ One of the following moisture sensors:
  + An [Adafruit STEMMA I2C Capacitive Moisture Sensor](https://www.adafruit.com/product/4026) with a [JST 4\-pin to female socket cable header](https://www.adafruit.com/product/3950)\.
  + A [Vegitronix VH400](https://vegetronix.com/soil-moisture-sensor) with an [Adafruit ADS1115 16-bit ADC](https://www.adafruit.com/product/1085). The VH400 outputs a voltage that varies relative to moisture, and this needs to be converted to a digital value for the RPi.  This option will require some light soldering and breadboard work.
