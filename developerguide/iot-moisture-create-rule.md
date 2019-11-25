# Create an AWS IoT Rule to Send an Email<a name="iot-moisture-create-rule"></a>

An AWS IoT rule defines a query and one or more actions to take when a message is received from a device\. The AWS IoT rules engine listens for messages sent by devices and uses the data in the messages to determine if some action should be taken\. For more information, see [Rules for AWS IoT ](iot-rules.md)\. 

In this tutorial, your Raspberry Pi publishes messages on `aws/things/RaspberryPi/shadow/update`\. This is an internal MQTT topic used by devices and the Thing Shadow service\. The Raspberry Pi publishes messages that have the following form:

```
{
    "reported": {
        "moisture" : <moisture-reading>,
        "temp" : <temperature-reading>
    }
}
```

You create a query that extracts the moisture and temperature data from the incoming message\. You also create an Amazon SNS action that takes the data and sends it to Amazon SNS topic subscribers if the moisture reading is below a threshold value\.

**Create an Amazon SNS rule**

1. In the [AWS IoT console](https://console.aws.amazon.com/iot/home), in the navigation pane, choose **Act**\. If a **You don’t have any rules yet** dialog box appears, choose **Create a rule**\. Otherwise, choose **Create**\.

1. In the **Create a rule** page, enter a name for your rule \(for example, **MoistureSensorRule**\)\.

1. For **Description**, provide a short description for this rule \(for example, **Sends an alert when soil moisture level readings are too low**\)\.

1. Under **Rule query statement**, choose SQL version **2016\-03\-23**, and enter the following AWS IoT SQL query statement:

   ```
   SELECT * FROM '$aws/things/RaspberryPi/shadow/update/accepted' WHERE state.reported.moisture < 400
   ```

   This statement triggers the rule action when the `moisture` reading is less than `400`\.
**Note**  
You might have to use a different value\. After you have the code running on your Raspberry Pi, you can see the values you get from your sensor by touching the sensor, placing it in water, or placing it in a planter\. 

1. Under **Set one or more actions**, choose **Add action**\.

1. On the **Select an action** page, choose **Send a message as an SNS push notification**\.

1. Scroll to the bottom of the page, and then choose **Configure action**\.

1. On the **Configure action** page, for **SNS target**, choose **Select**, and then choose **LowMoistureTopic**\. 

1. For **Message format**, choose **RAW**\.

1. Under **Choose or create a role to grant AWS IoT access to perform this action**, choose **Create role**\. Enter a name for the role \(for example, **LowMoistureTopicRole**\), and then choose **Create role**\.

1. Choose **Add action**\.

1. Choose **Create rule**\.