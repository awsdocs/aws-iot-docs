# Module 2: Sending Data with the Raspberry Pi<a name="iot-plant-module2"></a>

## Prerequisites for Steps 6–12<a name="w3aac17b7c11b3"></a>

In [Module 1: Setting Up AWS IoT and Sending Data with Your Development Computer](iot-plant-module1.md), you used your development computer to simulate soil moisture readings by generating random data, and then pushed those simulated readings into AWS IoT\. In Part 2 \(Steps 6–12\), you generate real soil moisture readings with a Raspberry Pi and then push those real readings into AWS IoT instead\.

### Prerequisites for Steps 6–12<a name="w3aac17b7c11b3b5"></a>
+ Complete all of the steps in [Module 1: Setting Up AWS IoT and Sending Data with Your Development Computer](iot-plant-module1.md)\.
+ A Raspberry Pi 3\. This sample was tested with a [Raspberry Pi 3 Model B](https://www.raspberrypi.org/products/raspberry-pi-3-model-b/)\.  
![\[A Raspberry Pi 3.\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/rpi-motherboard.png)
+ A Raspberry Pi 3 [micro USB adapter power supply](https://www.raspberrypi.org/documentation/hardware/raspberrypi/power/README.md) with at least 5V 2\.5A\. This sample was tested with a 5V 2\.5A power supply\.  
![\[Raspberry Pi 3 micro USB adapter power supply.\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/rpi-power.png)
+ A [micro SD card](https://www.raspberrypi.org/documentation/installation/sd-cards.md) with at least 16 GB\. This sample was tested with a microSDHC 16 GB card\.  
![\[Micro SD card.\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/rpi-microsdhc.png)
+ A desktop or laptop development computer with either a slot for the micro SD card or a micro SD card reader that can connect to the computer\. This sample was tested with a laptop running Windows 10 Enterprise that has a built\-in SD card reader\.
+ A network to connect the Raspberry Pi to AWS IoT and, optionally, to connect your development computer to the Raspberry Pi\. This setup can be either a wireless network, or a physical network router that you can connect to with Ethernet cables\. The Raspberry Pi 3 Model B provides both built\-in Wi\-Fi and an Ethernet port\. This sample was tested with a wireless network\.
+ If you don’t want to access the Raspberry Pi from your development computer, you need to connect the Raspberry Pi to a separate keyboard, mouse, and monitor\. The Raspberry Pi 3 Model B provides four USB ports and a full\-size HDMI port\. This sample was tested with a USB keyboard, a USB mouse, and a monitor with HDMI input\.
+ A [soil moisture sensor kit](https://www.instructables.com/id/Soil-Moisture-Sensor-Raspberry-Pi/) compatible with Raspberry Pi\. This includes the sensor module \(the “probe”\) and a microcontroller\. You also need two female\-to\-female connection wires from the sensor module to the microcontroller, and three female\-to\-female connection wires from the microcontroller to the Raspberry Pi’s onboard GPIO pins\. This sample was tested with a Gikfun soil moisture sensor\.  
![\[A soil moisture sensor kit.\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/rpi-sensor-kit.png)
+ A glass of water\.
+ A common houseplant\.