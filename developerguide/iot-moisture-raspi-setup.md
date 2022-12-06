# Setting up your Raspberry Pi and Adafruit STEMMA Soil Sensor<a name="iot-stemma-raspi-setup"></a>

Insert your microSD card into the Raspberry Pi, connect your monitor, keyboard, mouse, and, if you're not using Wi\-Fi, Ethernet cable\. Do not connect the power cable yet\.

Connect the JST jumper cable to the moisture sensor\. The other side of the jumper has four wires:
+ Green: I2C SCL
+ White: I2C SDA
+ Red: power \(3\.5 V\)
+ Black: ground

Hold the Raspberry Pi with the Ethernet jack on the right\. In this orientation, there are two rows of GPIO pins at the top\. Connect the wires from the moisture sensor to the bottom row of pins in the following order\. Starting at the left\-most pin, connect red \(power\), white \(SDA\), and green \(SCL\)\. Skip one pin, and then connect the black \(ground\) wire\. For more information, see [Python Computer Wiring](https://learn.adafruit.com/adafruit-stemma-soil-sensor-i2c-capacitive-moisture-sensor/python-circuitpython-test)\.

Attach the power cable to the Raspberry Pi and plug the other end into a wall socket to turn it on\.

**Configure your Raspberry Pi**

1. On **Welcome to Raspberry Pi**, choose **Next**\.

1. Choose your country, language, timezone, and keyboard layout\. Choose **Next**\.

1. Enter a password for your Raspberry Pi, and then choose **Next**\.

1. Choose your Wi\-Fi network, and then choose **Next**\. If you aren't using a Wi\-Fi network, choose **Skip**\.

1. Choose **Next** to check for software updates\. When the updates are complete, choose **Restart** to restart your Raspberry Pi\.

After your Raspberry Pi starts up, enable the I2C interface\.

1. In the upper left corner of the Raspbian desktop, click the Raspberry icon, choose **Preferences**, and then choose **Raspberry Pi Configuration**\.

1. On the **Interfaces** tab, for **I2C**, choose **Enable**\.

1. Choose **OK**\.

The libraries for the Adafruit STEMMA moisture sensor are written for CircuitPython\. To run them on a Raspberry Pi, you need to install the latest version of Python 3\.

1. Run the following commands from a command prompt to update your Raspberry Pi software:

   `sudo apt-get update`

   `sudo apt-get upgrade`

1. Run the following command to update your Python 3 installation:

   `sudo pip3 install --upgrade setuptools`

1. Run the following command to install the Raspberry Pi GPIO libraries:

   `pip3 install RPI.GPIO`

1. Run the following command to install the Adafruit Blinka libraries:

   `pip3 install adafruit-blinka`

   For more information, see [Installing CircuitPython Libraries on Raspberry Pi](https://learn.adafruit.com/circuitpython-on-raspberrypi-linux/installing-circuitpython-on-raspberry-pi)\.

1. Run the following command to install the Adafruit Seesaw libraries:

   `sudo pip3 install adafruit-circuitpython-seesaw`

1. Run the following command to install the AWS IoT Device SDK for Python:

   `pip3 install AWSIoTPythonSDK`
   
Your Raspberry Pi now has all of the required libraries\. Create a file called **moistureSensor\.py** and copy the following Python code into the file:

```

from adafruit_seesaw.seesaw import Seesaw
from AWSIoTPythonSDK.MQTTLib import AWSIoTMQTTShadowClient
from board import SCL, SDA

import logging
import time
import json
import argparse
import busio

# Shadow JSON schema:
#
# {
#   "state": {
#       "desired":{
#           "moisture":<INT VALUE>,
#           "temp":<INT VALUE>            
#       }
#   }
# }

# Function called when a shadow is updated
def customShadowCallback_Update(payload, responseStatus, token):

    # Display status and data from update request
    if responseStatus == "timeout":
        print("Update request " + token + " time out!")

    if responseStatus == "accepted":
        payloadDict = json.loads(payload)
        print("~~~~~~~~~~~~~~~~~~~~~~~")
        print("Update request with token: " + token + " accepted!")
        print("moisture: " + str(payloadDict["state"]["reported"]["moisture"]))
        print("temperature: " + str(payloadDict["state"]["reported"]["temp"]))
        print("~~~~~~~~~~~~~~~~~~~~~~~\n\n")

    if responseStatus == "rejected":
        print("Update request " + token + " rejected!")

# Function called when a shadow is deleted
def customShadowCallback_Delete(payload, responseStatus, token):

     # Display status and data from delete request
    if responseStatus == "timeout":
        print("Delete request " + token + " time out!")

    if responseStatus == "accepted":
        print("~~~~~~~~~~~~~~~~~~~~~~~")
        print("Delete request with token: " + token + " accepted!")
        print("~~~~~~~~~~~~~~~~~~~~~~~\n\n")

    if responseStatus == "rejected":
        print("Delete request " + token + " rejected!")


# Read in command-line parameters
def parseArgs():

    parser = argparse.ArgumentParser()
    parser.add_argument("-e", "--endpoint", action="store", required=True, dest="host", help="Your device data endpoint")
    parser.add_argument("-r", "--rootCA", action="store", required=True, dest="rootCAPath", help="Root CA file path")
    parser.add_argument("-c", "--cert", action="store", dest="certificatePath", help="Certificate file path")
    parser.add_argument("-k", "--key", action="store", dest="privateKeyPath", help="Private key file path")
    parser.add_argument("-p", "--port", action="store", dest="port", type=int, help="Port number override")
    parser.add_argument("-n", "--thingName", action="store", dest="thingName", default="Bot", help="Targeted thing name")
    parser.add_argument("-id", "--clientId", action="store", dest="clientId", default="basicShadowUpdater", help="Targeted client id")

    args = parser.parse_args()
    return args


# Configure logging
# AWSIoTMQTTShadowClient writes data to the log
def configureLogging():

    logger = logging.getLogger("AWSIoTPythonSDK.core")
    logger.setLevel(logging.DEBUG)
    streamHandler = logging.StreamHandler()
    formatter = logging.Formatter('%(asctime)s - %(name)s - %(levelname)s - %(message)s')
    streamHandler.setFormatter(formatter)
    logger.addHandler(streamHandler)


# Parse command line arguments
args = parseArgs()

if not args.certificatePath or not args.privateKeyPath:
    parser.error("Missing credentials for authentication.")
    exit(2)

# If no --port argument is passed, default to 8883
if not args.port: 
    args.port = 8883


# Init AWSIoTMQTTShadowClient
myAWSIoTMQTTShadowClient = None
myAWSIoTMQTTShadowClient = AWSIoTMQTTShadowClient(args.clientId)
myAWSIoTMQTTShadowClient.configureEndpoint(args.host, args.port)
myAWSIoTMQTTShadowClient.configureCredentials(args.rootCAPath, args.privateKeyPath, args.certificatePath)

# AWSIoTMQTTShadowClient connection configuration
myAWSIoTMQTTShadowClient.configureAutoReconnectBackoffTime(1, 32, 20)
myAWSIoTMQTTShadowClient.configureConnectDisconnectTimeout(10) # 10 sec
myAWSIoTMQTTShadowClient.configureMQTTOperationTimeout(5) # 5 sec

# Initialize Raspberry Pi's I2C interface
i2c_bus = busio.I2C(SCL, SDA)

# Intialize SeeSaw, Adafruit's Circuit Python library
ss = Seesaw(i2c_bus, addr=0x36)

# Connect to AWS IoT
myAWSIoTMQTTShadowClient.connect()

# Create a device shadow handler, use this to update and delete shadow document
deviceShadowHandler = myAWSIoTMQTTShadowClient.createShadowHandlerWithName(args.thingName, True)

# Delete current shadow JSON doc
deviceShadowHandler.shadowDelete(customShadowCallback_Delete, 5)

# Read data from moisture sensor and update shadow
while True:

    # read moisture level through capacitive touch pad
    moistureLevel = ss.moisture_read()

    # read temperature from the temperature sensor
    temp = ss.get_temp()

    # Display moisture and temp readings
    print("Moisture Level: {}".format(moistureLevel))
    print("Temperature: {}".format(temp))
    
    # Create message payload
    payload = {"state":{"reported":{"moisture":str(moistureLevel),"temp":str(temp)}}}

    # Update shadow
    deviceShadowHandler.shadowUpdate(json.dumps(payload), customShadowCallback_Update, 5)
    time.sleep(1)
    
# Setting up your Raspberry Pi and Vegitronix VH400 Moisture Sensor<a name="iot-vh400-raspi-setup"></a>

Insert your microSD card into the Raspberry Pi, connect your monitor, keyboard, mouse, and, if you're not using Wi\-Fi, Ethernet cable\. Do not connect the power cable yet\.

Solder headers to the Adafruit ADS1115, and place it on a breadboard.  Connect the VH400 wires to the ADS1115 as follows:
+ Bare Wire: GND
+ Red: VDD \(3\.5 V\)
+ Black: A0

The black wire on the VH400 provides the. voltage that varies with moisture level.  This is fed into the ADS1115 and converted to a digital value for the RPi to read via I2C.

Hold the Raspberry Pi with the Ethernet jack on the right\. In this orientation, there are two rows of GPIO pins at the top\. The pins are are labeled on the ADS1115. Connect the ADS1115 to the bottom row of RPi pins, left to right, as follows:

+ RPi Pin 1, connect to ADS1115 VDD \(power\), 
+ RPi Pin 3, connect to ADS1115 SDA
+ RPi Pin 5, connect to ADS1115 SCL
+ RPi Pin 7, skip this pin
+ RPi Pin 9, connect to ADS1115 GND \(ground\)

For more information, see [Python Computer Wiring](https://learn.adafruit.com/adafruit-stemma-soil-sensor-i2c-capacitive-moisture-sensor/python-circuitpython-test)\.

Attach the power cable to the Raspberry Pi and plug the other end into a wall socket to turn it on\.

**Configure your Raspberry Pi**

1. On **Welcome to Raspberry Pi**, choose **Next**\.

1. Choose your country, language, timezone, and keyboard layout\. Choose **Next**\.

1. Enter a password for your Raspberry Pi, and then choose **Next**\.

1. Choose your Wi\-Fi network, and then choose **Next**\. If you aren't using a Wi\-Fi network, choose **Skip**\.

1. Choose **Next** to check for software updates\. When the updates are complete, choose **Restart** to restart your Raspberry Pi\.

After your Raspberry Pi starts up, enable the I2C interface\.

1. In the upper left corner of the Raspbian desktop, click the Raspberry icon, choose **Preferences**, and then choose **Raspberry Pi Configuration**\.

1. On the **Interfaces** tab, for **I2C**, choose **Enable**\.

1. Choose **OK**\.

The libraries for the Adafruit STEMMA moisture sensor are written for CircuitPython\. To run them on a Raspberry Pi, you need to install the latest version of Python 3\.

1. Run the following commands from a command prompt to update your Raspberry Pi software:

   `sudo apt-get update`

   `sudo apt-get upgrade`

1. Run the following command to update your Python 3 installation:

   `sudo pip3 install --upgrade setuptools`

1. Run the following command to install the Raspberry Pi GPIO libraries:

   `pip3 install RPI.GPIO`

1. Run the following command to install the Adafruit ADS1115 libraries:

   `pip3 install adafruit_ads1x15`

1. Run the following command to install the AWS IoT Device SDK for Python:

   `pip3 install AWSIoTPythonSDK`

Your Raspberry Pi now has all of the required libraries\. Create a file called **moistureSensor\.py** and copy the following Python code into the file:

```
import Adafruit_ADS1x15
from AWSIoTPythonSDK.MQTTLib import AWSIoTMQTTShadowClient

import logging
import time
import json
import argparse

# Shadow JSON schema:
#
# {
#   "state": {
#       "desired":{
#           "moisture":<INT VALUE>,
#           "temp":<INT VALUE>            
#       }
#   }
# }

# Function called when a shadow is updated
def customShadowCallback_Update(payload, responseStatus, token):

    # Display status and data from update request
    if responseStatus == "timeout":
        print("Update request " + token + " time out!")

    if responseStatus == "accepted":
        payloadDict = json.loads(payload)
        print("~~~~~~~~~~~~~~~~~~~~~~~")
        print("Update request with token: " + token + " accepted!")
        print("moisture: " + str(payloadDict["state"]["reported"]["moisture"]))
        print("temperature: " + str(payloadDict["state"]["reported"]["temp"]))
        print("~~~~~~~~~~~~~~~~~~~~~~~\n\n")

    if responseStatus == "rejected":
        print("Update request " + token + " rejected!")

# Function called when a shadow is deleted
def customShadowCallback_Delete(payload, responseStatus, token):

     # Display status and data from delete request
    if responseStatus == "timeout":
        print("Delete request " + token + " time out!")

    if responseStatus == "accepted":
        print("~~~~~~~~~~~~~~~~~~~~~~~")
        print("Delete request with token: " + token + " accepted!")
        print("~~~~~~~~~~~~~~~~~~~~~~~\n\n")

    if responseStatus == "rejected":
        print("Delete request " + token + " rejected!")


# Read in command-line parameters
def parseArgs():

    parser = argparse.ArgumentParser()
    parser.add_argument("-e", "--endpoint", action="store", required=True, dest="host", help="Your device data endpoint")
    parser.add_argument("-r", "--rootCA", action="store", required=True, dest="rootCAPath", help="Root CA file path")
    parser.add_argument("-c", "--cert", action="store", dest="certificatePath", help="Certificate file path")
    parser.add_argument("-k", "--key", action="store", dest="privateKeyPath", help="Private key file path")
    parser.add_argument("-p", "--port", action="store", dest="port", type=int, help="Port number override")
    parser.add_argument("-n", "--thingName", action="store", dest="thingName", default="Bot", help="Targeted thing name")
    parser.add_argument("-id", "--clientId", action="store", dest="clientId", default="basicShadowUpdater", help="Targeted client id")

    args = parser.parse_args()
    return args


# Configure logging
# AWSIoTMQTTShadowClient writes data to the log
def configureLogging():

    logger = logging.getLogger("AWSIoTPythonSDK.core")
    logger.setLevel(logging.DEBUG)
    streamHandler = logging.StreamHandler()
    formatter = logging.Formatter('%(asctime)s - %(name)s - %(levelname)s - %(message)s')
    streamHandler.setFormatter(formatter)
    logger.addHandler(streamHandler)

def moisture_read():
    GAIN = 1
    adc_channel = 0

    value = adc.read_adc(adc_channel, gain=GAIN)
    analog_voltage = value*(4.096/32767)
    print(analog_voltage)
    print(value)    
    
    VWC = 0
    
    # https://vegetronix.com/Products/VH400/VH400-Piecewise-Curve
    if analog_voltage <= 1.1:
        VWC = 10*analog_voltage-1
    elif analog_voltage <= 1.3:
        VWC = 25*analog_voltage-17.5
    elif analog_voltage <= 1.82:
        VWC = 48.08*analog_voltage-47.5
    elif analog_voltage <= 2.2:
        VWC = 26.32*analog_voltage-7.89
    else:
        VWC = 62.5*analog_voltage-87.5
    
    return VWC

# Parse command line arguments
args = parseArgs()

if not args.certificatePath or not args.privateKeyPath:
    parser.error("Missing credentials for authentication.")
    exit(2)

# If no --port argument is passed, default to 8883
if not args.port: 
    args.port = 8883


# Init AWSIoTMQTTShadowClient
myAWSIoTMQTTShadowClient = None
myAWSIoTMQTTShadowClient = AWSIoTMQTTShadowClient(args.clientId)
myAWSIoTMQTTShadowClient.configureEndpoint(args.host, args.port)
myAWSIoTMQTTShadowClient.configureCredentials(args.rootCAPath, args.privateKeyPath, args.certificatePath)

# AWSIoTMQTTShadowClient connection configuration
myAWSIoTMQTTShadowClient.configureAutoReconnectBackoffTime(1, 32, 20)
myAWSIoTMQTTShadowClient.configureConnectDisconnectTimeout(10) # 10 sec
myAWSIoTMQTTShadowClient.configureMQTTOperationTimeout(5) # 5 sec

# Intialize the ADS1115 ADC
adc = Adafruit_ADS1x15.ADS1115()

# Connect to AWS IoT
myAWSIoTMQTTShadowClient.connect()

# Create a device shadow handler, use this to update and delete shadow document
deviceShadowHandler = myAWSIoTMQTTShadowClient.createShadowHandlerWithName(args.thingName, True)

# Delete current shadow JSON doc
deviceShadowHandler.shadowDelete(customShadowCallback_Delete, 5)

# Read data from moisture sensor and update shadow
while True:

    # read moisture level through capacitive touch pad
    moistureLevel = ss.moisture_read()

    # The VH400 does not have temperature, so we set that to 0
    temp = 0

    # Display moisture and temp readings
    print("Moisture Level: {}".format(moistureLevel))
    print("Temperature: {}".format(temp))
    
    # Create message payload
    payload = {"state":{"reported":{"moisture":str(moistureLevel),"temp":str(temp)}}}

    # Update shadow
    deviceShadowHandler.shadowUpdate(json.dumps(payload), customShadowCallback_Update, 5)
    time.sleep(5)
```

# Connecting to AWS IoT <a name="iot-moisture-connect"></a>

Save the file to a place you can find it\. Run `moistureSensor.py` from the command line with the following parameters:

endpoint  
Your custom AWS IoT endpoint in the form identifier\.iot\.region\.amazonaws\.com\. For more information, see [Device Shadow REST API](device-shadow-rest-api.md)\.

rootCA  
The full path to your AWS IoT root CA certificate\.

cert  
The full path to your AWS IoT device certificate\.

key  
The full path to your AWS IoT device certificate private key\.

thingName  
Your thing name \(in this case, `RaspberryPi`\)\.

clientId  
The MQTT client ID\. Use `RaspberryPi`\.

The command line should look like this:

`python3 moistureSensor.py --endpoint your-endpoint --rootCA ~/certs/AmazonRootCA1.pem --cert ~/certs/raspberrypi-certificate.pem.crt --key ~/certs/raspberrypi-private.pem.key --thingName RaspberryPi --clientId RaspberryPi`

Try touching the sensor, putting it in a planter, or putting it in a glass of water to see how the sensor responds to various levels of moisture\. If needed, you can change the threshold value in the `MoistureSensorRule` under AWS IoT->Manage->MEssage ROuting\. When the moisture sensor reading goes below the value specified in your rule's SQL query statement, AWS IoT publishes a message to the Amazon SNS topic\. You should receive an email message that contains the moisture and temperature data\.

After you have verified receipt of email messages from Amazon SNS, press **CTRL\+C** to stop the Python program\. It is unlikely that the Python program will send enough messages to incur charges, but it is a best practice to stop the program when you are done\.
