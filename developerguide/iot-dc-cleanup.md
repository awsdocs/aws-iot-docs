# Tutorial: Cleaning up after running the AWS IoT Device Client tutorials<a name="iot-dc-cleanup"></a>

The procedures in this tutorial walk you through removing the files and resources you created while completing the tutorials in this learning path\.

**Topics**
+ [Step 1: Cleaning up your devices after building demos with the AWS IoT Device Client](#iot-dc-cleanup-devices)
+ [Step 2: Cleaning up your AWS account after building demos with the AWS IoT Device Client](#iot-dc-cleanup-cloud)

## Step 1: Cleaning up your devices after building demos with the AWS IoT Device Client<a name="iot-dc-cleanup-devices"></a>

This tutorial describes two options for how to clean up the microSD card after you built the demos in this learning path\. Choose the option that provides the level of security that you need\.

Note that cleaning the device's microSD card does not remove any AWS IoT resources that you created\. To clean up the AWS IoT resources after you clean the device's microSD card, you should review the tutorial on [Step 2: Cleaning up your AWS account after building demos with the AWS IoT Device Client](#iot-dc-cleanup-cloud)\.

### Option 1: Cleaning up by rewriting the microSD card<a name="iot-dc-cleanup-devices-flash"></a>

The easiest and most thorough way to clean the microSD card after completing the tutorials in this learning path is to overwrite the microSD card with a saved image file that you created while preparing your device the first time\.

This procedure uses the local host computer to write a saved microSD card image to a microSD card\.

**Note**  
If your device doesn't use a removable storage medium for its operating system, refer to the procedure for that device\.

**To write a new image to the microSD card**

1. On your local host computer, locate the saved microSD card image that you want to write to your microSD card\. 

1. Insert your microSD card into the local host computer\.

1. Using an SD card imaging tool, write selected image file to the microSD card\.

1. After writing the Raspberry Pi OS image to the microSD card, eject the microSD card and safely remove it from the local host computer\.

Your microSD card is ready to use\.

### Option 2: Cleaning up by deleting user directories<a name="iot-dc-cleanup-devices-dirs"></a>

To clean the microSD card after completing the tutorials without rewriting the microSD card image, you can delete the user directories individually\. This is not as thorough as rewriting the microSD card from a saved image because it does not remove any system files that might have been installed\.

If removing the user directories is sufficiently thorough for you needs, you can follow this procedure\.

**To delete this learning path's user directories from your device**

1. Run these commands to delete the user directories, subdirectories, and all their files that were created in this learning path, in the terminal window connected to your device\.
**Note**  
After you delete these directories and files, you won't be able to run the demos without completing the tutorials again\.

   ```
   rm -Rf ~/dc-configs
   rm -Rf ~/policies
   rm -Rf ~/messages
   rm -Rf ~/certs
   rm -Rf ~/.aws-iot-device-client
   ```

1. Run these commands to delete the application source directories and files, in the terminal window connected to your device\.
**Note**  
These commands don't uninstall any programs\. They only remove the source files used to build and install them\. After you delete these files, the AWS CLI and the AWS IoT Device Client might not work\.

   ```
   rm -Rf ~/aws-cli
   rm -Rf ~/aws
   rm -Rf ~/aws-iot-device-client
   ```

## Step 2: Cleaning up your AWS account after building demos with the AWS IoT Device Client<a name="iot-dc-cleanup-cloud"></a>

These procedures help you identify and remove the AWS resources that you created while completing the tutorials in this learning path\.

### Clean up AWS IoT resources<a name="iot-dc-cleanup-cloud-iot"></a>

This procedure helps you identify and remove the AWS IoT resources that you created while completing the tutorials in this learning path\.


**AWS IoT resources created in this learning path**  

| Tutorial | Thing resource | Policy resource | 
| --- | --- | --- | 
|  [Tutorial: Installing and configuring the AWS IoT Device Client](iot-dc-install-dc.md)  |  **DevCliTestThing**  | DevCliTestThingPolicy | 
|  [Tutorial: Demonstrate MQTT message communication with the AWS IoT Device Client](iot-dc-testconn.md)  |  **PubSubTestThing**  | PubSubTestThingPolicy | 
|  [Tutorial: Demonstrate remote actions \(jobs\) with the AWS IoT Device Client](iot-dc-runjobs.md)  | user defined \(there could be more than one\) |  *user defined* \(there could be more than one\)  | 

**To delete the AWS IoT resources, follow this procedure for each thing resource that you created**

1. Replace `thing_name` with the name of the thing resource you want to delete, and then run this command to list the certificates attached to the thing resource, from the local host computer\.

   ```
   aws iot list-thing-principals --thing-name thing_name
   ```

   This command returns a response like this one that lists the certificates that are attached to `thing_name`\. In most cases, there will only be one certificate in the list\.

   ```
   {
       "principals": [
           "arn:aws:iot:us-west-2:57EXAMPLE833:cert/23853eea3cf0edc7f8a69c74abeafa27b2b52823cab5b3e156295e94b26ae8ac"
       ]
   }
   ```

1. For each certificate listed by the previous command:

   1. Replace `certificate_ID` with the certificate ID from the previous command\. The certificate ID is the alphanumeric characters that follow `cert/` in the ARN returned by the previous command\. Then run this command to inactivate the certificate\.

      ```
      aws iot update-certificate --new-status INACTIVE --certificate-id certificate_ID
      ```

      If successful, this command doesn't return anything\.

   1. Replace `certificate_ARN` with the certificate ARN from the list of certificates returned earlier, and then run this command to list the policies attached to this certificate\.

      ```
      aws iot list-attached-policies --target certificate_ARN
      ```

      This command returns a response like this one that lists the policies attached to the certificate\. In most cases, there will only be one policy in the list\.

      ```
      {
          "policies": [
              {
                  "policyName": "DevCliTestThingPolicy",
                  "policyArn": "arn:aws:iot:us-west-2:57EXAMPLE833:policy/DevCliTestThingPolicy"
              }
          ]
      }
      ```

   1. For each policy attached to the certificate:

      1. Replace `policy_name` with the `policyName` value from the previous command, replace `certificate_ARN` with the certificate's ARN, and then run this command to detach the policy from the certificate\.

         ```
         aws iot detach-policy --policy-name policy_name --target certificate_ARN
         ```

         If successful, this command doesn't return anything\.

      1. Replace `policy_name` with the `policyName` value, and then run this command to see if the policy is attached to any more certificates\.

         ```
         aws iot list-targets-for-policy --policy-name policy_name
         ```

         If the command returns an empty list like this, the policy is not attached to any certificates and you continue to list the policy versions\. If there are still certificates attached to the policy, continue with the detach\-thing\-principal step\.

         ```
         {
             "targets": []
         }
         ```

      1. Replace `policy_name` with the `policyName` value, and then run this command to check for policy versions\. To delete the policy, it must have only one version\.

         ```
         aws iot list-policy-versions --policy-name policy_name
         ```

         If the policy has only one version, like this example, you can skip to the delete\-policy step and delete the policy now\.

         ```
         {
             "policyVersions": [
                 {
                     "versionId": "1",
                     "isDefaultVersion": true,
                     "createDate": "2021-11-18T01:02:46.778000+00:00"
                 }
             ]
         }
         ```

         If the policy has more than one version, like this example, the policy versions with an `isDefaultVersion` value of `false` must be deleted before the policy can be deleted\.

         ```
         {
             "policyVersions": [
                 {
                     "versionId": "2",
                     "isDefaultVersion": true,
                     "createDate": "2021-11-18T01:52:04.423000+00:00"
                 },
                 {
                     "versionId": "1",
                     "isDefaultVersion": false,
                     "createDate": "2021-11-18T01:30:18.083000+00:00"
                 }
             ]
         }
         ```

         If you need to delete a policy version, replace `policy_name` with the `policyName` value, replace `version_ID` with the `versionId` value from the previous command, and then run this command to delete a policy version\.

         ```
         aws iot delete-policy-version --policy-name policy_name --policy-version-id version_ID
         ```

         If successful, this command doesn't return anything\.

         After you delete a policy version, repeat this step until the policy has only one policy version\.

      1. Replace `policy_name` with the `policyName` value, and then run this command to delete the policy\.

         ```
         aws iot delete-policy --policy-name policy_name
         ```

   1. Replace *`thing_name`* with the thing's name, replace `certificate_ARN` with the certificate's ARN, and then run this command to detach the certificate from the thing resource\.

      ```
      aws iot detach-thing-principal --thing-name thing_name --principal certificate_ARN
      ```

      If successful, this command doesn't return anything\.

   1. Replace `certificate_ID` with the certificate ID from the previous command\. The certificate ID is the alphanumeric characters that follow `cert/` in the ARN returned by the previous command\. Then run this command to delete the certificate resource\.

      ```
      aws iot delete-certificate --certificate-id certificate_ID
      ```

      If successful, this command doesn't return anything\.

1. Replace *`thing_name`* with the thing's name, and then run this command to delete the thing\.

   ```
   aws iot delete-thing --thing-name thing_name
   ```

   If successful, this command doesn't return anything\. 

### Clean up AWS resources<a name="iot-dc-cleanup-cloud-aws"></a>

This procedure helps you identify and remove other AWS resources that you created while completing the tutorials in this learning path\.


**Other AWS resources created in this learning path**  

| Tutorial | Resource type | Resource name or ID | 
| --- | --- | --- | 
|  [Tutorial: Demonstrate remote actions \(jobs\) with the AWS IoT Device Client](iot-dc-runjobs.md)  | Amazon S3 object | hello\-world\-job\.json | 
|  [Tutorial: Demonstrate remote actions \(jobs\) with the AWS IoT Device Client](iot-dc-runjobs.md)  |  AWS IoT job resources  | user defined | 

**To delete the AWS resources created in this learning path**

1. To delete the jobs created in this learning path

   1. Run this command to list the jobs in your AWS account\.

      ```
      aws iot list-jobs
      ```

      The command returns a list of the AWS IoT jobs in your AWS account and AWS Region that looks like this\.

      ```
      {
          "jobs": [
              {
                  "jobArn": "arn:aws:iot:us-west-2:57EXAMPLE833:job/hello-world-job-2",
                  "jobId": "hello-world-job-2",
                  "targetSelection": "SNAPSHOT",
                  "status": "COMPLETED",
                  "createdAt": "2021-11-16T23:40:36.825000+00:00",
                  "lastUpdatedAt": "2021-11-16T23:40:41.375000+00:00",
                  "completedAt": "2021-11-16T23:40:41.375000+00:00"
              },
              {
                  "jobArn": "arn:aws:iot:us-west-2:57EXAMPLE833:job/hello-world-job-1",
                  "jobId": "hello-world-job-1",
                  "targetSelection": "SNAPSHOT",
                  "status": "COMPLETED",
                  "createdAt": "2021-11-16T23:35:26.381000+00:00",
                  "lastUpdatedAt": "2021-11-16T23:35:29.239000+00:00",
                  "completedAt": "2021-11-16T23:35:29.239000+00:00"
              }
          ]
      }
      ```

   1. For each job that you recognize from the list as a job you created in this learning path, replace `jobId` with the `jobId` value of the job to delete, and then run this command to delete an AWS IoT job\.

      ```
      aws iot delete-job --job-id jobId
      ```

      If the command is successful, it returns nothing\.

1. To delete the job documents you stored in an Amazon S3 bucket in this learning path\.

   1. Replace `bucket` with the name of the bucket you used, and then run this command to list the objects in the Amazon S3 bucket that you used\.

      ```
      aws s3api list-objects --bucket bucket
      ```

      The command returns a list of the Amazon S3 objects in bucket that looks like this\.

      ```
      {
          "Contents": [
              {
                  "Key": "hello-world-job.json",
                  "LastModified": "2021-11-18T03:02:12+00:00",
                  "ETag": "\"868c8bc3f56b5787964764d4b18ed5ef\"",
                  "Size": 54,
                  "StorageClass": "STANDARD",
                  "Owner": {
                      "DisplayName": "EXAMPLE",
                      "ID": "e9e3d6ec1EXAMPLEf5bfb5e6bd0a2b6ed03884d1ed392a82ad011c144736a4ee"
                  }
              },
              {
                  "Key": "iot_job_firmware_update.json",
                  "LastModified": "2021-04-13T21:57:07+00:00",
                  "ETag": "\"7c68c591949391791ecf625253658c61\"",
                  "Size": 66,
                  "StorageClass": "STANDARD",
                  "Owner": {
                      "DisplayName": "EXAMPLE",
                      "ID": "e9e3d6ec1EXAMPLEf5bfb5e6bd0a2b6ed03884d1ed392a82ad011c144736a4ee"
                  }
              },
              {
                  "Key": "order66.json",
                  "LastModified": "2021-04-13T21:57:07+00:00",
                  "ETag": "\"bca60d5380b88e1a70cc27d321caba72\"",
                  "Size": 29,
                  "StorageClass": "STANDARD",
                  "Owner": {
                      "DisplayName": "EXAMPLE",
                      "ID": "e9e3d6ec1EXAMPLEf5bfb5e6bd0a2b6ed03884d1ed392a82ad011c144736a4ee"
                  }
              }
          ]
      }
      ```

   1. For each object that you recognize from the list as an object you created in this learning path, replace `bucket` with the bucket name and `key` with key value of the object to delete, and then run this command to delete an Amazon S3 object\.

      ```
       aws s3api delete-object --bucket bucket --key key
      ```

      If the command is successful, it returns nothing\.

After you delete all the AWS resources and objects that you created while completing this learning path, you can start over and repeat the tutorials\.