# ML Detect reference<a name="dd-detect-ml"></a>


|  | 
| --- |
| ML Detect is in preview release for AWS IoT Device Defender and is subject to change\. | 

With machine learning \(ML\) Detect, you can create Security Profiles that uses machine learning to learn expected device behaviors by automatically creating models based on historical device data, and assign these profiles to a group of devices or all the devices in your fleet\. AWS IoT Device Defender then identifies anomalies and triggers alarms using the ML models\.

For steps on how to get started with ML Detect, see [ML Detect guide](dd-detect-ml-getting-started.md)\.

**Topics**
+ [How ML Detect works](#dd-detect-ml-how-it-works)
+ [Minimum requirements](#dd-detect-ml-requirements)
+ [Supported metrics](#dd-detect-ml-metrics)
+ [Service quotas](#dd-detect-ml-quotas)
+ [ML Detect CLI commands](#dd-detect-ml-cli-commands)
+ [ML Detect APIs](#dd-detect-ml-apis)

## How ML Detect works<a name="dd-detect-ml-how-it-works"></a>

With Rules Detect, you can create static alarms to identify operational and security anomalies across seven [cloud\-side metrics](detect-cloud-side-metrics.md) and ten [device\-side metrics](detect-device-side-metrics.md)\. ML Detect takes this a step further by automatically learning device behaviors with machine learning models using data across [six cloud\-side metrics](#dd-detect-ml-metrics) from a trailing 14 day period\. It then retrains the models each day to refresh the device behaviors based on the latest trailing 14 days after initial models are built\. ML Detect monitors and identifies anomalous datapoints for these metrics with the ML models and triggers an alarm if an anomaly is detected\. The key benefits are that it automatically detects operational and security anomalies across fleet devices and it dynamically updates expected device behaviors based on new data trends to reduce false positive rates\.

While ML Detect is building its initial model, it requires 14 days and a minimum of 25,000 datapoints per metric to generate the model\. Afterwards, it updates the model every day as long as the minimum 25,000 metric datapoints per model are met\. If the minimum metric datapoint requirement is not met, ML Detect will attempt to build the model on the next day\. It will retry daily for 30 days before discontinuing the model\.

## Minimum requirements<a name="dd-detect-ml-requirements"></a>

For training and creating the initial ML model, ML Detect has two minimum requirements\. 

**Minimum training period**  
It takes 14 days for the initial models to be built\. After that, the model refreshes every day with a 14\-day trailing window of metric data\.

**Minimum datapoints**  
The minimum required datapoints to build an ML model is 25,000 datapoints per metric for the last 14 days\. For ongoing training and refreshing of the model, ML Detect requires the minimum datapoints met from monitored devices\. It’s roughly the equivalent of the setups below:  
+ 60 devices connecting and having activity on AWS IoT at 45\-minute intervals\.
+ 40 devices at 30\-minute intervals\.
+ 15 devices at 10\-minute intervals\.
+ 7 to 8 devices at 5\-minute intervals\.

**Devices attached to a Security Profile target**  
In order for data collection to progress, you must have things in the target that the SecurityProfile is attached to\.

After the initial model is created, ML models refresh on a daily basis and require the same datapoint minimums\.

## Supported metrics<a name="dd-detect-ml-metrics"></a>

You can use the following cloud\-side metrics with ML Detect:
+ [Authorization failures \(aws:authorization\-failures\)](detect-cloud-side-metrics.md#detect-auth-failures)
+ [Connection attempts \(aws:num\-connection\-attempts\)](detect-cloud-side-metrics.md#detect-num-connection-attempts)
+ [Disconnects \(aws:num\-disconnects\)](detect-cloud-side-metrics.md#detect-num-disconnects)
+ [Message size \(aws:message\-byte\-size\)](detect-cloud-side-metrics.md#detect-message-size)
+ [Messages sent \(aws:num\-messages\-sent\)](detect-cloud-side-metrics.md#detect-messages-sent)
+ [Messages received \(num\-messages\-received\)](detect-cloud-side-metrics.md#detect-messages-received)

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

The following APIs can be used to create and manage ML Detect security profiles\.
+ [CreateSecurityProfile](https://docs.aws.amazon.com/iot/latest/apireference/API_CreateSecurityProfile.html)
+ [AttachSecurityProfile](https://docs.aws.amazon.com/iot/latest/apireference/API_AttachSecurityProfile.html)
+ [ListSecurityProfiles](https://docs.aws.amazon.com/iot/latest/apireference/API_ListSecurityProfiles.html)
+ [DescribeSecurityProfile](https://docs.aws.amazon.com/iot/latest/apireference/API_DescribeSecurityProfile.html)
+ [UpdateSecurityProfile](https://docs.aws.amazon.com/iot/latest/apireference/API_UpdateSecurityProfile.html)
+ [DeleteSecurityProfile](https://docs.aws.amazon.com/iot/latest/apireference/API_DeleteSecurityProfile.html)
+ [GetBehaviorModelTrainingSummaries](https://docs.aws.amazon.com/iot/latest/apireference/API_GetBehaviorModelTrainingSummaries.html)
+ [ListActiveViolations](https://docs.aws.amazon.com/iot/latest/apireference/API_ListActiveViolations.html)
+ [ListViolationEvents](https://docs.aws.amazon.com/iot/latest/apireference/API_ListViolationEvents.html)