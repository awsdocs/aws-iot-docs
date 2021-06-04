# Update gateway firmware using CUPS service with AWS IoT Core for LoRaWAN<a name="connect-iot-lorawan-update-firmware"></a>

The [LoRa Basics Station](https://doc.sm.tc/station) software that runs on your gateway provides credential management and firmware update interface using the Configuration and Update Server \(CUPS\) protocol\. The CUPS protocol provides secure firmware update delivery with ECDSA signatures\.

You'll have to frequently update your gateway's firmware\. You can use the CUPS service with AWS IoT Core for LoRaWAN to provide firmware updates to the gateway where the updates can also be signed\. To update the gateway's firmware, you can use the SDK or CLI but not the console\. 

The update process takes about 45 minutes to complete\. It can take longer if you're setting up your gateway for the first time to connect to AWS IoT Core for LoRaWAN\. Gateway manufacturers usually provide their own firmware update files and signatures so you can use that instead and proceed to [Upload the firmware file to an S3 bucket and add an IAM role](connect-iot-lorawan-upload-firmware-s3bucket.md)\.

If you don't have the firmware update files, see [Generate the firmware update file and signature](connect-iot-lorawan-script-fwupdate-sigkey.md) for an example that you can use to adapt to your application\.

**Topics**
+ [Generate the firmware update file and signature](connect-iot-lorawan-script-fwupdate-sigkey.md)
+ [Upload the firmware file to an S3 bucket and add an IAM role](connect-iot-lorawan-upload-firmware-s3bucket.md)
+ [Schedule and run the firmware update by using a task definition](connect-iot-lorawan-schedule-firmware-update.md)