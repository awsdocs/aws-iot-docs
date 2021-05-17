# ML Detect reference<a name="dd-detect-ml"></a>

With machine learning Detect \(ML Detect\), you create Security Profiles that use machine learning to learn expected device behaviors by automatically creating models based on historical device data, and assign these profiles to a group of devices or all the devices in your fleet\. AWS IoT Device Defender then identifies anomalies and triggers alarms using the ML models\.

For information about how to get started with ML Detect, see [ML Detect guide](dd-detect-ml-getting-started.md)\.

**Topics**
+ [Use cases of ML Detect](#dd-detect-ml-use-cases)
+ [How ML Detect works](#dd-detect-ml-how-it-works)
+ [Minimum requirements](#dd-detect-ml-requirements)
+ [Limitations](#dd-detect-ml-limitations)
+ [Supported metrics](#dd-detect-ml-metrics)
+ [Service quotas](#dd-detect-ml-quotas)
+ [ML Detect CLI commands](#dd-detect-ml-cli-commands)
+ [ML Detect APIs](#dd-detect-ml-apis)
+ [Pause or delete an ML Detect Security Profile](#dd-detect-ml-disable-feature)

## Use cases of ML Detect<a name="dd-detect-ml-use-cases"></a>

You can use ML Detect to monitor your fleet devices when it's difficult to set the expected behaviors of devices\. For example, to monitor the number of disconnects metric, it might not be clear what is considered an acceptable threshold\. In this case, you can enable ML Detect to identify anomalous disconnect metric datapoints based off historical data reported from devices\.

Another use case of ML Detect is to monitor device behaviors that change dynamically over time\. ML Detect periodically learns the dynamic expected device behaviors based on changing data patterns from devices\. For example, device message sent volume could vary between weekdays and weekends, and ML detect will learn this dynamic behavior\.

## How ML Detect works<a name="dd-detect-ml-how-it-works"></a>

Using ML Detect, you can create behaviors to identify operational and security anomalies across [6 cloud\-side metrics](#dd-detect-ml-metrics) and [7 device\-side metrics](#dd-detect-ml-metrics)\. After the initial model training period, ML Detect refreshes the models daily based on the trailing 14 days of data\. It monitors datapoints for these metrics with the ML models and triggers an alarm if an anomaly is detected\.

ML Detect works best if you attach a Security Profile to a collection of devices with similar expected behaviors\. For example, if some of your devices are used at customers’ homes and other devices at business offices, the device behavior patterns might differ significantly between the two groups\. You can organize the devices into a *home\-device* thing group and an *office\-device* thing group\. For the best anomaly detection efficacy, attach each thing group to a separate ML Detect Security Profile\.

While ML Detect is building the initial model, it requires 14 days and a minimum of 25,000 datapoints per metric over the trailing 14\-day period to generate a model\. Afterwards, it updates the model every day there is a minimum number of metric datapoints\. If the minimum requirement isn't met, ML Detect attempts to build the model the next day, and will retry daily for the next 30 days before discontinuing the model for evaluations\.

## Minimum requirements<a name="dd-detect-ml-requirements"></a>

For training and creating the initial ML model, ML Detect has the following minimum requirements\.

**Minimum training period**  
It takes 14 days for the initial models to be built\. After that, the model refreshes every day with metric data from a 14\-day trailing period\.

**Minimum total datapoints**  
The minimum required datapoints to build an ML model is 25,000 datapoints per metric for the last 14 days\. For ongoing training and refreshing of the model, ML Detect requires the minimum datapoints be met from monitored devices\. It’s roughly the equivalent of the following setups:  
+ 60 devices connecting and having activity on AWS IoT at 45\-minute intervals\.
+ 40 devices at 30\-minute intervals\.
+ 15 devices at 10\-minute intervals\.
+ 7 devices at 5\-minute intervals\.

**Device group targets**  
In order for data collection to progress, you must have things in the target thing groups for the Security Profile\.

After the initial model is created, ML models refresh every day and require at least 25,000 datapoints for 14\-day trailing period\.

## Limitations<a name="dd-detect-ml-limitations"></a>

You can't currently use ML Detect with dimensions or with custom metrics\. The following metrics are not supported with ML Detect\.

**Cloud\-side metrics not supported with ML Detect:**  
+ [Source IP \(aws:source\-ip\-address\)](detect-cloud-side-metrics.md#detect-ip-address)

**Device\-side metrics not supported with ML Detect:**  
+ [Destination IPs \(`aws:destination-ip-addresses`\)](detect-device-side-metrics.md#detect-destination-ip-addresses)
+ [Listening TCP ports \(`aws:listening-tcp-ports`\)](detect-device-side-metrics.md#detect-listening-tcp-ports)
+ [Listening UDP ports \(`aws:listening-udp-ports`\)](detect-device-side-metrics.md#detect-listening-udp-ports)

## Supported metrics<a name="dd-detect-ml-metrics"></a>

You can use the following cloud\-side metrics with ML Detect:
+ [Authorization failures \(aws:num\-authorization\-failures\)](detect-cloud-side-metrics.md#detect-auth-failures)
+ [Connection attempts \(aws:num\-connection\-attempts\)](detect-cloud-side-metrics.md#detect-num-connection-attempts)
+ [Disconnects \(aws:num\-disconnects\)](detect-cloud-side-metrics.md#detect-num-disconnects)
+ [Message size \(aws:message\-byte\-size\)](detect-cloud-side-metrics.md#detect-message-size)
+ [Messages sent \(aws:num\-messages\-sent\)](detect-cloud-side-metrics.md#detect-messages-sent)
+ [Messages received \(num\-messages\-received\)](detect-cloud-side-metrics.md#detect-messages-received)

You can use the following device\-side metrics with ML Detect:
+ [Bytes out \(`aws:all-bytes-out`\)](detect-device-side-metrics.md#detect-all-bytes-out)
+ [Bytes in \(`aws:all-bytes-in`\)](detect-device-side-metrics.md#detect-all-bytes-in)
+ [Listening TCP port count \(`aws:num-listening-tcp-ports`\)](detect-device-side-metrics.md#detect-num-listening-tcp-ports)
+ [Listening UDP port count \(`aws:num-listening-udp-ports`\)](detect-device-side-metrics.md#detect-num-listening-udp-ports)
+ [Packets out \(`aws:all-packets-out`\)](detect-device-side-metrics.md#detect-all-packets-out)
+ [Packets in \(`aws:all-packets-in`\)](detect-device-side-metrics.md#detect-all-packets-in)
+ [Established TCP connections count \(`aws:num-established-tcp-connections`\)](detect-device-side-metrics.md#detect-num-established-tcp-connections)

## Service quotas<a name="dd-detect-ml-quotas"></a>

For information about ML Detect service quotas and limits, see [AWS IoT Device Defender endpoints and quotas](https://docs.aws.amazon.com/general/latest/gr/iot_device_defender.html)\.

## ML Detect CLI commands<a name="dd-detect-ml-cli-commands"></a>

You can use the following CLI commands to create and manage ML Detect\.
+ [create\-security\-profile](https://docs.aws.amazon.com/cli/latest/reference/iot/create-security-profile.html)
+ [attach\-security\-profile](https://docs.aws.amazon.com/cli/latest/reference/iot/attach-security-profile.html)
+ [list\-security\-profiles](https://docs.aws.amazon.com/cli/latest/reference/iot/list-security-profiles.html)
+ [describe\-security\-profile](https://docs.aws.amazon.com/cli/latest/reference/iot/describe-security-profile.html)
+ [update\-security\-profile](https://docs.aws.amazon.com/cli/latest/reference/iot/update-security-profile.html)
+ [delete\-security\-profile](https://docs.aws.amazon.com/cli/latest/reference/iot/delete-security-profile.html)
+ [get\-behavior\-model\-training\-summaries](https://docs.aws.amazon.com/cli/latest/reference/iot/get-behavior-model-training-summaries.html)
+ [list\-active\-violations](https://docs.aws.amazon.com/cli/latest/reference/iot/list-active-violations.html)
+ [list\-violation\-events](https://docs.aws.amazon.com/cli/latest/reference/iot/list-violation-events.html)

## ML Detect APIs<a name="dd-detect-ml-apis"></a>

The following APIs can be used to create and manage ML Detect Security Profiles\.
+ [CreateSecurityProfile](https://docs.aws.amazon.com/iot/latest/apireference/API_CreateSecurityProfile.html)
+ [AttachSecurityProfile](https://docs.aws.amazon.com/iot/latest/apireference/API_AttachSecurityProfile.html)
+ [ListSecurityProfiles](https://docs.aws.amazon.com/iot/latest/apireference/API_ListSecurityProfiles.html)
+ [DescribeSecurityProfile](https://docs.aws.amazon.com/iot/latest/apireference/API_DescribeSecurityProfile.html)
+ [UpdateSecurityProfile](https://docs.aws.amazon.com/iot/latest/apireference/API_UpdateSecurityProfile.html)
+ [DeleteSecurityProfile](https://docs.aws.amazon.com/iot/latest/apireference/API_DeleteSecurityProfile.html)
+ [GetBehaviorModelTrainingSummaries](https://docs.aws.amazon.com/iot/latest/apireference/API_GetBehaviorModelTrainingSummaries.html)
+ [ListActiveViolations](https://docs.aws.amazon.com/iot/latest/apireference/API_ListActiveViolations.html)
+ [ListViolationEvents](https://docs.aws.amazon.com/iot/latest/apireference/API_ListViolationEvents.html)

## Pause or delete an ML Detect Security Profile<a name="dd-detect-ml-disable-feature"></a>

You can pause your ML Detect Security Profile to stop monitoring device behaviors temporarily, or delete your ML Detect Security Profile to stop monitoring device behaviors for an extended period of time\.

**Pause ML Detect Security Profile by using the console**  
To pause an ML Detect Security Profile using the console, you must first have an empty thing group\. To create an empty thing group, see [Static thing groups](thing-groups.md)\. If you have created an empty thing group, then set the empty thing group as the target of the ML Detect Security Profile\.  
 You need to set the target of your Security Profile back to a device group with devices within 30 days, or you won't be able to reactivate the Security Profile\.

**Delete ML Detect Security Profile by using the console**  

To delete a Security Profile, follow these steps:

1.  In the AWS IoT console navigate to the sidebar and choose the **Defend** section\.

1. Under **Defend**, choose **Detect** and then **Security Profiles**\.

1. Choose the ML Detect Security Profile you want to delete\.

1. Choose **Actions**, and then from the options, choose **Delete**\.
After an ML Detect Security Profile is deleted, you won’t be able to reactivate the Security Profile\.

**Pause an ML Detect Security Profile by using the CLI**  
To pause a ML Detect Security Profile by using the CLI, use the `detach-security-security-profile` command:  

```
$aws iot detach-security-profile --security-profile-name SecurityProfileName --security-profile-target-arn arn:aws:iot:us-east-1:123456789012:all/registered-things
```
This option is only available in AWS CLI\. Similar to the console workflow, you need to set the target of your Security Profile back to a device group with devices within 30 days, or you won't be able to reactivate the Security Profile\. To attach a Security Profile to a device group, use the [https://docs.aws.amazon.com/cli/latest/reference/iot/attach-security-profile.html](https://docs.aws.amazon.com/cli/latest/reference/iot/attach-security-profile.html) command\.

**Delete a ML Detect Security Profile by using the CLI**  
You can delete a Security Profile by using the `delete-security-profile` command below:   

```
delete-security-profile --security-profile-name SecurityProfileName
```
After an ML Detect Security Profile is deleted, you won’t be able to reactivate the Security Profile\.