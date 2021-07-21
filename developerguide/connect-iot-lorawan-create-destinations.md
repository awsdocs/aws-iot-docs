# Add destinations to AWS IoT Core for LoRaWAN<a name="connect-iot-lorawan-create-destinations"></a>

AWS IoT Core for LoRaWAN destinations describe the AWS IoT rule that processes a device's data for use by AWS services\.

Because most LoRaWAN devices don't send data to AWS IoT Core for LoRaWAN in a format that can be used by AWS services, an AWS IoT rule must process it first\. The AWS IoT rule contains the SQL statement that interprets the device's data and the topic rule actions that send the result of the SQL statement to the services that will use it\.

## Add a destination using the console<a name="connect-iot-lorawan-create-destination-console"></a>

If you're adding a wireless device using the console as described in [Add your wireless device specification to AWS IoT Core for LoRaWAN using the console](connect-iot-lorawan-end-devices-add.md#connect-iot-lorawan-end-device-spec-console), after you've already added the wireless device specification and profiles to AWS IoT Core for LoRaWAN as described previously, you can go ahead and add a destination\.

Alternatively, you can also add an AWS IoT Core for LoRaWAN destination from the [ Destinations](https://console.aws.amazon.com/iot/home#/wireless/destinations) page of the AWS IoT console\.

To process a device's data, specify the following fields when creating an AWS IoT Core for LoRaWAN destination, and then choose **Add destination**\.
+ 

**Destination details**  
Enter a **Destination name** and an optional description for your destination\.
+ 

**Rule name**  
The AWS IoT rule that is configured to process the device's data\. Your destination will need a rule to process the messages it receives\. Enter a rule name and then choose **Copy** to copy the rule name that you'll enter when creating the AWS IoT rule\. You can either choose **Create rule** to create the rule now or navigate to the [ Rules](https://console.aws.amazon.com/iot/home#/create/rule) Hub of the AWS IoT console and create a rule with that name\.

  For more information about AWS IoT rules for destinations, see [Create rules to process LoRaWAN device messages](connect-iot-lorawan-destination-rules.md)\.
+ 

**Role name**  
The IAM role that gives the device's data permission to access the rule named in **Rule name**\. For more information about the details that a definition requires in the role, see [Create an IAM roles for your destinations](#connect-iot-lorawan-create-destinations-roles)

  For more information about IAM roles, see [Using IAM roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use.html)\.

## Add a destination by using the API<a name="connect-iot-lorawan-create-destination-api"></a>

The following lists describe the API actions that perform the tasks associated with adding, updating, or deleting a destination\.

**AWS IoT Wireless API actions for service profiles**
+ [CreateDestination](https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_CreateDestination.html)
+ [GetDestination](https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_GetDestination.html)
+ [ListDestinations](https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_ListDestinations.html)
+ [ UpdateDestination](https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_UpdateDestination.html)
+ [DeleteDestination](https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_DeleteDestination.html)

For the complete list of the actions and data types available to create and manage AWS IoT Core for LoRaWAN resources, see the [AWS IoT Wireless API reference](https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/welcome.html)\.

**How to use the AWS CLI to add a destination**  
You can use the AWS CLI to add a destination by using the [create\-destination](cli/latest/reference/iotwireless/create-destination.html) command\. The following example creates a destination\.

```
aws iotwireless create-destination \
    --name IoTWirelessDestination \
    --expression-type RuleName \
    --expression IoTWirelessRule \
    --role-arn arn:aws:iam::123456789012:role/IoTWirelessDestinationRole
```

Running this command creates a destination with the specified destination name, rule name, and role name\. For information about rule and role names for destinations, see [Create rules to process LoRaWAN device messages](connect-iot-lorawan-destination-rules.md) and [Create an IAM roles for your destinations](#connect-iot-lorawan-create-destinations-roles)\.

For information about the CLIs that you can use, see [AWS CLI reference](https://docs.aws.amazon.com/cli/latest/reference/iotwireless/index.html)\. 

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