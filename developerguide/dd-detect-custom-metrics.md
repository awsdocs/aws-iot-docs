# Custom metrics<a name="dd-detect-custom-metrics"></a>

With AWS IoT Device Defender custom metrics, you can define and monitor metrics that are unique to your fleet or use case, such as number of devices connected to Wi\-Fi gateways, charge levels for batteries, or number of power cycles for smart plugs\. Custom metric behaviors are defined in Security Profiles, which specify expected behaviors for a group of devices \(a thing group\) or for all devices\. You can monitor behaviors by setting up alarms, which you can use to detect and respond to issues that are specific to the devices\.

**Topics**
+ [How to use custom metrics in the console](#dd-detect-custom-metrics-how-to-console)
+ [How to use custom metrics from the CLI](#dd-detect-custom-metrics-how-to-cli)
+ [Custom metrics CLI commands](#dd-detect-custom-metrics-cli-commands)
+ [Custom metrics APIs](#dd-detect-custom-metrics-apis)

## How to use custom metrics in the console<a name="dd-detect-custom-metrics-how-to-console"></a>

**Topics**
+ [AWS IoT Device Defender Agent SDK \(Python\)](#dd-detect-custom-metrics-device-agent)
+ [Create a custom metric and add it to a Security Profile](#dd-detect-console-create)
+ [View custom metric details](#dd-detect-console-read)
+ [Update a custom metric](#dd-detect-console-edit)
+ [Delete a custom metric](#dd-detect-console-delete)

### AWS IoT Device Defender Agent SDK \(Python\)<a name="dd-detect-custom-metrics-device-agent"></a>

To get started, download the AWS IoT Device Defender Agent SDK \(Python\) sample agent\. The agent gathers the metrics and publishes reports\. Once your device\-side metrics are publishing, you can view the metrics being collected and determine thresholds for setting up alarms\. Instructions for setting up the device agent are available on the [AWS IoT Device Defender Agent SDK \(Python\) Readme](https://github.com/aws-samples/aws-iot-device-defender-agent-sdk-python/blob/master/README.rst)\. For more information, see [AWS IoT Device Defender Agent SDK \(Python\)](https://github.com/aws-samples/aws-iot-device-defender-agent-sdk-python)\.

### Create a custom metric and add it to a Security Profile<a name="dd-detect-console-create"></a>

The following procedure shows you how to create a custom metric in the console\.

1. In the [AWS IoT console](https://console.aws.amazon.com/iot), in the navigation pane, expand **Defend**, and then choose **Detect**, **Metrics**\.

1. On the **Custom metrics** page, choose **Create**\.

1. On the **Create custom metric** page, do the following\.

   1. Under **Name**, enter a name for your custom metric\. You can't modify this name after you create the custom metric\.

   1. Under **Display name \(optional\)**, you can enter a friendly name for your custom metric\. It doesn't have to be unique and it can be modified after creation\.

   1. Under **Type**, choose the type of metric you'd like to monitor\. Metric types include **string\-list**, **ip\-address\-list**, **number\-list**, and **number**\. The type can't be modified after creation\.
**Note**  
ML Detect only allows the **number** type\.

   1. Under **Tags**, you can select tags to be associated with the resource\.

   When you're done, choose **Confirm**\.

1. After you've created your custom metric, the **Custom metrics** page appears, where you can see your newly created custom metric\.

1. Next, you need to add your custom metric to a Security Profile\. In the [AWS IoT console](https://console.aws.amazon.com/iot), in the navigation pane, expand **Defend**, and then choose **Detect**, **Security profiles**\.

1. Choose the Security Profile you'd like to add your custom metric to\.

1. Choose **Actions**, **Edit**\.

1. Choose **Additional Metrics to retain**, and then choose your custom metric\. Choose **Next** on the following screens until you reach the **Confirm** page\. Choose **Save** and **Continue**\. After your custom metric has been successfully added, the Security Profile details page appears\.
**Note**  
Percentile statistics are not available for metrics when any of the metric values are negative numbers\.

### View custom metric details<a name="dd-detect-console-read"></a>

The following procedure shows you how to view a custom metric's details in the console\.

1. In the [AWS IoT console](https://console.aws.amazon.com/iot), in the navigation pane, expand **Defend**, and then choose **Detect**, **Metrics**\.

1. Choose the **Metric name** of the custom metric you'd like to view the details of\.

### Update a custom metric<a name="dd-detect-console-edit"></a>

The following procedure shows you how to update a custom metric in the console\.

1. In the [AWS IoT console](https://console.aws.amazon.com/iot), in the navigation pane, expand **Defend**, and then choose **Detect**, **Metrics**\.

1. Choose the option button next to the custom metric you'd like to update\. Then, for **Actions**, choose **Edit**\.

1. On the **Update custom metric** page, you can edit the display name and remove or add tags\.

1. After you're done, choose **Update**\. The **Custom metrics** page\.

### Delete a custom metric<a name="dd-detect-console-delete"></a>

The following procedure shows you how to delete a custom metric in the console\.

1. First, remove your custom metric from any Security Profile it's referenced in\. You can view which Security Profiles contain your custom metric on your custom metric details page\. In the [AWS IoT console](https://console.aws.amazon.com/iot), in the navigation pane, expand **Defend**, and then choose **Detect**, **Metrics**\.

1. Choose the custom metric you'd like to remove\. Remove the custom metric from any Security Profile listed under **Security Profiles** on the custom metric details page\.

1. In the [AWS IoT console](https://console.aws.amazon.com/iot), in the navigation pane, expand **Defend**, and then choose **Detect**, **Metrics**\.

1. Choose the option button next to the custom metric you'd like to delete\. Then, for **Actions**, choose **Delete**\.

1. On the **Are you sure you want to delete custom metric?** message, choose **Delete custom metric**\.
**Warning**  
After you've deleted a custom metric, you lose all data associated with the metric\. This action can't be undone\.

## How to use custom metrics from the CLI<a name="dd-detect-custom-metrics-how-to-cli"></a>

**Topics**
+ [AWS IoT Device Defender Agent SDK \(Python\)](#dd-detec-custom-metrics-cli-sdk)
+ [Create a custom metric and add it to a Security Profile](#dd-detect-custom-cli-create)
+ [View custom metric details](#dd-detect-custom-cli-read)
+ [Update a custom metric](#dd-detect-custom-cli-edit)
+ [Delete a custom metric](#dd-detect-custom-cli-delete)

### AWS IoT Device Defender Agent SDK \(Python\)<a name="dd-detec-custom-metrics-cli-sdk"></a>

To get started, download the AWS IoT Device Defender Agent SDK \(Python\) sample agent\. The agent gathers the metrics and publishes reports\. After your device\-side metrics are publishing, you can view the metrics being collected and determine thresholds for setting up alarms\. Instructions for setting up the device agent are available on the [AWS IoT Device Defender Agent SDK \(Python\) Readme](https://github.com/aws-samples/aws-iot-device-defender-agent-sdk-python/blob/master/README.rst)\. For more information, see [AWS IoT Device Defender Agent SDK \(Python\)](https://github.com/aws-samples/aws-iot-device-defender-agent-sdk-python)\.

### Create a custom metric and add it to a Security Profile<a name="dd-detect-custom-cli-create"></a>

The following procedure shows you how to create a custom metric and add it to a Security Profile from the CLI\.

1. Use the `[create\-custom\-metric](https://docs.aws.amazon.com/cli/latest/reference/iot/create-custom-metric.html)` command to create your custom metric\. The following example creates a custom metric that measures battery percentage\.

   ```
   aws iot create-custom-metric \
       --metric-name "batteryPercentage" \
       --metric-type "number" \
       --display-name "Remaining battery percentage." \
       --region us-east-1
       --client-request-token "02ccb92b-33e8-4dfa-a0c1-35b181ed26b0" \
   ```

   Output:

   ```
   {
       "metricName": "batteryPercentage",
       "metricArn": "arn:aws:iot:us-east-1:1234564789012:custommetric/batteryPercentage"
   }
   ```

1. After you've created your custom metric, you can either add the custom metric to an existing profile using `[update\-security\-profile](https://docs.aws.amazon.com/cli/latest/reference/iot/update-security-profile.html)` or create a new security profile to add the custom metric to using `[create\-security\-profile](https://docs.aws.amazon.com/cli/latest/reference/iot/create-security-profile.html)`\. Here, we create a new security profile called *batteryUsage* to add our new *batteryPercentage* custom metric to\. We also add a Rules Detect metric called *cellularBandwidth*\.

   ```
   aws iot create-security-profile \
       --security-profile-name batteryUsage \
       --security-profile-description "Shows how much battery is left in percentile."  \
       --behaviors "[{\"name\":\"great-than-75\",\"metric\":\"batteryPercentage\",\"criteria\":{\"comparisonOperator\":\"greater-than\",\"value\":{\"number\":75},\"consecutiveDatapointsToAlarm\":5,\"consecutiveDatapointsToClear\":1}},{\"name\":\"cellularBandwidth\",\"metric\":\"aws:message-byte-size\",\"criteria\":{\"comparisonOperator\":\"less-than\",\"value\":{\"count\":128},\"consecutiveDatapointsToAlarm\":1,\"consecutiveDatapointsToClear\":1}}]" \
       --region us-east-1
   ```

   Output:

   ```
   {
       "securityProfileArn": "arn:aws:iot:us-east-1:1234564789012:securityprofile/batteryUsage",
       "securityProfileName": "batteryUsage"
   }
   ```

**Note**  
Percentile statistics are not available for metrics when any of the metric values are negative numbers\.

### View custom metric details<a name="dd-detect-custom-cli-read"></a>

The following procedure shows you how to view the details for a custom metric from the CLI\.
+ Use the `[list\-custom\-metrics](https://docs.aws.amazon.com/cli/latest/reference/iot/list-custom-metrics.html)` command to view all of your custom metrics\.

  ```
  aws iot list-custom-metrics \
      --region us-east-1
  ```

  The output of this command looks like the following\.

  ```
  {
      "metricNames": [
          "batteryPercentage"
      ]
  }
  ```

### Update a custom metric<a name="dd-detect-custom-cli-edit"></a>

The following procedure shows you how to update a custom metric from the CLI\.
+ Use the `[update\-custom\-metric](https://docs.aws.amazon.com/cli/latest/reference/iot/update-custom-metric.html)` command to update a custom metric\. The following example updates the `display-name`\.

  ```
  aws iot update-custom-metric \
      --metric-name batteryPercentage \
      --display-name 'remaining battery percentage on device' \
      --region us-east-1
  ```

  The output of this command looks like the following\.

  ```
  {
      "metricName": "batteryPercentage",
      "metricArn": "arn:aws:iot:us-east-1:1234564789012:custommetric/batteryPercentage",
      "metricType": "number",
      "displayName": "remaining battery percentage on device",
      "creationDate": "2020-11-17T23:01:35.110000-08:00",
      "lastModifiedDate": "2020-11-17T23:02:12.879000-08:00"
  }
  ```

### Delete a custom metric<a name="dd-detect-custom-cli-delete"></a>

The following procedure shows you how to delete a custom metric from the CLI\.

1. To delete a custom metric, first remove it from any Security Profiles that it's attached to\. Use the `[list\-security\-profiles](https://docs.aws.amazon.com/cli/latest/reference/iot/list-security-profiles.html)` command to view Security Profiles with a certain custom metric\.

1. To remove a custom metric from a Security Profile, use the `[update\-security\-profiles](https://docs.aws.amazon.com/cli/latest/reference/iot/update-security-profiles.html)` command\. Enter all information that you want to keep, but exclude the custom metric\.

   ```
   aws iot update-security-profile \
     --security-profile-name batteryUsage \
     --behaviors "[{\"name\":\"cellularBandwidth\",\"metric\":\"aws:message-byte-size\",\"criteria\":{\"comparisonOperator\":\"less-than\",\"value\":{\"count\":128},\"consecutiveDatapointsToAlarm\":1,\"consecutiveDatapointsToClear\":1}}]"
   ```

   The output of this command looks like the following\.

   ```
   {
     "behaviors": [{\"name\":\"cellularBandwidth\",\"metric\":\"aws:message-byte-size\",\"criteria\":{\"comparisonOperator\":\"less-than\",\"value\":{\"count\":128},\"consecutiveDatapointsToAlarm\":1,\"consecutiveDatapointsToClear\":1}}],
     "securityProfileName": "batteryUsage",
     "lastModifiedDate": 2020-11-17T23:02:12.879000-09:00,
     "securityProfileDescription": "Shows how much battery is left in percentile.",
     "version": 2,
     "securityProfileArn": "arn:aws:iot:us-east-1:1234564789012:securityprofile/batteryUsage",  
     "creationDate": 2020-11-17T23:02:12.879000-09:00
   }
   ```

1. After the custom metric is detached, use the `[delete\-custom\-metric](https://docs.aws.amazon.com/cli/latest/reference/iot/delete-custom-metric.html)` command to delete the custom metric\.

   ```
   aws iot delete-custom-metric  \
     --metric-name batteryPercentage \
     --region us-east-1
   ```

   The output of this command looks like the following

   ```
   HTTP 200
   ```

## Custom metrics CLI commands<a name="dd-detect-custom-metrics-cli-commands"></a>

You can use the following CLI commands to create and manage custom metrics\.
+ [create\-custom\-metric](https://docs.aws.amazon.com/cli/latest/reference/iot/create-custom-metric.html)
+ [describe\-custom\-metric](https://docs.aws.amazon.com/cli/latest/reference/iot/describe-custom-metric.html)
+ [list\-custom\-metrics](https://docs.aws.amazon.com/cli/latest/reference/iot/list-custom-metrics.html)
+ [update\-custom\-metric](https://docs.aws.amazon.com/cli/latest/reference/iot/update-custom-metric.html)
+ [delete\-custom\-metric](https://docs.aws.amazon.com/cli/latest/reference/iot/delete-custom-metric.html)
+ [list\-security\-profiles](https://docs.aws.amazon.com/cli/latest/reference/iot/list-security-profiles.html)

## Custom metrics APIs<a name="dd-detect-custom-metrics-apis"></a>

The following APIs can be used to create and manage custom metrics\.
+ [CreateCustomMetric](https://docs.aws.amazon.com/iot/latest/apireference/API_CreateCustomMetric.html)
+ [DescribeCustomMetric](https://docs.aws.amazon.com/iot/latest/apireference/API_DescribeCustomMetric.html)
+ [ListCustomMetrics](https://docs.aws.amazon.com/iot/latest/apireference/API_ListCustomMetrics.html)
+ [UpdateCustomMetric](https://docs.aws.amazon.com/iot/latest/apireference/API_UpdateCustomMetric.html)
+ [DeleteCustomMetric](https://docs.aws.amazon.com/iot/latest/apireference/API_DeleteCustomMetric.html)
+ [ListSecurityProfiles](https://docs.aws.amazon.com/iot/latest/apireference/API_ListSecurityProfiles.html)