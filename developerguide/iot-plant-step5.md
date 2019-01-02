# Step 5: Simulate Random Moisture Levels<a name="iot-plant-step5"></a>

In this step, you use your development computer to simulate soil moisture readings by generating random data\. You then push those readings into the related shadow in AWS IoT\. When the readings get too low, Amazon SNS automatically sends an email alert to the houseplant’s owner\.

1. Make sure that your development computer has [Python](https://www.python.org/downloads/) and [pip](https://pypi.org/project/pip/) installed\.

   pip is included with Python versions 3\.4 and later\. To check your installed version of Python, run the command python \-\-version from the command prompt running in Admin mode for Windows, or from a terminal session running in macOS, Linux, or Unix\. To check whether pip is also installed, run the command pip \-\-version\.

1. Use pip to install the on your development computer\. To do this, run the command pip install AWSIoTPythonSDK\.

1. 3\. Use a text editor to create a new file on your development computer with the following code:

   ```
   from AWSIoTPythonSDK.MQTTLib import AWSIoTMQTTShadowClient
   import random, time
   
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
   
   # Keep generating random test data until this script 
   # stops running.
   # To stop running this script, press Ctrl+C.
   while True:
   
     # Generate random True or False test data to represent
     # okay or low moisture levels, respectively.
     moisture = random.choice([True, False])
     
     if moisture:
       myDeviceShadow.shadowUpdate(
         '{"state":{"reported":{"moisture":"okay"}}}', 
         myShadowUpdateCallback, 5)
     else:
       myDeviceShadow.shadowUpdate(
         '{"state":{"reported":{"moisture":"low"}}}', 
         myShadowUpdateCallback, 5)
     
     # Wait for this test value to be added.
     time.sleep(60)
   ```

   In the preceding code, replace the following values:

   + *a1b23cde4fghij\-ats\.iot\.us\-east\-1\.amazonaws\.com* with the REST API endpoint that AWS IoT generated for you\. To get this endpoint, in the [ AWS IoT console](https://console.aws.amazon.com/iot/home) navigation pane, expand **Manage**, choose **Things**, and then choose your thing’s name \(for example, **MyRPi**\)\. Choose **Interact**, and then look in the **HTTPS** area\.  
![\[MyRPi thing's Interact page,
                    with the HTTPS Rest API Endpoint shown.\]](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/images/console-https-endpoint.png)

   + *AmazonRootCA1\.pem* with the name of the root CA for AWS IoT, which you saved earlier to your development computer\.

   + *12ab3cd456\-private\.pem\.key* with the name of the private key for your device in AWS IoT, which you saved earlier to your development computer\.

   + *12ab3cd456\-certificate\.pem\.crt\.txt* with the name of the root certificate file for your device in AWS IoT, which you saved earlier to your development computer\.

   + The *60* in `time.sleep(60)` with the number of seconds to wait for each new random simulated reading to be generated\. The lower the value that you use for this number, the more frequently you might get email alerts\.

1. Save the file with the extension `.py`, for example, `moisture.py`, in the same directory where you saved your root CA for AWS IoT, the private key file for your device in AWS IoT, and the root certificate file for your device in AWS IoT\. If you choose to use a different name for the `.py` file, be sure to substitute it throughout this sample\.

1. From the command prompt running in Admin mode for Windows, or from a terminal session in macOS, Linux, or Unix, switch to the directory that contains the `moisture.py` file\. Then run the command python moisture\.py for Python to start running the `moisture.py` script\. 

   Every *60* seconds, the script generates a random `True` or `False` value\. If the value is `True`, Python sends a "moisture okay" reading to AWS IoT\. If the value is `False`, Python sends a low moisture reading to AWS IoT\. Whenever AWS IoT receives a low moisture reading, it triggers a rule that sends an alert to your email address through Amazon SNS\.

1. 6\. When you are done, press **Ctrl\+C** to stop running the script\.

If you don’t have a Raspberry Pi, you have reached the end of this sample walkthrough, and you can skip ahead to [](iot-plant-cleanup.md)\. Otherwise, continue to [Module 2: Sending Data with the Raspberry Pi](iot-plant-module2.md) to begin preparing your Raspberry Pi\.