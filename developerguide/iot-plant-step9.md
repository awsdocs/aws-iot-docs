# Step 9: Connect to the Raspberry Pi and Set Up Raspbian<a name="iot-plant-step9"></a>

In this step, you start the Raspberry Pi\. You connect to it directly or from your desktop or laptop computer\. If you connect directly to the Raspberry Pi, you then set up Raspbian on it\.

1. Insert the microSDHC card into the Raspberry Pi\. The card slot is on the underside of the motherboard\. The card goes in the slot only one way, typically with the writing on the card facing down\.  
![\[Insert the microSDHC card, with the writing on the card facing down, into the card slot.\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/rpi-insert-microsdhc.png)

1. If you want to access the Raspberry Pi directly, instead of from your development computer, connect a separate keyboard, mouse, and monitor directly to the Raspberry Pi, for example, by using the USB and HDMI ports\. Although the Raspberry Pi 3 provides Bluetooth connectivity, you won’t be able to connect via Bluetooth until you boot the Raspberry Pi for the first time\.  
![\[Mouse, keyboard, and monitor connected to the corresponding ports on the Raspberry Pi.\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/rpi-usb-monitor.png)

1. Insert the prongs of the micro USB adapter power supply into your power source, and then plug the micro USB end into its slot in the Raspberry Pi\.  
![\[Power supply for Raspberry Pi connected to the micro USB port.\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/rpi-insert-power.png)

   The Raspberry Pi red power LED light turns on, the green activity LED light begins flickering, and the Raspbian operating system automatically boots\. If you plan to access the Raspberry Pi from your development computer, the Raspberry Pi attempts to connect to the wireless network using the password that you specified earlier in the `wpa_supplicant.conf` file\.

1. If you don’t want to access the Raspberry Pi from your development computer, skip ahead to step 5 in this procedure\.

   To access the Raspberry Pi from your development computer, get the IP address of the Raspberry Pi that is now connected to the wireless network\. For example, if you can log in to your wireless network router as an administrator, you should be able to look up the IP address from there\. Otherwise, you could use a utility such as **ping** or an application such as [Nmap for Windows](https://nmap.org/)\.

   After you get the Raspberry Pi’s IP address, connect from your Windows desktop or laptop computer to the Raspberry Pi using an SSH connection tool such as [PuTTY](https://www.putty.org/), or the SSH utility in [Git Bash for Windows](https://git-scm.com/download/win)\.

   If you use PuTTY, for Host Name \(or IP address\), use the format *pi*@*X\.X\.X\.X*\. For SSH, use `ssh` *pi*@*X\.X\.X\.X*\. In either case, *pi* is the default user name, and *X\.X\.X\.X* is the Raspberry Pi’s IP address\. When prompted for a password, use the default password, *raspberry*\.

   Skip ahead to [Step 10: Set Up the Soil Moisture Sensor Kit](iot-plant-step10.md)\.

1. To access the Raspberry Pi directly instead of from your development computer, turn on your monitor to the correct input source \(for example, HDMI input\)\.

   A dialog box might appear, notifying you that SSH is enabled on the Raspberry Pi and the default password for the user pi has not been changed\. You can close this dialog box now, because you get an opportunity to change this password later in this step\.

1. The first time you start the Raspberry Pi, a **Welcome to Raspberry Pi** dialog box is displayed\. Choose **Next**\.

1. On the **Set Country** page, choose the **Country**, **Language**, and **Timezone** you want\. If you have a US keyboard, select the **US keyboard** box\.

1. Choose **Next**\.

1. On the **Change Password** page, enter a new password for the default user pi\.
**Note**  
You can complete this procedure whether or not you set a new password, but setting a new password helps keep your device secure\.

1. Choose **Next**\.

1. On the **Select WiFi Network** page, choose the wireless network to connect to, and then choose **Next**\.

1. On the **Update Software** page, choose **Next**\. When the update is complete, choose **OK**\.

1. On the **Setup Complete** page, choose **Reboot**, and wait while the Raspberry Pi restarts\.