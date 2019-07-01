# Step 11: Capture Data from the Soil Moisture Sensor Kit<a name="iot-plant-step11"></a>

In this step, you use the Python programming language to run some code on the Raspberry Pi to capture data from the soil moisture sensor kit\.

1. Use an available code editor on the Raspberry Pi \(for example, nano, IDLE, or vi\) to create a file with the following code\.

   ```
                         
   import RPi.GPIO as GPIO
   import time
   
   # Represents the GPIO21 pin. 
   channel = 21
   
   # Use the GPIO BCM pin numbering scheme.
   GPIO.setmode(GPIO.BCM)
   
   # Receive input signals through the pin.
   GPIO.setup(channel, GPIO.IN)
   
   # Infinite loop to keep this script running.
   while True:
     # 'No water' = 1/True (sensor's microcontroller light is off).
     if GPIO.input(channel):
       print("No water detected")
     else:
       # 'Water' = 0/False (microcontroller light is on).
       print("Water detected!")
   
     # Wait 5 seconds before checking again.
     time.sleep(5)
   
   # Clean things up if for any reason we get to this
   # point before script stops.
   GPIO.cleanup()
   ```

   This code listens every five seconds for input from the soil moisture sensor kit on the GPIO21 BCM pin \(the 40th pin\) on the Raspberry Pi\. If the sensor’s microcontroller light is off, a value of `1` is reported\. If the light is on, a value of `0` is reported\.

1. Save the file with the extension `.py`, for example, `gpio.py`, in the `deviceSDK` folder\. If you choose to use a different name for the `.py` file, be sure to substitute it throughout this sample\.

1. From the command prompt in PuTTY or SSH, or from the terminal in Raspbian, run commands to switch to the `deviceSDK` folder, and then use Python to run the `gpio.py` file, for example, `cd /deviceSDK && python gpio.py`\.

1. Every 5–10 seconds, place the prongs on the sensor module into a glass of water, or remove the prongs from the water\. Every 5 seconds, Python prints `No water detected` or `Water detected!`, depending on whether the prongs are in the water\.

1. When you’re done, stop running the script by pressing **Ctrl\+C**\.