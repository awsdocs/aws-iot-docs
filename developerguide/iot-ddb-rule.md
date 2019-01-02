# Creating an Amazon DynamoDB Rule<a name="iot-ddb-rule"></a>

DynamoDB rules allow you to take information from an incoming MQTT message and write it to a DynamoDB table\. 

**To create a DynamoDB rule**

1. In the [AWS IoT console](https://console.aws.amazon.com/iot/home), in the left navigation pane, choose **Act**\.  
![\[Image NOT FOUND\]](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/images/choose-rules.png)

1. On the **Rules** page, choose **Create**\.  
![\[Image NOT FOUND\]](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/images/dashboard-rules.png)

1. On the **Create a rule** page, enter a **Name** for your rule, and a **Description** for the rule\.  
![\[Image NOT FOUND\]](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/images/create-a-ddb-rule.png)

1. Scroll down to **Rule query statement**\. Choose the latest version from the **Using SQL version** list\. For **Rule query statement**, enter: 

   ```
   SELECT * FROM 'iotbutton/your-button-DSN'
   ```

   \(`"SELECT *"` specifies that you want to send the entire MQTT message that triggered the rule\. `"FROM 'iotbutton...'"` tells the rules engine to trigger this rule when an MQTT message is received whose topic matches this topic filter\.\) If you're not using an AWS IoT button, enter **my/topic** or the topic you use to send a test message\.
**Note**  
You can find the DSN on the bottom of the button\.  
![\[Image NOT FOUND\]](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/images/rule-query-ddb.png)

1. In **Set one or more actions**, choose **Add action**\.   
![\[Image NOT FOUND\]](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/images/rule-add-action.png)

1. On the **Select an action** page, choose **Insert a message into a DynamoDB table**, and then choose **Configure action**\.  
![\[Image NOT FOUND\]](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/images/select-an-action-ddb.png)  
![\[Image NOT FOUND\]](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/images/choose-configure-action.png)

1. On the **Configure action** page, choose **Create a new resource**\.  
![\[Image NOT FOUND\]](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/images/configure-action-ddb-entry.png)

1. On the **Amazon DynamoDB** page, choose **Create table**\.  
![\[Image NOT FOUND\]](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/images/dynamodb-welcome.png)

1. On the **Create DynamoDB table** page, enter a name in **Table name**\. In **Partition key**, enter **SerialNumber**\. Select **Add sort key**, and then enter **ClickType** in the **Sort key** field\. Choose **String** for both the partition and sort keys\.  
![\[Image NOT FOUND\]](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/images/create-ddb-table.png)

1. Choose **Create**\. It takes a few seconds to create your DynamoDB table\. Close the browser tab where the Amazon DynamoDB console is open\. If you don't close the tab, your DynamoDB table will not be displayed in the **Table name** list on the AWS IoT **Configure action** page\.

1. On the **Configure action** page, choose your new table from the **Table name** list\. In **Hash key value**, enter **$\{serialNumber\}**\. This instructs the rule to take the value of the `serialNumber` attribute from the MQTT message and write it into the **SerialNumber** column in the DynamoDB table\. In **Range key value**, enter **$\{clickType\}**\. This writes the value of the `clickType` attribute into the **ClickType** column\. Leave **Write message data to this column** blank\. By default, the entire message is written to a column in the table named Payload\. Choose **Create a new role**\.  
![\[Image NOT FOUND\]](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/images/configure-action-with-resource.png)  
![\[Image NOT FOUND\]](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/images/create-new-role-ddb.png)

1. Type a unique name in **IAM role name**, and then choose **Create a new role** again\. Choose the role you just created, choose **Update role**, and then choose **Add action**\.  
![\[Image NOT FOUND\]](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/images/create-new-role-ddb-2.png)

1. Choose **Create rule**\.

   A confirmation message shows the rule has been created, and you return to the **Rules** page\.  
![\[Image NOT FOUND\]](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/images/create-rule-for-ddb-final.png)

1. Test the rule by pressing your configured AWS IoT button or by using an MQTT client to publish a message on a topic that matches your rule's topic filter\. 

   Finally, return to the DynamoDB console and select the table you created to view the entry for your button press or message\.  
![\[Image NOT FOUND\]](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/images/ddb-table-example.png)