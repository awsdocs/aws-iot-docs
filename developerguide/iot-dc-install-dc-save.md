# \(Optional\) Save the microSD card image<a name="iot-dc-install-dc-save"></a>

At this point, your Raspberry Pi's microSD card has an updated OS, the basic application software, and the AWS IoT Device Client\. 

If you want to come back to try these exercises and tutorials again, you can skip the preceding procedures by writing the microSD card image that you save with this procedure to a new microSD card and continue the tutorials from [Step 2: Provision your Raspberry Pi in AWS IoT](iot-dc-install-provision.md)\.

**To save the microSD card image to a file:**

In the terminal window on your local host computer that's connected to your Raspberry Pi:

1. Confirm that your AWS account credentials have not been stored\.

   1. Run the AWS configure app with this command:

      ```
      aws configure
      ```

   1. If your credentials have been stored \(if they are displayed in the prompt\), then enter the **XYXYXYXYX** string when prompted as shown here\. Leave **Default region name** and **Default output format** blank\.

      ```
      AWS Access Key ID [****************YXYX]: XYXYXYXYX
      AWS Secret Access Key [****************YXYX]: XYXYXYXYX
      Default region name: 
      Default output format:
      ```

1. Enter this command to shutdown the Raspberry Pi\.

   ```
   sudo shutdown -h 0
   ```

1. After the Raspberry Pi shuts down completely, remove its power connector\.

1. Remove the microSD card from your device\.

1. On your local host computer: 

   1. Insert the microSD card\.

   1. Using your SD card imaging tool, save the microSD card’s image to a file\.

   1. After the microSD card’s image has been saved, eject the card from the local host computer\.

You can continue with this microSD card in [Step 2: Provision your Raspberry Pi in AWS IoT](iot-dc-install-provision.md)\.