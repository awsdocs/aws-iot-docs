# Schedule and run the firmware update by using a task definition<a name="connect-iot-lorawan-schedule-firmware-update"></a>

You can use a task definition to include details about the firmware update and define the update\. AWS IoT Core for LoRaWAN provides a firmware update based on information from the following three fields associated with the gateway\.
+ 

**Station**  
The version and build time of the Basics Station software\. To identify this information, you can also generate it by using the Basics Station software that is being run by your gateway \(for example, `2.0.5(rpi/std) 2021-03-09 03:45:09`\)\.
+ 

**PackageVersion**  
The firmware version, specified by the file `version.txt` in the gateway\. While this information might not be present in the gateway, we recommend it as a way to define your firmware version \(for example, `1.0.0`\)\.
+ 

**Model**  
The platform or model that is being used by the gateway \(for example, Linux\)\.

This procedure takes 20 minutes to complete\.

**Topics**
+ [Get the current version running on your gateway](#connect-iot-lorawan-gateway-current-version)
+ [Create a wireless gateway task definition](#connect-iot-lorawan-create-task-definition)
+ [Run the firmware update task and track progress](#connect-iot-lorawan-run-fwupdate-task)

## Get the current version running on your gateway<a name="connect-iot-lorawan-gateway-current-version"></a>

To determine your gateway's eligibility for a firmware update, the CUPS server checks all three fields, `Station`, `PackageVersion`, and `Model`, for a match when the gateway presents them during a CUPS request\. When you use a task definition, these fields are stored as part of the `CurrentVersion` field\.

You can use the AWS IoT Core for LoRaWAN API or AWS CLI to get the `CurrentVersion` for your gateway\. Following commands show how to get this information using the CLI\.

1. If you've already provisioned a gateway, you can get information about the gateway using the [get\-wireless\-gateway](https://docs.aws.amazon.com/cli/latest/reference/iotwireless/get-wireless-gateway.html) command\.

   ```
   aws iotwireless get-wireless-gateway \ 
       --identifier 5a11b0a85a11b0a8 \ 
           --identifier-type GatewayEui
   ```

   Following shows a sample output for the command\.

   ```
   {
       "Name": "Raspberry pi",
       "Id": "1352172b-0602-4b40-896f-54da9ed16b57",
       "Description": "Raspberry pi",
       "LoRaWAN": {
           "GatewayEui": "5a11b0a85a11b0a8",
           "RfRegion": "US915"
       },
       "Arn": "arn:aws:iotwireless:us-east-1:231894231068:WirelessGateway/1352172b-0602-4b40-896f-54da9ed16b57"
   }
   ```

1. Using the wireless gateway ID reported by the `get-wireless-gateway` command, you can use the [get\-wireless\-gateway\-firmware\-information](https://docs.aws.amazon.com/cli/latest/reference/iotwireless/get-wireless-gateway-firmware-information.html) command to get the `CurrentVersion`\. 

   ```
   aws iotwireless get-wireless-gateway-firmware-information \
       --id "3039b406-5cc9-4307-925b-9948c63da25b"
   ```

   Following shows a sample output for the command, with information from all three fields displayed by the `CurrentVersion`\.

   ```
   {
       "LoRaWAN": {
           "CurrentVersion": {
               "PackageVersion": "1.0.0",
               "Model": "rpi",
               "Station": "2.0.5(rpi/std) 2021-03-09 03:45:09"
           }
       }
   }
   ```

## Create a wireless gateway task definition<a name="connect-iot-lorawan-create-task-definition"></a>

When you create the task definition, we recommend that you specify automatic creation of tasks by using the `AutoCreateTasks` parameter\. `AutoCreateTasks` applies to any gateway that has a match for all three parameters mentioned previously\. If this parameter is disabled, the parameters have to be manually assigned to the gateway\.

You can create the wireless gateway task definition by using the AWS IoT Core for LoRaWAN API or AWS CLI\. Following commands show how to create the task definition using the CLI\.

1. Create a file, `input.json`, that'll contain the information to pass to the `CreateWirelessGatewayTaskDefinition` API\. In the `input.json` file, provide the following information that you obtained earlier:
   + 

**UpdateDataSource**  
Provide the link to your object containing the firmware update file that you uploaded to the S3 bucket\. \(for example, `s3://iotwirelessfwupdate/fwstation`\.
   + 

**UpdateDataRole**  
Provide the link to the Role ARN for the IAM role that you created, which provides permissions to read the S3 bucket\. \(for example, `arn:aws:iam::123456789012:role/IoTWirelessFwUpdateRole`\.
   + 

**SigKeyCRC and UpdateSignature**  
This information might be provided by your gateway manufacturer, but if you followed the procedure described in [Generate the firmware update file and signature](connect-iot-lorawan-script-fwupdate-sigkey.md), you'll find this information when generating the signature\.
   + 

**CurrentVersion**  
Provide the `CurrentVersion` output that you obtained previously by running the `get-wireless-gateway-firmware-information ` command\.

     ```
     cat input.json
     ```

     Following shows the contents of the `input.json` file\.

     ```
     {
         "AutoCreateTasks": true,
         "Name": "FirmwareUpdate",
         "Update":
         {
             "UpdateDataSource" : "s3://iotwirelessfwupdate/fwstation",
             "UpdateDataRole" : "arn:aws:iam::123456789012:role/IoTWirelessFwUpdateRole",
             "LoRaWAN" :
             {
                 "SigKeyCrc": 3434210794,
                 "UpdateSignature": "MEQCIDPY/p2ssgXIPNCOgZr+NzeTLpX+WfBo5tYWbh5pQWN3AiBROen+XlIdMScvAsfVfU/ZScJCalkVNZh4esyS8mNIgA==",
                 "CurrentVersion" :
                 {
                 "PackageVersion": "1.0.0",
                 "Model": "rpi",
                 "Station": "2.0.5(rpi/std) 2021-03-09 03:45:09"
                 }
             }
         }
     }
     ```

1. Pass the `input.json` file to the [create\-wireless\-gateway\-task\-definition](https://docs.aws.amazon.com/cli/latest/reference/iotwireless/get-wireless-gateway-task-definition.html) command to create the task definition\. 

   ```
   aws iotwireless create-wireless-gateway-task-definition \ 
       --cli-input-json file://input.json
   ```

   Following shows the output of the command\.

   ```
   {
       "Id": "4ac46ff4-efc5-44fd-9def-e8517077bb12",
       "Arn": "arn:aws:iotwireless:us-east-1:231894231068:WirelessGatewayTaskDefinition/4ac46ff4-efc5-44fd-9def-e8517077bb12"
   }
   ```

## Run the firmware update task and track progress<a name="connect-iot-lorawan-run-fwupdate-task"></a>

The gateway is ready to receive the firmware update and, once powered on, it connects to the CUPS server\. When the CUPS server finds a match in the version of the gateway, it schedules a firmware update\.

A task is a task definition in process\. As you specified automatic task creation by setting `AutoCreateTasks` to `True`, the firmware update task starts as soon as a matching gateway is found\.

You can track the progress of the task by using the `GetWirelessGatewayTask` API\. When you run the [get\-wireless\-gateway\-task](https://docs.aws.amazon.com/cli/latest/reference/iotwireless/get-wireless-gateway-task.html) command the first time, it will show the task status as `IN_PROGRESS`\.

```
aws iotwireless get-wireless-gateway-task \ 
    --id 1352172b-0602-4b40-896f-54da9ed16b57
```

Following shows the output of the command\.

```
{
    "WirelessGatewayId": "1352172b-0602-4b40-896f-54da9ed16b57",
    "WirelessGatewayTaskDefinitionId": "ec11f9e7-b037-4fcc-aa60-a43b839f5de3",
    "LastUplinkReceivedAt": "2021-03-12T09:56:12.047Z",
    "TaskCreatedAt": "2021-03-12T09:56:12.047Z",
    "Status": "IN_PROGRESS"
}
```

When you run the command the next time, if the firmware update takes effect, it will show the updated fields, `Package`, `Version`, and `Model` and the task status changes to `COMPLETED`\.

```
aws iotwireless get-wireless-gateway-task \ 
    --id 1352172b-0602-4b40-896f-54da9ed16b57
```

The following shows the output of the command\.

```
{
    "WirelessGatewayId": "1352172b-0602-4b40-896f-54da9ed16b57",
    "WirelessGatewayTaskDefinitionId": "ec11f9e7-b037-4fcc-aa60-a43b839f5de3",
    "LastUplinkReceivedAt": "2021-03-12T09:56:12.047Z",
    "TaskCreatedAt": "2021-03-12T09:56:12.047Z",
    "Status": "COMPLETED"
}
```

In this example, we showed you the firmware update using the Raspberry Pi based RAKWireless gateway\. The firmware update script stops the running BasicStation to store the updated `Package`, `Version`, and `Model` fields so BasicStation will have to be restarted\.

```
2021-03-12 09:56:13.108 [CUP:INFO] CUPS provided update.bin
2021-03-12 09:56:13.108 [CUP:INFO] CUPS provided signature len=70 keycrc=37316C36
2021-03-12 09:56:13.148 [CUP:INFO] ECDSA key#0 -> VERIFIED
2021-03-12 09:56:13.148 [CUP:INFO] Running update.bin as background process
2021-03-12 09:56:13.149 [SYS:VERB] /tmp/update.bin: Forked, waiting...
2021-03-12 09:56:13.151 [SYS:INFO] Process /tmp/update.bin (pid=6873) completed
2021-03-12 09:56:13.152 [CUP:INFO] Interaction with CUPS done - next regular check in 10s
```

If the firmware update fails, you see a status of `FIRST_RETRY` from the CUPS server, and the gateway sends the same request\. If the CUPS server is unable to connect to the gateway after a `SECOND_RETRY`, it will show a status of `FAILED`\.

After the previous task was `COMPLETED` or `FAILED`, delete the old task by using the [delete\-wireless\-gateway\-task](https://docs.aws.amazon.com/cli/latest/reference/iotwireless/delete-wireless-gateway-task.html) command before starting a new one\.

```
aws iotwireless delete-wireless-gateway-task \ 
    --id 1352172b-0602-4b40-896f-54da9ed16b57
```