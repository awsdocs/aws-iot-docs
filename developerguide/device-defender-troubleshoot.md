# AWS IoT Device Defender Troubleshooting Guide<a name="device-defender-troubleshoot"></a>General

Q: Are there any prerequisites for using AWS IoT Device Defender?   
A: If you want to use device\-reported metrics, you must first deploy an agent on your AWS IoT connected devices or device gateways\. Devices must provide a consistent client identifier or thing name\.Audit

Q: I enabled a check and my audit has been showing "In\-Progress" for a long time\. Is something wrong? When can I expect results?   
A: When a check is enabled, data collection starts immediately\. However, if your account has a large amount of data to collect \(certificates, things, policies, and so on\), the results of the check might not be available for some time after you have enabled it\. Detect

Q: How do I know the thresholds to set in an AWS IoT Device Defender security profile behavior?   
A: Start by creating a security profile behavior with low thresholds and attach it to a thing group that contains a representative set of devices\. You can use AWS IoT Device Defender to view the current metrics, and then fine\-tune the device behavior thresholds to match your use case\. 

Q: I created a behavior, but it is not triggering a violation when I expect it to\. How should I fix it?   
A: When you define a behavior, you are specifying how you expect your device to behave normally\. For example, if you have a security camera that only connects to one central server on TCP port 8888, you don't expect it to make any other connections\. To be alerted if the camera makes a connection on another port, you define a behavior like this:   

```
{
  "name": "Listening TCP Ports",
  "metric": "aws:listening-tcp-ports",
  "criteria": {
    "comparisonOperator": "in-port-set",
    "value": {
      "ports": [ 8888 ]
    }
  }
}
```
If the camera makes a TCP connection on TCP port 443, the device behavior would be violated and an alert would be triggered\. 

Q: One or more of my behaviors are in violation\. How do I clear the violation?   
A: Alarms clear after the device returns to expected behavior, as defined in the behavior profiles\. Behavior profiles are evaluated upon receipt of metrics data for your device\. If the device doesn't publish any metrics for more than two days, the violation event is set to `alarm-invalidated` automatically\.

Q: I deleted a behavior that was in violation, but how do I stop the alerts?   
A: Deleting a behavior stops all future violations and alerts for that behavior\. Earlier alerts must be drained from your notification mechanism\. When you delete a behavior, the record of violations of that behavior is retained for the same time period as all other violations in your account\. Device Metrics

Q: I'm submitting metrics reports that I know violate my behaviors, but no violations are being triggered\. What's wrong?   
A: Check that your metrics reports are being accepted by subscribing to the following MQTT topics:  

```
$aws/things/THING_NAME/defender/metrics/FORMAT/rejected
$aws/things/THING_NAME/defender/metrics/FORMAT/accepted
```
where `THING_NAME` is the name of the thing reporting the metric and `FORMAT` is either "json" or "cbor", depending on the format of the metrics report submitted by the thing\.  
After you have subscribed, you should receive messages on these topics for each metric report submitted\. A `rejected` message indicates that there was a problem parsing the metric report\. An error message is included in the message payload to help you correct any errors in your metric report\. An `accepted` message indicates the metric report was parsed properly\. 

Q: What happens if I send an empty metric in my metric report?  
A: An empty list of ports or IP addresses is always considered in conformity with the corresponding behavior\. If the corresponding behavior was in violation, the violation is cleared\. 

Q: Why do my device metric reports contain messages for devices that aren't in the AWS IoT registry?  
If you have one or more security profiles attached to all things or to all unregistered things, AWS IoT Device Defender includes metrics from unregistered things\. If you want to exclude metrics from unregistered things, you can attach the profiles to all registered devices instead of all devices\.

Q: I'm not seeing messages from one or more unregistered devices even though I applied a security profile to all unregistered devices or all devices\. How can I fix it?  
Verify that you are sending a well\-formed metrics report using one of the supported formats, For information, see [Device Metrics Document Specification](device-defender-detect.md#DetectMetricsMessagesSpec)\. Verify that the unregistered devices are using a consistent client identifier or thing name\. Messages reported by devices are rejected if the thing name contains control characters or if the thing name is longer than 128 bytes of UTF\-8 encoded characters\.

Q: What happens if an unregistered device is added to the registry or a registered device becomes unregistered?  
A: If a device is added to or removed from the registry:  
+ You see two separate violations for the device \(one under its registered thing name, one under its unregistered identity\) if it continues to publish metrics for violations\. Active violations for the old identity stop appearing after two days, but are available in violations history for up to 14 days\.

Q: Which value should I supply in the report ID field of my device metrics report?   
A: Use a unique value for each metric report, expressed as a positive integer\. A common practice is to use a [Unix epoch timestamp](https://en.wikipedia.org/wiki/Unix_time)\. 

Q: Should I create a dedicated MQTT connection for AWS IoT Device Defender metrics?   
A: A separate MQTT connection is not required\. 

Q: Which client ID should I use when connecting to publish device metrics?   
For devices \(things\) that are in the AWS IoT registry, use the registered thing name\. For devices that are not in the AWS IoT registry, use a consistent identifier when you connect to AWS IoT\. This practice helps match the violations to the thing name\.

Q: Can I publish metrics for a device with a different client ID?   
It is possible to publish metrics on behalf of another thing\. You can do this by publishing the metrics to the AWS IoT Device Defender reserved topic for that device\. For example, `Thing-1` would like to publish metrics for itself and also on behalf of `Thing-2`\. `Thing-1` collects its own metrics and publishes them on the MQTT topic:  

```
$aws/things/Thing-1/defender/metrics/json
```
`Thing-1` then obtains metrics from `Thing-2` and publishes those metrics on the MQTT topic:  

```
$aws/things/Thing-2/defender/metrics/json
```

Q: How many security profiles and behaviors can I have in my account?   
A: See [AWS IoT Device Defender Endpoints and Quotas](https://docs.aws.amazon.com/general/latest/gr/iot_device_defender.html)\. 

Q: What does a prototypical target role for an alert target look like?   
A: A role that allows AWS IoT Device Defender to publish alerts on an alert target \(SNS topic\) requires two things:   
+ A trust relationship that specifies iot\.amazonaws\.com as the trusted entity\. 
+ An attached policy that grants AWS IoT permission to publish on a specified SNS topic\. For example:

  ```
  {
      "Version": "2012-10-17",
      "Statement": [
          {
              "Effect": "Allow",
              "Action": "sns:Publish",
              "Resource": "<sns-topic-arn>"
          }
      ]
  }
  ```