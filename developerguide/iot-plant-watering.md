# AWS IoT Plant Watering Sample<a name="iot-plant-watering"></a>

This hands\-on sample demonstrates how to use AWS IoT to continually detect the current soil moisture level for a common houseplant\. Whenever the moisture level gets too low, an email alert is sent to the houseplant’s owner as a reminder to water it\.

To get real soil moisture readings, this sample uses hardware such as a [Raspberry Pi](https://www.raspberrypi.org/) and a soil moisture sensor kit, and a common houseplant\. If you don’t have this hardware or the houseplant, you can simulate the soil moisture readings by generating random readings from your development computer instead\. This sample shows you both approaches\.

**Contents**
+ [Module 1: Setting Up AWS IoT and Sending Data with Your Development Computer](iot-plant-module1.md)
  + [Prerequisites for Steps 1–5](iot-plant-module1.md#iot-plant-step1-prereqs)
  + [Step 1: Create the AWS IoT Policy](iot-plant-step1.md)
  + [Step 2: Create the Thing](iot-plant-step2.md)
  + [Step 3: Send and Receive Test Data for the Thing](iot-plant-step3.md)
  + [Step 4: Set Up Email Alerts for Low Moisture Readings](iot-plant-step4.md)
  + [Step 5: Simulate Random Moisture Levels](iot-plant-step5.md)
+ [Module 2: Sending Data with the Raspberry Pi](iot-plant-module2.md)
  + [Prerequisites for Steps 6–12](iot-plant-module2.md#w3aac17b7c11b3)
    + [Prerequisites for Steps 6–12](iot-plant-module2.md#w3aac17b7c11b3b5)
  + [Step 6: Begin Preparing the microSDHC Card](iot-plant-step6.md)
  + [Step 7: Download Raspbian to the microSDHC Card](iot-plant-step7.md)
  + [Step 8: Finish Preparing the microSDHC card](iot-plant-step8.md)
  + [Step 9: Connect to the Raspberry Pi and Set Up Raspbian](iot-plant-step9.md)
  + [Step 10: Set Up the Soil Moisture Sensor Kit](iot-plant-step10.md)
  + [Step 11: Capture Data from the Soil Moisture Sensor Kit](iot-plant-step11.md)
  + [Step 12: Send Soil Moisture Sensor Readings to AWS IoT](iot-plant-step12.md)
+ [Cleaning Up](iot-plant-cleanup.md)
+ [Next Steps](iot-plant-next-steps.md)