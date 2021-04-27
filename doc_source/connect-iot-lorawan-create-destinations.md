# Add destinations to AWS IoT Core for LoRaWAN<a name="connect-iot-lorawan-create-destinations"></a>

AWS IoT Core for LoRaWAN destinations describe the AWS IoT rule that processes a device's data for use by AWS services\.

Because most LoRaWAN devices don't send data to AWS IoT Core for LoRaWAN in a format that can be used by AWS services, an AWS IoT rule must process it first\. The AWS IoT rule contains the SQL statement that interprets the device's data and the topic rule actions that send the result of the SQL statement to the services that will use it\.

To process a device's data, an AWS IoT Core for LoRaWAN destination contains the following elements\.
+ 

**Role name**  
The IAM role that gives the device's data permission to access the rule named in **Rule name**\. For more information about the details that a definition requires in the role, see [Create an IAM roles for your destinations](#connect-iot-lorawan-create-destinations-roles)

  For more information about IAM roles, see [Using IAM roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use.html)\.
+ 

**Rule name**  
The AWS IoT rule that is configured to process the device's data\.

  For more information about AWS IoT rules for destinations, see [Create rules to process LoRaWAN device messages](connect-iot-lorawan-destination-rules.md)\.

## Create an IAM roles for your destinations<a name="connect-iot-lorawan-create-destinations-roles"></a>

AWS IoT Core for LoRaWAN destinations require IAM roles that give AWS IoT Core for LoRaWAN the permissions necessary to send data to the AWS IoT rule\. If such a role is not already defined, you'll need to define it so that it will appear in the list of roles\.

**To create an IAM policy for your AWS IoT Core for LoRaWAN destination role**

1. Open the [ Policies hub of the IAM console](https://console.aws.amazon.com/iam/home#/policies)\.

1. Choose **Create policy**, and choose the **JSON** tab\.

1. In the editor, delete any content from the editor and paste this policy document\.

   ```
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Effect": "Allow",
               "Action": [
                   "iot:DescribeEndpoint",
                   "iot:Publish"
               ],
               "Resource": "*"
           }
       ]
   }
   ```

1. Choose **Review policy**, and in **Name**, enter a name for this policy\. You'll need this name to use in the next procedure\.

   You can also describe this policy in **Description**, if you want\.

1. Choose **Create policy**\.

**To create an IAM role for an AWS IoT Core for LoRaWAN destination**

1. Open the [ Roles hub of the IAM console](https://console.aws.amazon.com/iam/home#/roles) and choose **Create role**\.

1. In **Select type of trusted entity**, choose **Another AWS account**\.

1. In **Account ID**, enter your AWS account ID, and then choose **Next: Permissions**\.

1. In the search box, enter the name of the IAM policy that you created in the previous procedure\.

1. In the search results, check the IAM policy that you created in the previous procedure\.

1. Choose **Next: Tags**, and then choose **Next: Review**\.

1. In **Role name**, enter the name of this role, and then choose **Create role**\.

1. In the confirmation message, choose the name of the role you created to edit the new role\.

1. In **Summary**, choose the **Trust relationships** tab, and then choose **Edit trust relationship**\.

1. In **Policy Document**, change the `Principal` property to look like this example\.

   ```
   "Principal": { 
       "Service": "iotwireless.amazonaws.com" 
   },
   ```

   After you change the `Principal` property, the complete policy document should look like this example\.

   ```
   {
     "Version": "2012-10-17",
     "Statement": [
       {
         "Effect": "Allow",
         "Principal": {
           "Service": "iotwireless.amazonaws.com"
         },
         "Action": "sts:AssumeRole",
         "Condition": {}
       }
     ]
   }
   ```

1. To save your changes and exit, choose **Update Trust Policy**\.

With this role defined, you can find it in the list of roles when you configure your AWS IoT Core for LoRaWAN destinations\.