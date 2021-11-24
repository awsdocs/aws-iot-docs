# Create FUOTA task and provide firmware image<a name="connect-iot-lorawan-fuota-create-task"></a>

To update the firmware of your LoRaWAN devices, first create a FUOTA task and provide the digitally signed firmware image you want to use for the update\. You can then add your devices and multicast groups to the task and schedule a FUOTA session\. When the session starts, AWS IoT Core for LoRaWAN sets up a fragmentation session and your end devices collect the fragments, reconstruct the image, and apply the new firmware\. For information about the FUOTA process, see [FUOTA process overview](connect-iot-lorawan-fuota-mc-process.md)\.

The following shows how you can create a FUOTA task and upload the firmware image or delta image that you'll store in an S3 bucket\.

## Prerequisites<a name="connect-iot-lorawan-fuota-task-prereq"></a>

Before you can perform FUOTA updates, the firmware image must be digitally signed so that your end devices can verify the authenticity of the image when applying the image\. You can use any third\-party tool to generate the digital signature for your firmware image\. We recommend that you use a digital signature tool such as the one embedded in the [ARM Mbed GitHub repository](https://github.com/armmbed/mbed-os-example-lorawan-fuota), which also includes tools for generating the delta image and for devices to use that image\.

## Create FUOTA task and upload firmware image by using the console<a name="connect-iot-lorawan-fuota-task-console"></a>

To create a FUOTA task and upload your firmware image by using the console, go to the [FUOTA tasks](https://console.aws.amazon.com/iot/home#/wireless/fuotaTasks) tab of the console and then choose **Create FUOTA task**\.

1. 

**Create FUOTA task**

   To create your FUOTA task, specify the task properties and tags\.

   1. 

**Specify FUOTA task properties**  
To specify FUOTA task properties, enter the following information for your FUOTA task\.
      + **Name**: Enter a unique name for your FUOTA task\. The name must contain only letters, numbers, hyphens, and underscores\. It can't contain spaces\.
      + **Description**: You can provide an optional description for your multicast group\. The description field can be up to 2,048 characters\.
      + **RFRegion**: Set the frequency band for your FUOTA task\. The frequency band must match the one you used to provision your wireless devices or multicast groups\.

   1. 

**Tags for FUOTA task**  
You can optionally provide any key\-value pairs as **Tags** for your FUOTA task\. To continue creating your task, choose **Next**\.

1. 

**Upload firmware image**

   Choose the firmware image file that you want to use to update the firmware of the devices you add to the FUOTA task\. The firmware image file is stored in an S3 bucket\. You can provide AWS IoT Core for LoRaWAN the permissions to access the firmware image on your behalf\. We recommend that you digitally sign the firmware images so that its authenticity is verified when the firmware update is performed\.

   1. 

**Choose firmware image file**  
You can either upload a new firmware image file to an S3 bucket or choose an existing image that has already been uploaded to an S3 bucket\.
**Note**  
The firmware image file must not be larger than 1 megabyte in size\. The larger your firmware size, the longer it can take for your update process to complete\.
      + To use an existing image, choose **Select an existing firmware image**, choose **Browse S3**, and then choose the firmware image file you want to use\.

        AWS IoT Core for LoRaWAN populates the S3 URL, which is the path to your firmware image file in the S3 bucket\. The format of the path is `s3://bucket_name/file_name`\. To view the file in the [Amazon Simple Storage Service](https://console.aws.amazon.com/s3/) console, choose **View**\.
      + To upload a new firmware image\.

        1. Choose **Upload a new firmware image** and upload your firmware image\. The image file must not be larger than 1 megabyte\. 

        1. To create an S3 bucket and enter a **Bucket name** for storing your firmware image file, choose **Create S3 bucket**\.

   1. 

**Permissions to access the bucket**  
You can either create a new service role or choose an existing role to allow AWS IoT Core for LoRaWAN to access the firmware image file in the S3 bucket on your behalf\. Choose **Next**\.

      To create a new role, you can enter a role name or leave it blank for a random name to be generated automatically\. To view the policy permissions that grant access to the S3 bucket, choose **View policy permissions**\.

   For more information about using an S3 bucket to store your image and granting AWS IoT Core for LoRaWAN permissions to access it, see [Upload the firmware file to an S3 bucket and add an IAM role](connect-iot-lorawan-upload-firmware-s3bucket.md)\. 

1. 

**Review and create**

   To create your FUOTA task, review the FUOTA task and configuration details that you specified and the choose **Create task**\.

## Create FUOTA task and upload firmware image by using the API<a name="connect-iot-lorawan-fuota-task-api"></a>

To create a FUOTA task and specify your firmware image file by using the API, use the [https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_CreateFuotaTask.html](https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_CreateFuotaTask.html) API operation or the [https://docs.aws.amazon.com/cli/latest/reference/iotwireless/create-fuota-task.html](https://docs.aws.amazon.com/cli/latest/reference/iotwireless/create-fuota-task.html) CLI command\. You can provide an `input.json` file as input to the `create-fuota-task` command\. When you use the API or CLI, the firmware image file that you provide as input must be already uploaded to an S3 bucket\. You also specify the IAM role that gives AWS IoT Core for LoRaWAN access to the firmware image in the S3 bucket\.

```
aws iotwireless create-fuota-task \
    --cli-input-json file://input.json
```

where:

**Contents of input\.json**

```
{   
   "Description": "FUOTA task to update firmware of devices in multicast group.",
   "FirmwareUpdateImage": "S3:/firmware_bucket/firmware_image
   "FirmwareUpdateRole": "arn:aws:iam::123456789012:role/service-role/ACF1zBEI"
   "LoRaWAN": { 
      "RfRegion": "US915"
   },
   "Name": "FUOTA_Task_MC"  
}
```

After you create your FUOTA task, you can use the following API operations or CLI commands to update, delete, or get information about your FUOTA task\.
+ [https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_UpdateFuotaTask](https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_UpdateFuotaTask) or [https://docs.aws.amazon.com/cli/latest/reference/iotwireless/update-fuota-task.html](https://docs.aws.amazon.com/cli/latest/reference/iotwireless/update-fuota-task.html) 
+ [https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_GetFuotaTask](https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_GetFuotaTask) or [https://docs.aws.amazon.com/cli/latest/reference/iotwireless/get-fuota-task.html](https://docs.aws.amazon.com/cli/latest/reference/iotwireless/get-fuota-task.html) 
+ [https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_ListFuotaTasks](https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_ListFuotaTasks) or [https://docs.aws.amazon.com/cli/latest/reference/iotwireless/list-fuota-tasks.html](https://docs.aws.amazon.com/cli/latest/reference/iotwireless/list-fuota-tasks.html) 
+ [https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_DeleteFuotaTask](https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_DeleteFuotaTask) or [https://docs.aws.amazon.com/cli/latest/reference/iotwireless/delete-fuota-task.html](https://docs.aws.amazon.com/cli/latest/reference/iotwireless/delete-fuota-task.html) 

## Next steps<a name="connect-iot-lorawan-fuota-next-steps"></a>

Now that you've created a FUOTA task and provided the firmware image, you can add devices to the task for updating their firmware\. You can either add individual devices or multicast groups to the task\. For more information, see [Add devices and multicast groups to a FUOTA task and schedule a FUOTA session](connect-iot-lorawan-fuota-add-devices.md)\.