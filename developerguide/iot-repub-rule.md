# Tutorial: Republishing an MQTT message<a name="iot-repub-rule"></a>

This tutorial demonstrates how to create an AWS IoT rule that publishes an MQTT message when a specified MQTT message is received\. The incoming message payload can be modified by the rule before it's published\. This makes it possible to create messages that are tailored to specific applications without the need to alter your device or its firmware\. You can also use the filtering aspect of a rule to publish messages only when a specific condition is met\.

The messages republished by a rule act like messages sent by any other AWS IoT device or client\. Devices can subscribe to the republished messages the same way they can subscribe to any other MQTT message topic\.

**What you'll learn in this tutorial:**
+ How to use simple SQL queries and functions in a rule query statement
+ How to use the MQTT client to test an AWS IoT rule

This tutorial takes about 30 minutes to complete\.

**Topics**
+ [Review MQTT topics and AWS IoT rules](#iot-repub-rule-mqtt)
+ [Step 1: Create an AWS IoT rule to republish an MQTT message](#iot-repub-rule-define)
+ [Step 2: Test your new rule](#iot-repub-rule-test)
+ [Step 3: Review the results and next steps](#iot-repub-rule-review)

**Before you start this tutorial, make sure that you have:**
+ 

**[Set up your AWS account](setting-up.md)**  
You'll need your AWS account and AWS IoT console to complete this tutorial\.
+ 

**Reviewed [View MQTT messages with the AWS IoT MQTT client](view-mqtt-messages.md)**  
Be sure you can use the MQTT client to subscribe and publish to a topic\. You'll use the MQTT client to test your new rule in this procedure\.

## Review MQTT topics and AWS IoT rules<a name="iot-repub-rule-mqtt"></a>

Before talking about AWS IoT rules, it helps to understand the MQTT protocol\. In IoT solutions, the MQTT protocol offers some advantages over other network communication protocols, such as HTTP, which makes it a popular choice for use by IoT devices\. This section reviews the key aspects of MQTT as they apply to this tutorial\. For information about how MQTT compares to HTTP, see [Choosing a protocol for your device communicationConnection duration limits](protocols.md#protocol-selection)\.

**MQTT protocol**  
The MQTT protocol uses a publish/subscribe communication model with its host\. To send data, devices publish messages that are identified by topics to the AWS IoT message broker\. To receive messages from the message broker, devices subscribe to the topics they will receive by sending topic filters in subscription requests to the message broker\. The AWS IoT rules engine receives MQTT messages from the message broker\.

**AWS IoT rules**  
AWS IoT rules consist of a rule query statement and one or more rule actions\. When the AWS IoT rules engine receives an MQTT message, these elements act on the message as follows\.
+ 

**Rule query statement**  
The rule's query statement describes the MQTT topics to use, interprets the data from the message payload, and formats the data as described by a SQL statement that is similar to statements used by popular SQL databases\. The result of the query statement is the data that is sent to the rule's actions\.
+ 

**Rule action**  
Each rule action in a rule acts on the data that results from the rule's query statement\. AWS IoT supports [many rule actions](iot-rule-actions.md)\. In this tutorial, however, you'll concentrate on the [Republish](republish-rule-action.md) rule action, which publishes the result of the query statement as an MQTT message with a specific topic\.

## Step 1: Create an AWS IoT rule to republish an MQTT message<a name="iot-repub-rule-define"></a>

The AWS IoT rule that you'll create in this tutorial subscribes to the `device/device_id/data` MQTT topics where *device\_id* is the ID of the device that sent the message\. These topics are described by a [topic filter](topics.md#topicfilters) as `device/+/data`, where the `+` is a wildcard character that matches any string between the two forward slash characters\.

When the rule receives a message from a matching topic, it republishes the `device_id` and `temperature` values as a new MQTT message with the `device/data/temp` topic\. 

For example, the payload of an MQTT message with the `device/22/data` topic looks like this:

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

The rule takes the `temperature` value from the message payload, and the `device_id` from the topic, and republishes them as an MQTT message with the `device/data/temp` topic and a message payload that looks like this:

```
{
  "device_id": "22",
  "temperature": 28
}
```

With this rule, devices that only need the device's ID and the temperature data subscribe to the `device/data/temp` topic to receive only that information\.

**To create a rule that republishes an MQTT message**

1. Open [the **Rules** hub of the AWS IoT console](https://console.aws.amazon.com/iot/home#/rulehub)\.

1. In **Rules**, choose **Create** and start creating your new rule\.

1. In the top part of **Create a rule**:

   1. In **Name**, enter the rule's name\. For this tutorial, name it **republish\_temp**\.

      Remember that a rule name must be unique within your Account and Region, and it can't have any spaces\. We've used an underscore character in this name to separate the two words in the rule's name\.

   1.  In **Description**, describe the rule\. 

      A meaningful description helps you remember what this rule does and why you created it\. The description can be as long as needed, so be as detailed as possible\. 

1. In **Rule query statement** of **Create a rule**:

   1.  In **Using SQL version**, select **2016\-03\-23**\. 

   1. In the **Rule query statement** edit box, enter the statement: 

      ```
      SELECT topic(2) as device_id, temperature FROM 'device/+/data'
      ```

      This statement:
      + Listens for MQTT messages with a topic that matches the `device/+/data` topic filter\.
      + Selects the second element from the topic string and assigns it to the `device_id` field\.
      + Selects the value `temperature` field from the message payload and assigns it to the `temperature` field\.

1. In **Set one or more actions**:

   1. To open up the list of rule actions for this rule, choose **Add action**\.

   1. In **Select an action**, choose **Republish a message to an AWS IoT topic**\.

   1. At the bottom of the action list, choose **Configure action** to open the selected action's configuration page\.

1. In **Configure action**:

   1.  In **Topic**, enter **device/data/temp**\. This is the MQTT topic of the message that this rule will publish\. 

   1.  In **Quality of Service**, choose **0 \- The message is delivered zero or more times**\. 

   1.  In **Choose or create a role to grant AWS IoT access to perform this action**:

      1.  Choose **Create Role**\. The **Create a new role** dialog box opens\. 

      1. Enter a name that describes the new role\. In this tutorial, use **republish\_role**\. 

         When you create a new role, the correct policies to perform the rule action are created and attached to the new role\. If you change the topic of this rule action or use this role in another rule action, you must update the policy for that role to authorize the new topic or action\. To update an existing role, choose **Update role** in this section\.

      1. Choose **Create Role** to create the role and close the dialog box\. 

   1. Choose **Add action** to add the action to the rule and return to the **Create a rule** page\. 

1. The **Republish a message to an AWS IoT topic** action is now listed in **Set one or more actions**\.

   In the new action's tile, below **Republish a message to an AWS IoT topic**, you can see the topic to which your republish action will publish\.

   This is the only rule action you'll add to this rule\.

1. In **Create a rule**, scroll down to the bottom and choose **Create rule** to create the rule and complete this step\.

## Step 2: Test your new rule<a name="iot-repub-rule-test"></a>

To test your new rule, you'll use the MQTT client to publish and subscribe to the MQTT messages used by this rule\.

Open the [MQTT client in the AWS IoT console](https://console.aws.amazon.com/iot/home#/test) in a new window\. This will let you edit the rule without losing the configuration of your MQTT client\. The MQTT client does not retain any subscriptions or message logs if you leave it to go to another page in the console\.

**To use the MQTT client to test your rule**

1. In the [MQTT client in the AWS IoT console](https://console.aws.amazon.com/iot/home#/test), subscribe to the input topics, in this case, `device/+/data`\.

   1. In the MQTT client, under **Subscriptions**, choose **Subscribe to a topic**\.

   1. In **Subscription topic**, enter the topic of the input topic filter, **device/\+/data**\.

   1. Keep the rest of the fields at their default settings\.

   1. Choose **Subscribe to topic**\.

      In the **Subscriptions** column, under **Publish to a topic**, **device/\+/data** appears\. 

1. Subscribe to the topic that your rule will publish: `device/data/temp`\.

   1. Under **Subscriptions**, choose **Subscribe to a topic** again, and in **Subscription topic**, enter the topic of the republished message, **device/data/temp**\.

   1. Keep the rest of the fields at their default settings\.

   1. Choose **Subscribe to topic**\.

      In the **Subscriptions** column, under **device/\+/data**, **device/data/temp** appears\. 

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

   1. To send your MQTT message, choose **Publish to topic**\.

1. Review the messages that were sent\.

   1. In the MQTT client, under **Subscriptions**, there is a green dot next to the two topics to which you subscribed earlier\.

      The green dots indicate that one or more new messages have been received since the last time you looked at them\.

   1. Under **Subscriptions**, choose **device/\+/data** to check that the message payload matches what you just published and looks like this:

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

   1. Under **Subscriptions**, choose **device/data/temp** to check that your republished message payload looks like this:

      ```
      {
        "device_id": "22",  
        "temperature": 28
      }
      ```

      Notice that the `device_id` value is a quoted string and the `temperature` value is numeric\. This is because the `topic()` function extracted the string from the input message's topic name while the `temperature` value uses the numeric value from the input message's payload\.

      If you want to make the `device_id` value a numeric value, replace `topic(2)` in the rule query statement with:

      ```
      cast(topic(2) AS DECIMAL)
      ```

      Note that casting the `topic(2)` value to a numeric value will only work if that part of the topic contains only numeric characters\.

1. If you see that the correct message was published to the **device/data/temp** topic, then your rule worked\. See what more you can learn about the Republish rule action in the next section\.

   If you don't see that the correct message was published to either the **device/\+/data** or **device/data/temp** topics, check the troubleshooting tips\.

### Troubleshooting your Republish message rule<a name="iot-repub-rule-trouble"></a>

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

**You don't see your republished message in the MQTT client**  
For your rule to work, it must have the correct policy that authorizes it to receive and republish a message and it must receive the message\.

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

**Check the republished message topic in the rule action**  
The topic to which the Republish rule action publishes the new message must match the topic to which you subscribed in the MQTT client\.

    Open the rule you created in the console and check the topic to which the rule action will republish the message\.
  + 

**Check the role being used by the rule**  
The rule action must have permission to receive the original topic and publish the new topic\. 

    The policies that authorize the rule to receive message data and republish it are specific to the topics used\. If you change the topic used to republish the message data, you must update the rule action's role to update its policy to match the current topic\.

    If you suspect this is the problem, edit the Republish rule action and create a new role\. New roles created by the rule action receive the authorizations necessary to perform these actions\.

## Step 3: Review the results and next steps<a name="iot-repub-rule-review"></a>

**In this tutorial**
+ You used a simple SQL query and a couple of functions in a rule query statement to produce a new MQTT message\.
+ You created a rule that republished that new message\.
+ You used the MQTT client to test your AWS IoT rule\.

**Next steps**  
After you republish a few messages with this rule, try experimenting with it to see how changing some aspects of the tutorial affect the republished message\. Here are some ideas to get you started\.
+ Change the *device\_id* in the input message's topic and observe the effect in the republished message payload\.
+ Change the fields selected in the rule query statement and observe the effect in the republished message payload\.
+ Try the next tutorial in this series and learn how to [Tutorial: Sending an Amazon SNS notification](iot-sns-rule.md)\.

The Republish rule action used in this tutorial can also help you debug rule query statements\. For example, you can add this action to a rule to see how its rule query statement is formatting the data used by its rule actions\.