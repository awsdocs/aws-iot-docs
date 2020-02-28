# Getting Started with Alexa Voice Service Integration for AWS IoT on an NXP Device<a name="avs-integration-aws-iot-gs-nxp"></a>

 The NXP i\.MX 106A development kit enables you to preview Alexa Voice Service \(AVS\) Integration for AWS IoT using a preconfigured NXP account\. After you preview the functionality with the NXP account, you need to customize the firmware, application source code, and the NXP mobile application provided with the kit to use your own account\. This topic walks you through the steps to preview with the preconfigured account and to customize your device with your own account\. 

**Topics**
+ [Preview AVS Integration for AWS IoT with a Preconfigured NXP Account](#iot-mqtt-alexa-gs-nxp-preview)
+ [Use Your AWS and Alexa Voice Service Developer Accounts to Set Up AVS for AWS IoT](#iot-mqtt-alexa-gs-nxp-configure)

## Preview AVS Integration for AWS IoT with a Preconfigured NXP Account<a name="iot-mqtt-alexa-gs-nxp-preview"></a>

### Prerequisites<a name="iot-mqtt-alexa-gs-nxp-preview-prereqs"></a>

To follow these steps, you need the following resources\.
+ [NXP i\.MX 106A development kit](https://www.nxp.com/design/designs/mcu-based-solution-for-br-alexa-voice-service:MCU-VOICE-CONTROL-AVS)
+ A Mac, Windows 7/10, or Linux computer
+ A serial driver that works with your computer
+ An Android or iOS mobile device
+ Android Debug Bridge \(ADB\)
+ An [Amazon Alexa account](https://alexa.amazon.com)

### Install and Boot the Development Kit<a name="iot-mqtt-alexa-gs-nxp-preview-devkit"></a>

1. Activate the device kit\. The kit comes with a Get Started Onboarding card\. This card contains instructions to activate the kit and acquire the necessary software package and the reference design files\. The software package contains the SDK source code and the companion Android application \(`VoiceCompanionApp.apk`\)\. If you're using an iOS device, you must request access to the TestFlight iOS application from your local NXP representative\.

   After you download and extract the software package, you see the following file structure\.  
![\[The software package contains folders named Amazon, DefaultBinaries, Drivers, MobileApplication, SDK, and Tools. It also contains the developer guide, the migration guide, and the user guide.\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/iot-nxp-software-package.png)

    The `MobileApplication` folder contains the Android mobile application\.

   The `Tools` folder contains scripts that you use to configure your device\.

   You can also find the following documentation:
   + `SLN-ALEXA-IOT-DG.pdf` in the *NXP Developer Guide*
   + `SLN-ALEXA-IOT-MG.pdf` in the *NXP Migration Guide*
   + `SLN-ALEXA-IOT-UG.pdf` in the *NXP User Guide*

1. Boot the device kit\. The USB splitter cable that comes with the kit has power and data connections\. 

   1. Connect both of the USB\-A connections to your computer\. 

   1. Connect the USB\-C connector to your kit\. Your configuration might look the following image\.  
![\[Plug the two USB-A connections into your computer. Plug the single USB-A connection to your device kit.\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/iot-nxp-device-kit.png)

   When the board has power, the D1 LED lights up and displays green\. The D2 LED \(closest to the speaker\) indicates the state of the device\. When the device turns on, this LED flashes multiple colors\. When the device runs the application code, the D2 LED blinks yellow\. When initialization of the device is complete, the D2 LED displays orange to indicate that it's waiting for Wi\-Fi credentials to be added to the device\.

   The NXP User Guide contains a description of the device's physical controls\.  
![\[The D2 LED displays orange to indicate that it's waiting for Wi-Fi credentials.\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/iot-nxp-device-kit-powered.png)

1. Use the terminal application to understand how the client application on the device works\. You can also use it to debug and make sure that the device is in the correct state\. 

   Enter the following commands to get started with the application:
   + `help` – Displays a list of the available commands\.
   + `enable_usb_log` – Enables logging on the device\. This helps you understand what the application is doing\.
   + `logs` – Displays the last few lines of the logs\. At this point, the logs should indicate that the device is waiting for Wi\-Fi credentials\.

### Connect the Device to AWS IoT and Amazon Alexa<a name="iot-mqtt-alexa-gs-nxp-preview-connect"></a>

1. Install the appropriate mobile application\. If you're using an Android device, you can use ADB in the terminal to install the `VoiceCompanionApp.apk` file\. If you're using an iOS device, use the TestFlight application\.

1. Provision the Wi\-Fi credentials\. You can provision the Wi\-Fi credentials by using either the companion mobile application or the terminal\.

   1. Follow these steps to provision the Wi\-Fi credentials by using the mobile application\.

      1. When the device boots up from its factory new state, it creates a Wi\-Fi access point\. Open the companion mobile application, and choose **WIFI PROVISION** to connect to this access point\.

      1. Choose a network from the list\. 

      1. Enter your password, and choose **Send** to transmit the credentials to the device kit\.

   1. In the terminal application, enter the following command\.

      ```
      setup YourWiFiNetworkName YourWiFiNetworkPassword
      ```

   1. Choose **Enter**\.

1. Authenticate the device kit and connect to AWS IoT and Amazon Alexa\. Make sure that your mobile device and the device kit are using the same Wi\-Fi network so that the two devices can communicate\. 

   To do so, press the **Discover** button in the mobile application\. When discovery is complete, you see a list of serial numbers for the available device kits near you\. If more than one is available, you can confirm your kit's unique serial number by entering **serial\_number** in the terminal application\.

1. Choose the device kit that to use\. The mobile application uses the Login with Amazon service to prompt you for your Amazon user credentials\. The device kit begins to register the device with AVS\. The D2 LED displays purple to indicate that device registration has started\.
**Note**  
If your mobile device already has the Amazon shopping application installed, the mobile companion application automatically uses the Amazon account that is logged into the shopping application\.

After you sign in to your Amazon account, the companion application performs the following steps:

1. The device kit receives the access token from the Login with Amazon service and begins to connect the device to AWS IoT and AVS\. The mobile application displays a completion percentage\. The D2 LED on the device displays orange\.

1. The D1 LED blinks green every 500 milliseconds until the device connects to AWS IoT\.

1. When the device connects to AWS IoT, the device kit begins to connect the device to AVS\. The D1 LED blinks green every 250 milliseconds\.

1. When the device connects to AVS, the color of the device kit's serial number changes to yellow in the mobile application\. The mobile application displays a **Complete** message\. The D1 LED turns off, and the device plays a chime\. The device is now connected to NXP's AWS IoT account\.

You can try a few Alexa Voice commands, such as the following examples:
+ "Alexa, what's the weather like?"
+ "Alexa, play a news briefing\."
+ "Alexa, play music\."

## Use Your AWS and Alexa Voice Service Developer Accounts to Set Up AVS for AWS IoT<a name="iot-mqtt-alexa-gs-nxp-configure"></a>

### Prerequisites<a name="iot-mqtt-alexa-gs-nxp-configure-prereqs"></a>

For this procedure, you need the following resources\.
+ [NXP i\.MX 106A development kit](https://www.nxp.com/design/designs/mcu-based-solution-for-br-alexa-voice-service:MCU-VOICE-CONTROL-AVS)
+ MCUXPresso v10\.3\.1
+ Segger J\-Link debug probe
+ A Mac computer
+ An Android mobile device
+ Android Studio
+ Android Debug Bridge
+ An [Amazon Alexa account](https://alexa.amazon.com)
+ An [AWS account](http://aws.amazon.com)

### Configure Your AWS IoT Account and Provision Your Device<a name="iot-mqtt-alexa-gs-nxp-configure-account"></a>

1. Generate credentials to authenticate with AWS IoT\. All devices must have a device certificate, private key, and root CA certificate installed in their firmware to communicate with AWS IoT\. Follow the instructions in [Register a Device in the Registry](register-device.html) to register your device with AWS IoT\. For more information about X\.509 certificates, see [X\.509 Client Certificates](https://docs.aws.amazon.com/iot/latest/developerguide/x509-client-certs.html)\.

1. Navigate to the root folder and open the NXP Developer Guide\. Follow the instructions in "Section 9 \(Filesystem\)" to convert your client certificates and keys to a binary format that the NXP filesystem can read\. 

   Use the following script and commands to convert the certificates and keys\. The `bin_dump.py` script is in the `Tools\Ivaldi.zip\Scripts\sln_alexa_iot_utils` folder\.

   ```
   python3 bin_dump.py CertificateName-certificate.pem.crt CertificateName-certificate.pem.crt.bin
   ```

   ```
   python3 bin_dump.py CertificateName-private.pem.key CertificateName-private.pem.key.bin
   ```

For information about options for generating credentials for production devices at scale, see [Device Provisioning](https://docs.aws.amazon.com/iot/latest/developerguide/iot-provision.html)\.

**To set up your device in the Alexa Voice Service Developer console**

1. Navigate to the root folder and open the NXP Migration Guide\.

1. To get the MD5 and SHA256 signatures that you need to create an AVS API key, follow the instructions in "Section 2\. Creating a Keystore for Android AVS Companion Application" in the *NXP Migration Guide*\. \(This includes navigating to the [Alexa Voice Service Developer Console](https://developer.amazon.com/alexa/console/avs/home?)\.\)

1. To create a [Login with Amazon](https://developer.amazon.com/apps-and-games/login-with-amazon) security profile and create an AVS product, follow the instructions in "Section 3" in the *NXP Migration Guide*\.

**To rebuild the Android mobile app**

1. Open the VoiceCompanionApp project in Android Studio\. The `VoiceCompanionApp.apk` file is included in the NXP software package\.

1. In Android Studio, add the API key that you generated in the previous procedure to the `api_key.txt` file in the **assets** folder\. This synchronizes the companion mobile application with your AVS security profile\. The application uses the API key when you log in by using the Login with Amazon service\. When the application gets an authorization code from Amazon, it transfers the code to the device firmware\. The device then obtains access and refresh tokens from the Login with Amazon service to make calls to AVS\.

1. In Android Studio, choose **Build Project** and **Generate a Signed Bundle/APK**\. For more information, see "Section 4\. Building and Generating the Mobile Application" in the *NXP Migration Guide*\. For more information about authorizing with Alexa in this way, see [Authorize from a Companion App \(Android/iOS\)](https://developer.amazon.com/docs/alexa-voice-service/authorize-companion-app.html)\.

1. Install the updated mobile application to your mobile device\. Set up ADB on your Mac computer and install the updated Android APK file to your mobile phone by using the terminal console\. If you installed the mobile application to preview the AVS for AWS IoT service with the preconfigured NXP account, you must uninstall and then reinstall the application\.

**To update the client application with your AWS credentials**

1. Follow the instructions in Section 3 and Section 4 in the *NXP Developer Guide* to make the required Segger J\-Link driver modifications\. These instructions also explain how to import the `Bootloader`, `ais_demo`, and `bootstrap` projects by using the MCUXPresso IDE\.

1. Follow the instructions for updating the source code and rebuilding the application in "Section 7\. RT106A Firmware Changes" in the *NXP Migration Guide*\. We recommend that you use MCUXPresso v10\.3\.1\.

   These source code changes enable the device firmware to communicate with your AWS account and your AVS product instead of the default NXP account\.

**To set up and connect your device to AWS IoT and Amazon Alexa**

1. Reset the device to factory defaults\. If you previewed the AVS for AWS IoT service with the preconfigured NXP account, you must reset the device firmware before you log in again with the companion mobile application\. This ensures that the mobile application and the device use your security profile\. Make sure that the device is connected to your computer\. Push **SW1** for 10 seconds to initiate the factory reset\.

   The *NXP User Guide* that is included in the NXP software package contains a description of the device's physical controls\.

1. Reprogram the firmware to use the certificate and key that you generated for your device\. Add the key and certificate to the updated `Bootstrap` and `Ais_demo` binaries that you created in the previous procedure\. We also recommend reprogramming the firmware with the default `Intelligent toolbox`, `app_crt` and `CA_crt` binaries\.

   For instructions, see "Section 5\. Building and Programming" in the *NXP Developer Guide*\.

1. Follow the instructions in the previous [Connect the Device to AWS and Amazon Alexa](#iot-mqtt-alexa-gs-nxp-preview-connect) procedure\. This connects you to AVS for AWS IoT with your own security profile and credentials\.