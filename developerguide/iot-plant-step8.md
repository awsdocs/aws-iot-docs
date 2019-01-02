# Step 8: Finish Preparing the microSDHC card<a name="iot-plant-step8"></a>

In this step, you add several files to the microSDHC card\. These files enable you to connect to the Raspberry Pi from your desktop or laptop computer and enable the Raspberry Pi to communicate with AWS IoT\.

1. If you plan to connect to the Raspberry Pi from your desktop or laptop computer, create a blank file named `ssh` in the root of the microSDHC card\. This file allows you to connect to the Raspberry Pi from an SSH connection tool \(such as PuTTY for Windows, the SSH utility in GitBash for Windows, or the SSH utility for macOS, Linux, or Unix\) after the Raspberry Pi boots\. 

   For example, for Windows, in the command prompt running in Admin mode, run the following command, which creates a blank file named `ssh` in the root of the microSDHC card\. This command assumes the microSDHC card is connected as drive letter D\.

   ```
   fsutil file createnew D:\ssh 0
   ```

1. If you plan to connect to the Raspberry Pi from your desktop or laptop computer, create a blank file named `wpa_supplicant.conf` in the root of the microSDHC card\. This file enables the Raspberry Pi to connect to a wireless network\.

   For example, for Windows, in the same command prompt, run the following command, which creates a blank file named `wpa_supplicant.conf` in the root of the microSDHC card\. This command assumes the microSDHC card is connected as drive letter D\.

   ```
   fsutil file createnew D:\wpa_supplicant.conf 0
   ```

1. If you created the blank `wpa_supplicant.conf` file, open a text editor and add the following content to the `wpa_supplicant.conf` file\. Then save the file\.

   ```
   ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
   network={
       ssid="MyWirelessNetworkName"
       psk="MyWirelessNetworkPassword"
       key_mgmt=WPA-PSK
   }
   ```

   In the preceding content, replace *MyWirelessNetworkName* with the name of the wireless network\. Replace *MyWirelessNetworkPassword* with the password for the wireless network\. For more information, see [ Wireless Connectivity](https://www.raspberrypi.org/documentation/configuration/wireless/) on the Raspberry Pi website\.

1. Create a folder in the root of the microSDHC card named `deviceSDK`\.

1. Copy the files that AWS IoT generated for you earlier ending in `.certificate.pem.ct.txt` \(the root certificate file for your device in AWS IoT\), `private.pem.key` \(the private key for your device in AWS IoT\), and `.pem` \(the root CA for AWS IoT\) into this new `deviceSDK` folder\.

1. Copy the file named `moisture.py` from [Step 5: Simulate Random Moisture Levels](iot-plant-step5.md) into this new `deviceSDK` folder\.

1. Eject the microSDHC card from your desktop or laptop computer\.