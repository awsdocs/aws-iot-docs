# Scoping metrics in security profiles using dimensions<a name="scoping-security-behavior"></a>

Dimensions are attributes that you can define to get more precise data about metrics and behaviors in your security profile\. You define the scope by providing a value or pattern that is used as a filter\. For example, you can define a topic filter dimension that applies a metric only to MQTT topics that match a particular value, such as "data/bulb/\+/activity"\. For information about defining a dimension that you can use in your security profile, see [CreateDimension](dd-api-iot-CreateDimension.md)\.

Dimension values support MQTT wildcards\. MQTT wildcards help you subscribe to multiple topics simultaneously\. There are two different kinds of wildcards: single\-level \(`+`\) and multi\-level `(#`\)\. For example, the dimension value `Data/bulb/+/activity` creates a subscription that matches all topics that exist on the same level as the `+`\. Dimension values also support the MQTT client ID substitution variable $\{iot:ClientId\}\.

Dimensions of type TOPIC\_FILTER are compatible with the following set of cloud\-side metrics:
+ Number of messages sent
+ Number of messages received
+ Message byte size
+ Source IP address
+ Number of authorization failures

For an example of how to use the dimensions attribute, see [Monitoring legacy devices with AWS IoT Device Defender detect dimensions](https://docs.aws.amazon.com/iot/latest/developerguide/dd-dimensions-example.html)\.

## How to use dimensions in the console<a name="dimensions-console-instruc"></a>

**To create and apply a dimension to a security profile behavior**

1. In the [AWS IoT console](https://console.aws.amazon.com/iot), in the navigation pane, expand **Defend**, expand **Detect**, and then choose **Security profiles**\.

1. On the **Security profiles** page, choose **Create** to add a new security profile, or **Edit** to apply a dimension to an existing security profile\.

1. On the **Expected behaviors** page, select one of the five cloud\-side metrics dimensions supports under **Metric**\. The **Dimension** and **Dimension operator** boxes display\. For information about supported cloud\-side metrics, see [Scoping metrics in security profiles using dimensions](#scoping-security-behavior)\.

1. For **Dimension**, choose **Add dimension**\.

1. On the **Create a new dimension** page, enter details for your new dimension\. **Dimensions values** supports MQTT wildcards \# and \+ and the MQTT client ID substitution variable $\{iot:ClientId\}\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/dimensions-create-new.png)

1. Choose **Save**\.

1. You can optionally add dimensions to metrics under **Additional Metrics to retain**\.

1. To finish creating the behavior, type the information in the other required fields, and then choose **Next**\.

1. Complete the remaining steps to finish creating a security profile\.

**To view your violations**

1. In the [AWS IoT console](https://console.aws.amazon.com/iot), in the navigation pane, expand **Defend**, expand **Detect**, and then choose **Violations**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/dimensions-violations.png)

1. In the **Behavior** column, pause over the behavior you want to see the violation information for\.

**To view and update your dimensions**

1. In the [AWS IoT console](https://console.aws.amazon.com/iot), in the navigation pane, expand **Defend**, expand **Detect**, and then choose **Dimensions**\.

1. Select the dimension that you want to edit\.

1. Choose **Actions**, and then choose **Edit**\.  
![\[Image of dimensions dropdown in console.\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/dimensions-dropdown.png)

**To delete a dimension**

1. In the [AWS IoT console](https://console.aws.amazon.com/iot), in the navigation pane, expand **Defend**, expand **Detect**, and then choose **Dimensions**\.

1. Select the dimension that you want to delete\.

1. Confirm that the dimension isn't attached to a security profile by checking the **Used in** column\. If the dimension is attached to a security profile, open the **Security profiles** page on the left, and edit the security profiles that the dimension is attached to\. When you delete the dimension, you also delete the behavior\. If you want to keep the behavior, choose the ellipsis, then choose **Copy**\. Then you can proceed with deleting the behavior\. If you want to delete another dimension, follow the steps in this section\.   
![\[Image of dimensions delete page.\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/dimensions-delete.png)

1. Choose **Actions**, and then choose **Delete**\.

## How to use dimensions on the AWS CLI<a name="dimensions-cli-instruc"></a>

**To create and apply a dimension to a security profile behavior**

1. First create the dimension before attaching it to a security profile\. Use the [CreateDimension](dd-api-iot-CreateDimension.md) command to create a dimension:

   ```
   aws iot create-dimension \   
     --name TopicFilterForAuthMessages \ 
     --type TOPIC_FILTER \ 
     --string-values device/+/auth
   ```

   The output of this command looks like the following:

   ```
   {
       "arn": "arn:aws:iot:us-west-2:123456789012:dimension/TopicFilterForAuthMessages",
       "name": "TopicFilterForAuthMessages"
   }
   ```

1. Either add the dimension to an existing security profile by using [UpdateSecurityProfile](dd-api-iot-UpdateSecurityProfile.md), or add the dimension to a new security profile by using [CreateSecurityProfile](dd-api-iot-CreateSecurityProfile.md)\. In the following example, we create a new security profile that checks if messages to `TopicFilterForAuthMessages` are under 128 bytes, and retains the number of messages sent to non\-auth topics\.

   ```
   aws iot create-security-profile \
     --security-profile-name ProfileForConnectedDevice \
     --security-profile-description "Check to see if messages to TopicFilterForAuthMessages are under 128 bytes and retains the number of messages sent to non-auth topics." \ 
     --behaviors "[{\"name\":\"CellularBandwidth\",\"metric\":\"aws:message-byte-size\",\"criteria\":{\"comparisonOperator\":\"less-than\",\"value\":{\"count\":128},\"consecutiveDatapointsToAlarm\":1,\"consecutiveDatapointsToClear\":1}},{\"name\":\"Authorization\",\"metric\":\"aws:num-authorization-failures\",\"criteria\":{\"comparisonOperator\":\"less-than\",\"value\":{\"count\":10},\"durationSeconds\":300,\"consecutiveDatapointsToAlarm\":1,\"consecutiveDatapointsToClear\":1}}]" \ 
     --additional-metrics-to-retain-v2 "[{\"metric\": \"aws:num-authorization-failures\",\"metricDimension\": {\"dimensionName\": \"TopicFilterForAuthMessages\",\"operator\": \"NOT_IN\"}}]"
   ```

   The output of this command looks like the following:

   ```
   {
       "securityProfileArn": "arn:aws:iot:us-west-2:1234564789012:securityprofile/ProfileForConnectedDevice",
       "securityProfileName": "ProfileForConnectedDevice"
   }
   ```

   To save time, you can also load a parameter from a file instead of typing it as a command line parameter value\. For more information, see [Loading AWS CLI Parameters from a File](https://docs.aws.amazon.com/cli/latest/userguide/cli-usage-parameters-file.html)\. The following shows the `behavior` parameter in expanded JSON format:

   ```
   [
     {
       "criteria": {
         "comparisonOperator": "less-than",
         "consecutiveDatapointsToAlarm": 1,
         "consecutiveDatapointsToClear": 1,
         "value": {
           "count": 128
         }
       },
       "metric": "aws:message-byte-size",
       "metricDimension": {
         "dimensionName:": "TopicFilterForAuthMessages"
       },
       "name": "CellularBandwidth"
     }
   ]
   ```

**To view security profiles with a dimension**
+ Use the [ListSecurityProfiles](dd-api-iot-ListSecurityProfiles.md) command to view security profiles with a certain dimension:

  ```
  aws iot list-security-profiles \
    --dimension-name TopicFilterForAuthMessages
  ```

  The output of this command looks like the following:

  ```
  {
      "securityProfileIdentifiers": [
          {
              "name": "ProfileForConnectedDevice",
              "arn": "arn:aws:iot:us-west-2:1234564789012:securityprofile/ProfileForConnectedDevice"
          }
      ]
  }
  ```

**To update your dimension**
+ Use the [UpdateDimension](dd-api-iot-UpdateDimension.md) command to update a dimension:

  ```
  aws iot update-dimension \
    --name TopicFilterForAuthMessages \  
    --string-values device/${iot:ClientId}/auth
  ```

  The output of this command looks like the following:

  ```
  {
      "name": "TopicFilterForAuthMessages",
      "lastModifiedDate": 1585866222.317,
      "stringValues": [
          "device/${iot:ClientId}/auth"
      ],
      "creationDate": 1585854500.474,
      "type": "TOPIC_FILTER",
      "arn": "arn:aws:iot:us-west-2:1234564789012:dimension/TopicFilterForAuthMessages"
  }
  ```

**To delete a dimension**

1. To delete a dimension, first detach it from any security profiles that it's attached to\. Use the [ListSecurityProfiles](dd-api-iot-ListSecurityProfiles.md) command to view security profiles with a certain dimension\.

1. To remove a dimension from a security profile, use the [UpdateSecurityProfile](dd-api-iot-UpdateSecurityProfile.md) command\. Enter all information that you want to keep, but exclude the dimension:

   ```
   aws iot update-security-profile \
     --security-profile-name ProfileForConnectedDevice \
     --security-profile-description "Check to see if authorization fails 10 times in 5 minutes or if cellular bandwidth exceeds 128" \
     --behaviors "[{\"name\":\"metric\":\"aws:message-byte-size\",\"criteria\":{\"comparisonOperator\":\"less-than\",\"value\":{\"count\":128},\"consecutiveDatapointsToAlarm\":1,\"consecutiveDatapointsToClear\":1}},{\"name\":\"Authorization\",\"metric\":\"aws:num-authorization-failures\",\"criteria\":{\comparisonOperator\":\"less-than\",\"value\"{\"count\":10},\"durationSeconds\":300,\"consecutiveDatapointsToAlarm\":1,\"consecutiveDatapointsToClear\":1}}]"
   ```

   The output of this command looks like the following:

   ```
   {
     "behaviors": [
       {
         "metric": "aws:message-byte-size",
         "name": "CellularBandwidth",
         "criteria": {
           "consecutiveDatapointsToClear": 1,
           "comparisonOperator": "less-than",
           "consecutiveDatapointsToAlarm": 1,
           "value": {
             "count": 128
           }
         }
       },
       {
         "metric": "aws:num-authorization-failures",
         "name": "Authorization",
         "criteria": {
           "durationSeconds": 300,
           "comparisonOperator": "less-than",
           "consecutiveDatapointsToClear": 1,
           "consecutiveDatapointsToAlarm": 1,
           "value": {
             "count": 10
           }
         }
       }
     ],
     "securityProfileName": "ProfileForConnectedDevice",
     "lastModifiedDate": 1585936349.12,
     "securityProfileDescription": "Check to see if authorization fails 10 times in 5 minutes or if cellular bandwidth exceeds 128",
     "version": 2,
     "securityProfileArn": "arn:aws:iot:us-west-2:123456789012:securityprofile/Preo/ProfileForConnectedDevice",  
     "creationDate": 1585846909.127
   }
   ```

1. After the dimension is detached, use the [DeleteDimension](dd-api-iot-DeleteDimension.md) command to delete the dimension:

   ```
   aws iot delete-dimension \
     --name TopicFilterForAuthMessages
   ```