# Store device data in a DynamoDB table<a name="iot-ddb-rule"></a>

This tutorial demonstrates how to create an AWS IoT rule that sends message data to a DynamoDB table\.

In this tutorial, you create a rule that sends message data from an imaginary weather sensor device to a DynamoDB table\. The rule formats the data from many weather sensors such that they can be added to a single database table\.

**What you'll learn in this tutorial**
+ How to create a DynamoDB table
+ How to send message data to a DynamoDB table from an AWS IoT rule
+ How to use substitution templates in an AWS IoT rule
+ How to use simple SQL queries and functions in a rule query statement
+ How to use the MQTT client to test an AWS IoT rule

This tutorial takes about 30 minutes to complete\.

**Topics**
+ [Create the DynamoDB table for this tutorial](#iot-ddb-rule-ddb-table)
+ [Create an AWS IoT rule to send data to the DynamoDB table](#iot-ddb-rule-topic-rule)
+ [Test the AWS IoT rule and DynamoDB table](#iot-ddb-rule-test)
+ [Review the results and next steps](#iot-ddb-rule-review)

**Before you start this tutorial, make sure that you have:**
+ 

**[Set up your AWS account](setting-up.md)**  
You'll need your AWS account and AWS IoT console to complete this tutorial\.
+ 

**Reviewed [View MQTT messages with the AWS IoT MQTT client](view-mqtt-messages.md)**  
Be sure you can use the MQTT client to subscribe and publish to a topic\. You'll use the MQTT client to test your new rule in this procedure\.
+ 

**Reviewed the [Amazon DynamoDB](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Introduction.html) overview**  
If you've not used DynamoDB before, review [Getting Started with DynamoDB](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/GettingStartedDynamoDB.html) to become familiar with the basic concepts and operations of DynamoDB\.

## Create the DynamoDB table for this tutorial<a name="iot-ddb-rule-ddb-table"></a>

In this tutorial, you'll create a DynamoDB table with these attributes to record the data from the imaginary weather sensor devices: 
+ `sample_time` is a primary key and describes the time the sample was recorded\.
+ `device_id` is a sort key and describes the device that provided the sample 
+ `device_data` is the data received from the device and formatted by the rule query statement

**To create the DynamoDB table for this tutorial**

1. Open the [DynamoDB console](https://console.aws.amazon.com/dynamodb/home), and then choose **Create table**\.

1. In **Create DynamoDB table**:

   1.  In **Table name**, enter the table name: **wx\_data**\.

   1. In **Primary key**, in **Partition key**, enter **sample\_time**, and in the option list next to the field, choose **Number**\.

   1. Check **Add sort key**\.

   1. In the field that appears below **Add sort key**, enter **device\_id**, and in the option list next to the field, choose **Number**\.

   1. At the bottom of the page, choose **Create**\.

You'll define `device_data` later, when you configure the DynamoDB rule action\.

## Create an AWS IoT rule to send data to the DynamoDB table<a name="iot-ddb-rule-topic-rule"></a>

In this step, you'll use the rule query statement to format the data from the imaginary weather sensor devices to write to the database table\.

A sample message payload received from a weather sensor devices looks like this:

```
{
  "temperature": 28,
  "humidity": 80,
  "barometer": 1013,
  "wind": {
    "velocity": 22,
    "bearing": 255
  }
}
```

For the database entry, you'll use the rule query statement to flatten the structure of the message payload to look like this:

```
{
  "temperature": 28,
  "humidity": 80,
  "barometer": 1013,
  "wind_velocity": 22,
  "wind_bearing": 255
}
```

In this rule, you'll also use a couple of [Substitution templates](iot-substitution-templates.md)\. Substitution templates are expressions that let you insert dynamic values from functions and message data\.

**To create the AWS IoT rule to send data to the DynamoDB table**

1. Open [the **Rules** hub of the AWS IoT console](https://console.aws.amazon.com/iot/home#/rulehub)\.

1. To start creating your new rule in **Rules**, choose **Create**\.

1. In the top part of **Create a rule**:

   1. In **Name**, enter the rule's name, **wx\_data\_ddb**\.

      Remember that a rule name must be unique within your AWS Account and Region, and it can't have any spaces\. We've used an underscore character in this name to separate the two words in the rule's name\.

   1. In **Description**, describe the rule\. 

      A meaningful description makes it easier to remember what this rule does and why you created it\. The description can be as long as needed, so be as detailed as possible\. 

1. In **Rule query statement** of **Create a rule**:

   1. In **Using SQL version**, select **2016\-03\-23**\. 

   1. In the **Rule query statement** edit box, enter the statement: 

      ```
      SELECT temperature, humidity, barometer,
        wind.velocity as wind_velocity,
        wind.bearing as wind_bearing,
      FROM 'device/+/data'
      ```

      This statement:
      + Listens for MQTT messages with a topic that matches the `device/+/data` topic filter\.
      + Formats the elements of the `wind` attribute as individual attributes\.
      + Passes the `temperature`, `humidity`, and `barometer` attributes unchanged\.

1. In **Set one or more actions**:

   1. To open up the list of rule actions for this rule, choose **Add action**\.

   1. In **Select an action**, choose **Insert a message into a DynamoDB table**\.

   1. To open the selected action's configuration page, at the bottom of the action list, choose **Configure action**\.

1. In **Configure action**:

   1. In **Table name**, choose the name of the DynamoDB table you created in a previous step: **wx\_data**\.

      The **Partition key**, **Partition key type**, **Sort key**, and **Sort key type** fields are filled with the values from your DynamoDB table\.

   1. In **Partition key value**\. enter **$\{timestamp\(\)\}**\.

      This is the first of the [Substitution templates](iot-substitution-templates.md) you'll use in this rule\. Instead of using a value from the message payload, it will use the value returned from the [timestamp](iot-sql-functions.md#iot-function-timestamp) function\.

   1. In Sort key value, enter **$\{cast\(topic\(2\) AS DECIMAL\)\}**\. 

      This is the second one of the [Substitution templates](iot-substitution-templates.md) you'll use in this rule\. It inserts the value of the second element in [topic](iot-sql-functions.md#iot-function-topic) name, which is the device's ID, after it [casts](iot-sql-functions.md#iot-sql-function-cast) it to a DECIMAL value to match the numeric format of the key\.

   1. In **Write message data to this column**, enter **device\_data**\.

      This will create the `device_data` column in the DynamoDB table\.

   1. Leave **Operation** blank\.

   1. In **Choose or create a role to grant AWS IoT access to perform this action**, choose **Create Role**\.

   1. In **Create a new role**, enter **wx\_ddb\_role**, and choose **Create role**\.

   1. At the bottom of **Configure action**, choose **Add action**\.

   1. To create the rule, at the bottom of **Create a rule**, choose **Create rule**\.

## Test the AWS IoT rule and DynamoDB table<a name="iot-ddb-rule-test"></a>

To test the new rule, you'll use the MQTT client to publish and subscribe to the MQTT messages used in this test\.

Open the [MQTT client in the AWS IoT console](https://console.aws.amazon.com/iot/home#/test) in a new window\. This will let you edit the rule without losing the configuration of your MQTT client\. The MQTT client does not retain any subscriptions or message logs if you leave it to go to another page in the console\. You'll also want a separate console window open to the [DynamoDB Tables hub in the AWS IoT console](https://console.aws.amazon.com/dynamodb/home#tables:) to view the new entries that your rule sends\.

**To use the MQTT client to test your rule**

1. In the [MQTT client in the AWS IoT console](https://console.aws.amazon.com/iot/home#/test), subscribe to the input topic, `device/+/data`\.

   1. In the MQTT client, under **Subscriptions**, choose **Subscribe to a topic**\.

   1. In **Subscription topic**, enter the topic of the input topic filter, **device/\+/data**\.

   1. Keep the rest of the fields at their default settings\.

   1. Choose **Subscribe to topic**\.

      In the **Subscriptions** column, under **Publish to a topic**, **device/\+/data** appears\. 

   You should see the message that you sent in the **device/\+/data** subscription\.

1. Publish a message to the input topic with a specific device ID, **device/22/data**\. You can't publish to MQTT topics that contain wildcard characters\.

   1. In the MQTT client, under **Subscriptions**, choose **Publish to topic**\.

   1. In the **Publish** field, enter the input topic name, **device/22/data**\.

   1. Copy the sample data shown here and, in the edit box below the topic name, paste the sample data\.

      ```
      {
        "temperature": 28,
        "humidity": 80,
        "barometer": 1013,
        "wind": {
          "velocity": 22,
          "bearing": 255
        }
      }
      ```

   1. To publish the MQTT message, choose **Publish to topic**\.

1. Check to see the row in the DynamoDB table that your rule created\.

   1. In the [DynamoDB Tables hub in the AWS IoT console](https://console.aws.amazon.com/dynamodb/home#tables:), choose **wx\_data**, and then choose the **Items** tab\.

      If you're already on the **Items** tab, you might need to refresh the display by choosing the refresh icon in the upper\-right corner of the table's header\.

   1. Notice that the **sample\_time** values in the table are links and open one\. If you just sent your first message, it will be the only one in the list\.

      This link displays all the data in that row of the table\.

   1. Expand the **device\_data** entry to see the data that resulted from the rule query statement\.

   1. Explore the different representations of the data that are available in this display\. You can also edit the data in this display\.

   1. After you have finished reviewing this row of data, to save any changes you made, choose **Save**, or to exit without saving any changes, choose **Cancel**\.

If you don't see the correct behavior, check the troubleshooting tips\.

### Troubleshooting your DynamoDB rule<a name="iot-ddb-rule-trouble"></a>

Here are some things to check in case you're not seeing the results you expect\.
+ 

**You got an error banner**  
If an error appeared when you published the input message, correct that error first\. The following steps might help you correct that error\.
+ 

**You don't see the input message in the MQTT client**  
Every time you publish your input message to the `device/22/data` topic, that message should appear in the MQTT client if you subscribed to the `device/+/data` topic filter as described in the procedure\.

**Things to check**
  + 

**Check the topic filter you subscribed to**  
If you subscribed to the input message topic as described in the procedure, you should see a copy of the input message every time you publish it\.

    If you don't see the message, check the topic name you subscribed to and compare it to the topic to which you published\. Topic names are case sensitive and the topic to which you subscribed must be identical to the topic to which you published the message payload\.
  + 

**Check the message publish function**  
In the MQTT client, under **Subscriptions**, choose **device/\+/data**, check the topic of the publish message, and then choose **Publish to topic**\. You should see the message payload from the edit box below the topic appear in the message list\. 
+ 

**You don't see your data in the DynamoDB table**  
The first thing to do is to manually refresh the display by choosing the refresh icon in the upper\-right corner of the table's header\. If that doesn't display the data you're looking for, check the following\.

**Things to check**
  + 

**Check the AWS Region of your MQTT client and the rule that you created**  
The console in which you're running the MQTT client must be in the same AWS Region as the rule you created\. 
  + 

**Check the input message topic in the rule query statement**  
For the rule to work, it must receive a message with the topic name that matches the topic filter in the FROM clause of the rule query statement\.

    Check the spelling of the topic filter in the rule query statement with that of the topic in the MQTT client\. Topic names are case sensitive and the message's topic must match the topic filter in the rule query statement\.
  + 

**Check the contents of the input message payload**  
For the rule to work, it must find the data field in the message payload that is declared in the SELECT statement\.

    Check the spelling of the `temperature` field in the rule query statement with that of the message payload in the MQTT client\. Field names are case sensitive and the `temperature` field in the rule query statement must be identical to the `temperature` field in the message payload\.

    Make sure that the JSON document in the message payload is correctly formatted\. If the JSON has any errors, such as a missing comma, the rule will not be able to read it\. 
  + 

**Check the key and field names used in the rule action**  
The field names used in the topic rule must match those found in the JSON message payload of the published message\.

    Open the rule you created in the console and check the field names in the rule action configuration with those used in the MQTT client\.
  + 

**Check the role being used by the rule**  
The rule action must have permission to receive the original topic and publish the new topic\. 

    The policies that authorize the rule to receive message data and update the DynamoDB table are specific to the topics used\. If you change the topic or DynamoDB table name used by the rule, you must update the rule action's role to update its policy to match\.

    If you suspect this is the problem, edit the rule action and create a new role\. New roles created by the rule action receive the authorizations necessary to perform these actions\.

## Review the results and next steps<a name="iot-ddb-rule-review"></a>

After you send a few messages to the DynamoDB table with this rule, try experimenting with it to see how changing some aspects from the tutorial affect the data written to the table\. Here are some ideas to get you started\.
+ Change the *device\_id* in the input message's topic and observe the effect on the data\. You could use this to simulate receiving data from multiple weather sensors\.
+ Change the fields selected in the rule query statement and observe the effect on the data\. You could use this to filter the data stored in the table\.
+ Add a republish rule action to send an MQTT message for each row added to the table\. You could use this for debugging\.

After you have completed this tutorial, check out the next one\!