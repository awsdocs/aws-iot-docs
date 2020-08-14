# How to use AWS IoT Device Defender detect<a name="detect-HowToHowTo"></a>

1. You can use AWS IoT Device Defender Detect with just cloud\-side metrics, but if you plan to use device\-reported metrics, you must first deploy the AWS IoT SDK on your AWS IoT connected devices or device gateways\. For more information, see [Sending metrics from devices](DetectMetricsMessages.md)\.

1. Consider viewing the metrics that your devices generate before you define behaviors and create alarms\. AWS IoT can collect metrics from your devices so you can first identify usual or unusual behavior for a group of devices, or for all devices in your account\. Use [CreateSecurityProfile](dd-api-iot-CreateSecurityProfile.md), but specify only those `additionalMetricsToRetain` that you're interested in\. Don't specify `behaviors` at this point\. 

   Use the AWS IoT console to look at your device metrics to see what constitutes typical behavior for your devices\.

1. Create a set of behaviors for your security profile\. Behaviors contain metrics that specify normal behavior for a group of devices or for all devices in your account\. For more information and examples, see [Metrics](detect-metrics.md)\. After you create a set of behaviors, you can validate them with [ValidateSecurityProfileBehaviors](dd-api-iot-ValidateSecurityProfileBehaviors.md)\. 

1. Use the [CreateSecurityProfile](dd-api-iot-CreateSecurityProfile.md) action to create a security profile that includes your behaviors\. You can use the `alertTargets` parameter to have alerts sent to a target \(an SNS topic\) when a device violates a behavior\. \(If you send alerts using SNS, be aware that these count against your AWS account's SNS topic quota\. It's possible that a large burst of violations can exceed your SNS topic quota\. You can also use CloudWatch metrics to check for violations\. For more information, see [ Using AWS IoT metricsUsing AWS IoT metrics  The metrics reported by AWS IoT provide information that you can analyze in different ways\. The following use cases are based on a scenario where you have ten things that connect to the internet once a day\. Each day:   Ten things connect to AWS IoT at roughly the same time\.   Each thing subscribes to a topic filter, and then waits for an hour before disconnecting\. During this period, things communicate with one another and learn more about the state of the world\.   Each thing publishes some perception it has based on its newly found data using `UpdateThingShadow`\.   Each thing disconnects from AWS IoT\.   To help you get started, these topics explore some of the questions that you might have\.   [How can I be notified if my things do not connect successfully each day?](creating_alarms.md#how_to_detect_connection_failures)   [How can I be notified if my things are not publishing data each day?](creating_alarms.md#how_to_detect_publish_failures)   [How can I be notified if my thing's shadow updates are being rejected each day?](creating_alarms.md#detect_rejected_updates)   [How can I create a CloudWatch alarm for Jobs?](creating_alarms.md#cw-jobs-alarms)   ](monitoring-cloudwatch.md#how_to_use_metrics)\. 

1. Use the [AttachSecurityProfile](dd-api-iot-AttachSecurityProfile.md) action to attach the security profile to a group of devices \(a thing group\), all registered things in your account, all unregistered things, or all devices\. AWS IoT Device Defender Detect starts checking for abnormal behavior and, if any behavior violations are detected, sends alerts\. You might want to attach a security profile to all unregistered things if, for example, you expect to interact with mobile devices that are not in your account's thing registry\. You can define different sets of behaviors for different groups of devices to meet your needs\.

   To attach a security profile to a group of devices, you must specify the ARN of the thing group that contains them\. A thing group ARN has the following format\.

   ```
   arn:aws:iot:region:account-id:thinggroup/thing-group-name
   ```

   To attach a security profile to all of the registered things in an AWS account \(ignoring unregistered things\), you must specify an ARN with the following format\.

   ```
   arn:aws:iot:region:account-id:all/registered-things
   ```

   To attach a security profile to all unregistered things, you must specify an ARN with the following format\.

   ```
   arn:aws:iot:region:account-id:all/unregistered-things
   ```

   To attach a security profile to all devices, you must specify an ARN with the following format\.

   ```
   arn:aws:iot:region:account-id:all/things
   ```

1. You can also keep track of violations with the [ListActiveViolations](dd-api-iot-ListActiveViolations.md) action, which lets you to see which violations were detected for a given security profile or target device\.

   Use the [ListViolationEvents](dd-api-iot-ListViolationEvents.md) action to see which violations were detected during a specified time period\. You can filter these results by security profile or device\.

1. If your devices violate the defined behaviors too often, or not often enough, you should fine\-tune the behavior definitions\. 

1. To review the security profiles that you set up and the devices that are being monitored, use the [ListSecurityProfiles](dd-api-iot-ListSecurityProfiles.md), [ListSecurityProfilesForTarget](dd-api-iot-ListSecurityProfilesForTarget.md), and [ListTargetsForSecurityProfile](dd-api-iot-ListTargetsForSecurityProfile.md) actions\. 

   Use the [DescribeSecurityProfile](dd-api-iot-DescribeSecurityProfile.md) action to get more details about a security profile\. 

1. To update a security profile, use the [UpdateSecurityProfile](dd-api-iot-UpdateSecurityProfile.md) action\. Use the [DetachSecurityProfile](dd-api-iot-DetachSecurityProfile.md) action to detach a security profile from an account or target thing group\. Use the [DeleteSecurityProfile](dd-api-iot-DeleteSecurityProfile.md) action to delete a security profile entirely\. 