# Step 10: Set Up the Soil Moisture Sensor Kit<a name="iot-plant-step10"></a>

In this step, you connect the soil moisture sensor kit to the running Raspberry Pi and test it to make sure that the sensor kit works\.

1. Connect two female\-to\-female connector wires from the two pins on the sensor module to the two pins on the microcontroller\. The connector wires can be connected in either order, but be sure to connect to the two and only two pins on the one side of the microcontroller, and not to any of the four pins on the other side of the microcontroller\.  
![\[The wiring connections for the moisture sensor module and the microcontroller.\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/rpi-sensor-2-prong.png)

1. Connect three female\-to\-female connector wires from three specific pins on the microcontroller to three specific pins on the Raspberry Pi, as follows:

   1. Connect from the VCC 5v\+ power pin on the microcontroller to one of the 5V power pins on the Raspberry Pi \(for example, pin 2\)\.

   1. Connect from the GND ground pin on the microcontroller to one of the GND ground pins on the Raspberry Pi \(for example, pin 6\)\.

   1. Connect from the DO digital data pin on the microcontroller to one of the GPIO pins on the Raspberry Pi \(for example, GPIO21 on pin 40\)\.

   1. Do not connect anything to the AO analog data pin on the microcontroller\.

      To see a graphical list of pins and their labels, you can run the `pinout` command on the Raspberry Pi, or go to [https://pinout\.xyz](https://pinout.xyz/)\.  
![\[Wiring connections for the other end of the microcontroller.\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/rpi-sensor-3-prong.png)  
![\[Wiring connections for the gpio on the Raspberry Pi.\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/rpi-gpio.png)

      If the connections are correct, the PWR power LED light is displayed on the microcontroller\.  
![\[Microcontroller with the green power LED lit.\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/rpi-microcontroller-power.png)

      The LED lights on some microcontrollers might display in green while others might display in red\. Both colors mean the same thing\.

1. Place the prongs on the sensor module into a mug of water\. If the sensor detects water, the DO digital data LED light is displayed on the microcontroller\. If the DO LED light isn’t displayed, with the prongs still in the water, use a slotted screwdriver to change the sensitivity of the potentiometer on the microcontroller until the DO light is displayed\. Move the prongs in and out of the water to check whether the DO light goes on and off, adjusting the potentiometer on the microcontroller as needed\.  
![\[When the sensor is out of the water, the sensor light is off.\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/rpi-no-water.png)  
![\[When the sensor is in the water, the sensor light is on.\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/rpi-water.png)  
![\[Potentiometer on the microcontroller.\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/rpi-potentiometer.png)