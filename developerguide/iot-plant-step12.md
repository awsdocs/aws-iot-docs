# Step 12: Send Soil Moisture Sensor Readings to AWS IoT<a name="iot-plant-step12"></a>

In this step, you use the Python programming language to run some code on the Raspberry Pi that captures data from the soil moisture sensor kit and then sends it to AWS IoT\.

To do this, you integrate some of the code that you wrote in the previous step into the code that you wrote in `moisture.py` for [Step 5: Simulate Random Moisture Levels](iot-plant-step5.md)\.

1. Use an available code editor on the Raspberry Pi to open the `moisture.py` file that you created in [Step 5: Simulate Random Moisture Levels](iot-plant-step5.md)\.

1. Add some of the code from the `gpio.py` file that you wrote into the `moisture.py` file, and make a few changes to the existing code in the `moisture.py` file, as indicated between the \#\#\# BEGIN and \#\#\# END comments, as follows\. The final code is listed immediately after the following code to be changed\.

   ```
   from AWSIoTPythonSDK.MQTTLib import AWSIoTMQTTShadowClient
   ### BEGIN INSERTING CODE HERE ###
   import RPi.GPIO as GPIO
   import time
   ### END INSERTING CODE HERE ###
   ### BEGIN REMOVING CODE HERE ###
   import random, time # <- Remove this line of code.
   ### END REMOVING CODE HERE
   
   # A random programmatic shadow client ID.
   SHADOW_CLIENT = "myShadowClient"
   
   # The unique hostname that AWS IoT generated for 
   # this device.
   HOST_NAME = "a1b23cde4fghij-ats.iot.us-east-1.amazonaws.com"
   
   # The relative path to the correct root CA file for AWS IoT, 
   # which you have already saved onto this device.
   ROOT_CA = "AmazonRootCA1.pem"
   
   # The relative path to your private key file that 
   # AWS IoT generated for this device, which you 
   # have already saved onto this device.
   PRIVATE_KEY = "12ab3cd456-private.pem.key"
   
   # The relative path to your certificate file that 
   # AWS IoT generated for this device, which you 
   # have already saved onto this device.
   CERT_FILE = "12ab3cd456-certificate.pem.crt.txt"
   
   # A programmatic shadow handler name prefix.
   SHADOW_HANDLER = "MyRPi"
   
   # Automatically called whenever the shadow is updated.
   def myShadowUpdateCallback(payload, responseStatus, token):
     print()
     print('UPDATE: $aws/things/' + SHADOW_HANDLER + 
       '/shadow/update/#')
     print("payload = " + payload)
     print("responseStatus = " + responseStatus)
     print("token = " + token)
   
   # Create, configure, and connect a shadow client.
   myShadowClient = AWSIoTMQTTShadowClient(SHADOW_CLIENT)
   myShadowClient.configureEndpoint(HOST_NAME, 8883)
   myShadowClient.configureCredentials(ROOT_CA, PRIVATE_KEY,
     CERT_FILE)
   myShadowClient.configureConnectDisconnectTimeout(10)
   myShadowClient.configureMQTTOperationTimeout(5)
   myShadowClient.connect()
   
   # Create a programmatic representation of the shadow.
   myDeviceShadow = myShadowClient.createShadowHandlerWithName(
     SHADOW_HANDLER, True)
   
   ### BEGIN INSERTING CODE HERE. ###
   # Represents the GPIO21 pin on the Raspberry Pi. 
   channel = 21
   
   # Use the GPIO BCM pin numbering scheme.
   GPIO.setmode(GPIO.BCM)
   
   # Receive input signals through the pin.
   GPIO.setup(channel, GPIO.IN)
   ### END INSERTING CODE HERE. ###
   
   ### BEGIN REMOVING CODE HERE. ###
   # Keep generating random test data until this script 
   # stops running.
   # To stop running this script, press Ctrl+C.
   # ^^^ Remove these code comments. 
   ### END REMOVING CODE HERE. ###
   while True:
   
     ### BEGIN REMOVING CODE HERE. ###
     # Generate random True or False test data to represent
     # okay or low moisture levels, respectively.
     moisture = random.choice([True, False])
     # ^^^ These comments and code are no longer needed.
     ### END REMOVING CODE HERE. ###
     
     ### BEGIN REMOVING CODE HERE. ###
     if moisture: # <- Remove this line of code.
     ### END REMOVING CODE HERE. ###
     ### BEGIN ADDING CODE HERE. ###
     if GPIO.input(channel):
     ### END ADDING CODE HERE. ###
       ### BEGIN CHANGING CODE HERE. ###
       myDeviceShadow.shadowUpdate(
         '{"state":{"reported":{"moisture":"low"}}}', 
         myShadowUpdateCallback, 5)
     else:
       myDeviceShadow.shadowUpdate(
         '{"state":{"reported":{"moisture":"okay"}}}', 
         myShadowUpdateCallback, 5)
       # ^^^ Swap 'low' and 'okay' conditions.
       ### END CHANGING CODE HERE. ###
   
     # Wait for this test value to be added.
     time.sleep(60)
   ```

   The final code should look like this: 

   ```
   from AWSIoTPythonSDK.MQTTLib import AWSIoTMQTTShadowClient
   import RPi.GPIO as GPIO
   import time
   
   # A random programmatic shadow client ID.
   SHADOW_CLIENT = "myShadowClient"
   
   # The unique hostname that AWS IoT generated for 
   # this device.
   HOST_NAME = "a1b23cde4fghij-ats.iot.us-east-1.amazonaws.com"
   
   # The relative path to the correct root CA file for AWS IoT, 
   # that you have already saved onto this device.
   ROOT_CA = "AmazonRootCA1.pem"
   
   # The relative path to your private key file that 
   # AWS IoT generated for this device, that you 
   # have already saved onto this device.
   PRIVATE_KEY = "12ab3cd456-private.pem.key"
   
   # The relative path to your certificate file that 
   # AWS IoT generated for this device, that you 
   # have already saved onto this device.
   CERT_FILE = "12ab3cd456-certificate.pem.crt.txt"
   
   # A programmatic shadow handler name prefix.
   SHADOW_HANDLER = "MyRPi"
   
   # Automatically called whenever the shadow is updated.
   def myShadowUpdateCallback(payload, responseStatus, token):
     print()
     print('UPDATE: $aws/things/' + SHADOW_HANDLER + 
       '/shadow/update/#')
     print("payload = " + payload)
     print("responseStatus = " + responseStatus)
     print("token = " + token)
   
   # Create, configure, and connect a shadow client.
   myShadowClient = AWSIoTMQTTShadowClient(SHADOW_CLIENT)
   myShadowClient.configureEndpoint(HOST_NAME, 8883)
   myShadowClient.configureCredentials(ROOT_CA, PRIVATE_KEY,
     CERT_FILE)
   myShadowClient.configureConnectDisconnectTimeout(10)
   myShadowClient.configureMQTTOperationTimeout(5)
   myShadowClient.connect()
   
   # Create a programmatic representation of the shadow.
   myDeviceShadow = myShadowClient.createShadowHandlerWithName(
     SHADOW_HANDLER, True)
   
   # Represents the GPIO21 pin on the Raspberry Pi. 
   channel = 21
   
   # Use the GPIO BCM pin numbering scheme.
   GPIO.setmode(GPIO.BCM)
   
   # Receive input signals through the pin.
   GPIO.setup(channel, GPIO.IN)
   
   while True:
    
     if GPIO.input(channel):
       myDeviceShadow.shadowUpdate(
         '{"state":{"reported":{"moisture":"low"}}}', 
         myShadowUpdateCallback, 5)
     else:
       myDeviceShadow.shadowUpdate(
         '{"state":{"reported":{"moisture":"okay"}}}', 
         myShadowUpdateCallback, 5)
     
     # Wait for this test value to be added.
     time.sleep(60)
   ```
**Note**  
In the preceding code, note that the following values will not match your code:  
*a1b23cde4fghij\-ats\.iot\.us\-east\-1\.amazonaws\.com* will instead be the REST API endpoint that AWS IoT generated for you\. 
*AmazonRootCA1\.pem* will instead be name of the root CA for AWS IoT\.
*12ab3cd456\-private\.pem\.key* will instead be the name of the private key for your device in AWS IoT\.
*12ab3cd456\-certificate\.pem\.crt\.txt* will instead be the name of the root certificate file for your device in AWS IoT\.
The *60* in `time.sleep(60)` will be the number of seconds you want to wait for each new reading to be generated\. The lower this number, the more frequently you might get email alerts\.

1. Save your changes to the `moisture.py` file\.

1. From the command prompt in PuTTY or SSH, or from the terminal in Raspbian, run the following command to use the `pip` program to install the AWS IoT Device SDK for Python on the Raspberry Pi:

   ```
   pip install AWSIoTPythonSDK
   ```

1. Run commands to switch to the `deviceSDK` folder if you're not already there\. Then use Python to run the `moisture.py` file, for example, cd /deviceSDK && python moisture\.py\.

1. Every 45–60 seconds, place the prongs on the sensor module into a glass of water, or remove the prongs from the water\. Every 60 seconds, Python reports `"moisture": "low"` or `"moisture": "okay"` to AWS IoT, depending on whether the prongs are in the water\. Whenever AWS IoT receives a low moisture reading, it triggers a rule that sends an alert to your email address through Amazon SNS\.

1. When you’re done, press **Ctrl\+C** to stop running the script\.

1. You can now replace the glass of water with a common houseplant\. Place the prongs on the sensor module into the houseplant’s soil\. Adjust the potentiometer on the sensor’s microcontroller to get the right sensitivity of soil moisture that you want\.

1. In the `moisture.py` file, change the `60` in `time.sleep(60)` to the number of seconds between times you want to check the soil\. For example, to check once an hour, change `60` to `3600`\. To check once a day, change `60` to `86400`\.

1. Restart the `moisture.py` script by running the command python moisture\.py\.

1. After each interval of seconds in `time.sleep` passes, Python reports `"moisture": "low"` or `"moisture": "okay"` to AWS IoT\. This depends on whether the DO light on the sensor’s microcontroller is off or on, the soil moisture level, and the potentiometer’s sensitivity setting\. Whenever AWS IoT receives a low moisture reading, it triggers a rule that sends an alert to your email address through Amazon SNS\.

1. When you’re done, stop running the script by pressing **Ctrl\+C**\.