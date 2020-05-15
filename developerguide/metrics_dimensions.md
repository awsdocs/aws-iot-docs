# AWS IoT Metrics and Dimensions<a name="metrics_dimensions"></a>

When you interact with AWS IoT, the service sends the following metrics and dimensions to CloudWatch every minute\. You can use the following procedures to view the metrics for AWS IoT\.

**To view metrics \(CloudWatch console\)**

Metrics are grouped first by the service namespace, and then by the various dimension combinations within each namespace\.

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Metrics**\.

1. In the **CloudWatch Metrics by Category** pane, under the metrics category for AWS IoT, choose a metrics category, and then in the upper pane, scroll down to view the full list of metrics\.

**To view metrics \(CLI\)**
+ At a command prompt, use the following command:

  ```
  1. aws cloudwatch list-metrics --namespace "AWS/IoT"
  ```

CloudWatch displays the following metrics for AWS IoT:

## Dimensions for Metrics<a name="aws-iot-metricdimensions"></a>

Metrics use the namespace and provide metrics for the following dimensions:


****  

| Dimension | Description | 
| --- | --- | 
| ActionType |  The [action type](https://docs.aws.amazon.com/iot/latest/developerguide/iot-rule-actions.html) specified by the rule that triggered the request\.  | 
|  `Protocol`  |  The protocol used to make the request\. Valid values are: MQTT or HTTP  | 
|  `RuleName`  |  The name of the rule triggered by the request\.  | 
|  `JobId`  |  The ID of the job whose progress or message connection success/failure is being monitored\.  | 
|  `CheckName`  |  The name of the Device Defender audit check whose results are being monitored\.  | 
|  `ScheduledAuditName`  |  The name of the Device Defender scheduled audit whose check results are being monitored\. This has the value `OnDemand` if the results reported are for an audit that was performed on demand\.  | 
|  `SecurityProfileName`  |  The name of the Device Defender Detect security profile whose behaviors are being monitored\.  | 
|  `BehaviorName`  |  The name of the Device Defender Detect security profile behavior that is being monitored\.  | 
|  `TemplateName`  |  The name of the provisioning template\.  | 