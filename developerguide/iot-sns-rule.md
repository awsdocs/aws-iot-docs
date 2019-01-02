# Creating an Amazon SNS Rule<a name="iot-sns-rule"></a>

You can define a rule that sends message data to an Amazon SNS topic\. 

In this tutorial, you create a rule that sends the name of the AWS IoT thing that triggered the rule to all subscribers of an Amazon SNS topic\.

**To create a rule with an SNS action**

1. In the [AWS IoT console](https://console.aws.amazon.com/iot/home), in the left navigation pane, choose **Act**\.  
![\[Image NOT FOUND\]](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/images/choose-rules.png)

1. On the **Rules** page, choose **Create**\.  
![\[Image NOT FOUND\]](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/images/dashboard-rules.png)

1. Enter a name for your rule\.  
![\[Image NOT FOUND\]](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/images/sns-create-rule.png)

1. In the **Rule query statement** editor, enter

   ```
   SELECT *, topic(3) as thing FROM '$aws/things/+/shadow/update/accepted'
   ```

   \(The topic filter following the `"FROM"` specifies the topics that, when a message is published to them, trigger the rule's action\. The plus sign \(`+`\) used in the topic filter is a wildcard character that matches any thing name\. The `"topic(3)"` attribute following `"SELECT"` appends the thing name, which is the third topic field, onto the message contents\.\)  
![\[Image NOT FOUND\]](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/images/sns-message-source.png)

1. In the **Set one or more actions** section, choose **Add action**\.  
![\[Image NOT FOUND\]](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/images/sns-add-action.png)

1. Under **Select an action**, choose **Send a message as an SNS push notification**, and then choose **Configure action**\.  
![\[Image NOT FOUND\]](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/images/select-sns-action.png)  
![\[Image NOT FOUND\]](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/images/choose-configure-action.png)

1. On the **Configure action** page, for **SNS target**, choose **Create**\.  
![\[Image NOT FOUND\]](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/images/sns-add-topic.png)

1. A new tab opens in your browser\. Enter a topic name, and then choose **Create**\.  
![\[Image NOT FOUND\]](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/images/sns-name-topic.png)

1. On the **Configure action** page, for **SNS target**, choose the SNS topic you just created\. For **Message format**, choose **JSON**\. For **IAM role name**, choose **Create a new role**\.  
![\[Image NOT FOUND\]](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/images/sns-configure-action-1.png)

1. Enter a name for the role, and then choose **Create a new role**\.  
![\[Image NOT FOUND\]](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/images/sns-configure-action-3.png)

1. Select the role you just created, and then choose **Add action**\.  
![\[Image NOT FOUND\]](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/images/sns-configure-action-4.png)

1. Choose **Create rule**\.  
![\[Image NOT FOUND\]](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/images/create-rule-for-ddb-final.png)

To test the rule you just created, add a subscription to the SNS topic you created, and update the shadow of any AWS IoT thing\. 

You can use the AWS IoT console to find a thing, open its detail page, and change the device's shadow\. When the Device Shadow service is notified of the change, it publishes a message on `$aws/things/MySNSThing/shadow/update/accepted`\. Your rule is triggered and all subscribers to your SNS topic receive a message that contains your thing's name\. 