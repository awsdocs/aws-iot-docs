# Setting up your Raspberry Pi and moisture sensor<a name="iot-moisture-raspi-setup"></a>

Insert your micro SD card into the Raspberry Pi, connect your monitor, keyboard, mouse, and, if you're not using Wi\-Fi, Ethernet cable\. Do not connect the power cable yet\.

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
    parser.add_argument("-e", "--endpoint", action="store", required=True, dest="host", help="Your AWS IoT custom endpoint")
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
```

Save the file to a place you can find it\. Run `moistureSensor.py` from the command line with the following parameters:

endpoint  
Your custom AWS IoT endpoint\. For more information, see [Device Shadow REST API](device-shadow-rest-api.md)\.

rootCA  
The full path the your AWS IoT root CA certificate\.

cert  
The full path to your AWS IoT device certificate\.

key  
The full path to your AWS IoT device certificate private key\.

thingName  
Your thing name \(in this case, `RaspberryPi`\)\.

clientId  
The MQTT client ID\. Use `RaspberryPi`\.

The command line should look like this:

`python3 moistureSensor.py --endpoint <your-endpoint> --rootCA ~/certs/AmazonRootCA1.pem --cert ~/certs/raspberrypi-certificate.pem.crt --key ~/certs/raspberrypi-private.pem.key --thingName RaspberryPi --clientId RaspberryPi`

Try touching the sensor, putting it in a planter, or putting it in a glass of water to see how the sensor responds to various levels of moisture\. If needed, you can change the threshold value in the `MoistureSensorRule`\. When the moisture sensor reading goes below the value specified in your rule's SQL query statement, AWS IoT publishes a message to the Amazon SNS topic\. You should receive an email message that contains the moisture and temperature data\.

After you have verified receipt of email messages from Amazon SNS, press **CTRL\+C** to stop the Python program\. It is unlikely the Python program will send enough messages to incur charges, but it is a best practice to stop the program when you are done\.