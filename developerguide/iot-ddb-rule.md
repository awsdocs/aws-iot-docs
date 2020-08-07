# Creating a rule with a DynamoDB action<a name="iot-ddb-rule"></a>

The DynamoDB action allows you to take information from an incoming MQTT message and write it to a DynamoDB table\. 

**To create a DynamoDB rule**

1. In the [AWS IoT console](https://console.aws.amazon.com/iot/home), in the navigation pane, choose **Act**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/choose-rules.png)

1. On the **Rules** page, choose **Create**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/dashboard-rules.png)

1. On the **Create a rule** page, enter a name and description for your rule\.
**Note**  
We do not recommend the use of personally identifiable information in rule names or descriptions\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/create-a-ddb-rule.png)

1. Under **Rule query statement**, choose the latest version from the **Using SQL version** list\. For **Rule query statement**, enter: 

   ```
   SELECT * FROM 'my/greenhouse'
   ```

   \(`"SELECT *"` specifies that you want to send the entire MQTT message that initiated the rule\. `"FROM 'my/greenhouse'"` tells the rules engine to initiate this rule when an MQTT message whose topic matches this topic filter is received\. Choose **Add action\.**  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/rule-query-ddb.png)

1. On **Select an action**, choose **Insert a message into a DynamoDB table**, and then choose **Configure action**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/set-an-action.png)

1. On **Configure action**, choose **Create a new resource**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/configure-action-ddb-entry.png)

1. On the **Amazon DynamoDB** page, choose **Create table**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/dynamodb-welcome.png)

1. On **Create DynamoDB table**, enter a name\. In **Partition key**, enter **Row**\. Select **Add sort key**, and then enter **PositionInRow** in the **Sort key** field\. `Row` represents a row of plants in a greenhouse\. `PositionInRow` represents the position of a plant in the row\. Choose **String** for both the partition and sort keys, and then choose **Create**\. It takes a few seconds to create your DynamoDB table\. Close the browser tab where the Amazon DynamoDB console is open\. If you don't close the tab, your DynamoDB table is not displayed in the **Table name** list on the **Configure action** page of the AWS IoT console\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/create-ddb-table.png)

1. On **Configure action**, choose your new table from the **Table name** list\. In **Partition key value**, enter **$\{row\}**\. This instructs the rule to take the value of the `row` attribute from the MQTT message and write it into the **Row** column in the DynamoDB table\. In **Sort key value**, enter **$\{pos\}**\. This writes the value of the `pos` attribute into the **PositionInRow** column\. In **Write message data to this column**, enter **Payload**\. This inserts the message payload into the `Payload` column\. Leave **Operation** blank\. This field allows you to specify which operation \(INSERT, UPDATE, or DELETE\) you want to perform when the action is initiated\. Choose **Create a new role**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/configure-action-with-resource.png)

1. In **Create a new role**, enter a unique name, and then choose **Create role**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/create-new-role-ddb-2.png)

1. Choose **Add action**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/create-rule-for-ddb-final.png)

1. Choose **Create rule**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/create-rule-for-ddb-final-2.png)

## Testing a rule with a DynamoDB action<a name="test-db-rule"></a>

1. To test the rule, open the AWS IoT console and from the navigation pane, choose **Test**\.

1. Choose **Publish to a topic**\. In the **Publish** section, enter **my/greenhouse**\. In the message area, enter the following JSON:

   ```
   {
       "row" : "0",
       "pos" : "0",
       "moisture" : "75"
     }
   ```  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/ddb-message.png)

   Return to the DynamoDB console and choose **Tables**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/ddb-tables.png)

   Select the **GreenhouseTable**, and then choose **Items**\. Your data is displayed on the **Items** tab\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/ddb-table-data.png)