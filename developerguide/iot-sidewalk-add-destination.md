# Add a destination for your Sidewalk device<a name="iot-sidewalk-add-destination"></a>

Before you can add an AWS IoT Core for LoRaWAN destination and create a rule for routing the messages sent from your Sidewalk device, you must create a wireless connectivity profile\. To create the profile, first register your Sidewalk device, and then add the credentials to your AWS account\. For more information, see [Add your Sidewalk account credentials](iot-sidewalk-add-credentials.md)\.

Creating a Sidewalk destination is similar to how you create a destination for your LoRaWAN devices\. The following shows how you can create a destination by using the AWS Management Console or the API\.

## Add a destination by using the console<a name="iot-sidewalk-destination-console"></a>

You can add your Sidewalk destination from the [ Destinations](https://console.aws.amazon.com/iot/home#/wireless/destinations) page of the AWS IoT console\.

Specify the following fields when creating an AWS IoT Core for LoRaWAN destination, and then choose **Add destination**\.
+ 

**Destination details**  
Enter a **Destination name** and an optional description for your destination\. For the **Destination name**, enter **SidewalkDestination**\. You can optionally enter a description, such as **This is a destination for Sidewalk devices**\.
+ 

**Rule name**  
The AWS IoT rule that is configured to process the device's data\. Your destination needs a rule to process the messages it receives\. Enter a rule name \(say **SidewalkRule**\) and then choose **Copy** to copy the rule name that you'll enter when creating the AWS IoT rule\. You can either choose **Create rule** to create the rule now or navigate to the [Rules](https://console.aws.amazon.com/iot/home#/create/rule) Hub of the AWS IoT console and create a rule with the name you copied\.

  For more information about AWS IoT rules for destinations, see [ Create rules to process Sidewalk device messages](https://docs.aws.amazon.com/iot/latest/developerguide/iot-sidewalk-create-rules.html)\.
+ 

**Role name**  
The IAM role that gives the device's data permission to access the rule named in **Rule name**\. To create the IAM role, follow the steps described in [Create an IAM role for your destinations](connect-iot-lorawan-create-destinations.md#connect-iot-lorawan-create-destinations-roles)\. When creating the role:
  + For **Select type of trusted entity**, choose **AWS service**, and then choose **IoT** as the service\.
  + Enter **SidewalkRole** for the **Role name**\.
  + Use the same policy document as described in [Create an IAM role for your destinations](connect-iot-lorawan-create-destinations.md#connect-iot-lorawan-create-destinations-roles)\.

  For more information about IAM roles, see [Using IAM roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use.html)\.

## Add a destination by using the API<a name="iot-sidewalk-destination-api"></a>

The following lists describe the API actions that perform the tasks associated with adding, updating, or deleting a destination\.

**AWS IoT Wireless API actions for service profiles**
+ [CreateDestination](https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_CreateDestination.html)
+ [GetDestination](https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_GetDestination.html)
+ [ListDestinations](https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_ListDestinations.html)
+ [ UpdateDestination](https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_UpdateDestination.html)
+ [DeleteDestination](https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_DeleteDestination.html)

For the complete list of actions and data types available to create and manage AWS IoT Core for LoRaWAN resources, see the [AWS IoT Wireless API reference](https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/welcome.html)\.

**How to use the AWS CLI to add a destination**  
You can use the AWS CLI to add a destination by using the [create\-destination](cli/latest/reference/iotwireless/create-destination.html) command\. The following example creates a destination\.

```
aws iotwireless create-destination \
    --name SidewalkDestination \
    --expression-type RuleName \
    --expression SidewalkRule \
    --role-arn arn:aws:iam::123456789012:role/SidewalkRole
```

Running this command creates a destination with the specified destination name, rule name, and role name\. For information about rule and role names for destinations, see [ Create rules to process Sidewalk device messages](https://docs.aws.amazon.com/iot/latest/developerguide/iot-sidewalk-create-rules.html)\.

For information about the CLIs that you can use, see [AWS CLI reference](https://docs.aws.amazon.com/cli/latest/reference/iotwireless/index.html)\. 

## Next steps<a name="iot-sidewalk-destination-next-steps"></a>

Now that you've added the destination, you can create the destination rule for your Sidewalk device that will route messages to other services\. For more information, see [Create rules to process Sidewalk device messages](iot-sidewalk-create-rules.md)\.