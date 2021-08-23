# Add destinations to AWS IoT Core for LoRaWAN<a name="connect-iot-lorawan-create-destinations"></a>

AWS IoT Core for LoRaWAN destinations describe the AWS IoT rule that processes a device's data for use by AWS services\.

Because most LoRaWAN devices don't send data to AWS IoT Core for LoRaWAN in a format that can be used by AWS services, an AWS IoT rule must process it first\. The AWS IoT rule contains the SQL statement that interprets the device's data and the topic rule actions that send the result of the SQL statement to the services that will use it\.

If you're adding your destination for the first time, we recommend that you use the console\. 

## Add a destination using the console<a name="connect-iot-lorawan-create-destination-console"></a>

If you're adding a wireless device using the console as described in [Add your wireless device specification to AWS IoT Core for LoRaWAN using the console](connect-iot-lorawan-end-devices-add.md#connect-iot-lorawan-end-device-spec-console), after you've already added the wireless device specification and profiles to AWS IoT Core for LoRaWAN as described previously, you can go ahead and add a destination\.

Alternatively, you can also add an AWS IoT Core for LoRaWAN destination from the [ Destinations](https://console.aws.amazon.com/iot/home#/wireless/destinations) page of the AWS IoT console\.

To process a device's data, specify the following fields when creating an AWS IoT Core for LoRaWAN destination, and then choose **Add destination**\.
+ 

**Destination details**  
Enter a **Destination name** and an optional description for your destination\.
+ 

**Rule name**  
The AWS IoT rule that is configured to evaluate messages sent by your device and process the device's data\. The rule name will be mapped to your destination\. The destination requires the rule to process the messages that it receives\. You can choose for the messages to be processed by either invoking an AWS IoT rule or by publishing to the AWS IoT message broker\.
  + If you choose **Enter a rule name**, enter a name, and then choose **Copy** to copy the rule name that you'll enter when creating the AWS IoT rule\. You can either choose **Create rule** to create the rule now or navigate to the [Rules](https://console.aws.amazon.com/https://console.aws.amazon.com/iot/home#/create/rule) Hub of the AWS IoT console and create a rule with that name\.

    You can also enter a rule and use the **Advanced** setting to specify a topic name\. The topic name is provided during rule invocation and is accessed by using the `topic` expression inside the rule\. For more information about AWS IoT rules, see [Rules for AWS IoT](iot-rules.md)\.
  + If you choose **Publish to AWS IoT message broker**, enter a topic name\. You can then copy the MQTT topic name and multiple subscribers can subscribe to this topic to receive messages published to that topic\. For more information, see [MQTT topics](topics.md)\.

  For more information about AWS IoT rules for destinations, see [Create rules to process LoRaWAN device messages](connect-iot-lorawan-destination-rules.md)\.
+ 

**Role name**  
The IAM role that grants the device's data permission to access the rule named in **Rule name**\. In the console, you can create a new service role or select an existing service role\. If you're creating a new service role, you can either enter a role name \(for example, **IoTWirelessDestinationRole**\), or leave it blank for AWS IoT Core for LoRaWAN to generate a new role name\. AWS IoT Core for LoRaWAN will then automatically create the IAM role with the appropriate permissions on your behalf\.

  For more information about IAM roles, see [Using IAM roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use.html)\.

## Add a destination by using the API<a name="connect-iot-lorawan-create-destination-api"></a>

If you want to add a destination using the CLI instead, you must have already created the rule and IAM role for your destination\. For more information about the details that a destination requires in the role, see [Create an IAM role for your destinations](#connect-iot-lorawan-create-destinations-roles)\.

The following list contains the API actions that perform the tasks associated with adding, updating, or deleting a destination\.

**AWS IoT Wireless API actions for destinations**
+ [CreateDestination](https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_CreateDestination.html)
+ [GetDestination](https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_GetDestination.html)
+ [ListDestinations](https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_ListDestinations.html)
+ [ UpdateDestination](https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_UpdateDestination.html)
+ [DeleteDestination](https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_DeleteDestination.html)

For the complete list of the actions and data types available to create and manage AWS IoT Core for LoRaWAN resources, see the [AWS IoT Wireless API reference](https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/welcome.html)\.

**How to use the AWS CLI to add a destination**  
You can use the AWS CLI to add a destination by using the [create\-destination](https://docs.aws.amazon.com/cli/latest/reference/iotwireless/create-destination.html) command\. The following example shows how to create a destination by entering a rule name by using `RuleName` as the value for the `expression-type` parameter\. If you want to specify a topic name for publishing or subscribing to the message broker, change the `expression-type` parameter's value to `MqttTopic`d\.

```
aws iotwireless create-destination \
    --name IoTWirelessDestination \
    --expression-type RuleName \
    --expression IoTWirelessRule \
    --role-arn arn:aws:iam::123456789012:role/IoTWirelessDestinationRole
```

Running this command creates a destination with the specified destination name, rule name, and role name\. For information about rule and role names for destinations, see [Create rules to process LoRaWAN device messages](connect-iot-lorawan-destination-rules.md) and [Create an IAM role for your destinations](#connect-iot-lorawan-create-destinations-roles)\.

For information about the CLIs that you can use, see [AWS CLI reference](https://docs.aws.amazon.com/cli/latest/reference/iotwireless/index.html)\. 

## Create an IAM role for your destinations<a name="connect-iot-lorawan-create-destinations-roles"></a>

AWS IoT Core for LoRaWAN destinations require IAM roles that give AWS IoT Core for LoRaWAN the permissions necessary to send data to the AWS IoT rule\. If such a role is not already defined, uou must define it so that it will appear in the list of roles\.

When you use the console to add a destination, AWS IoT Core for LoRaWAN automatically creates an IAM role for you, as described previously in this topic\. When you add a destination using the API or CLI, you must create the IAM role for your destination\.

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