# Troubleshooting Guide<a name="device-defender-troubleshoot"></a>General

Q: Are there any prerequisites to using ?   
A: You can use the Detect feature for devices that are registered as things with AWS IoT\. To use the cloud\-side metrics, devices must connect using the thing name as their client Id\. Audit

Q: I enabled a check and my audit has been showing "In\-Progress" for a long time\. Is something wrong? When will I get results?   
A: When a check is enabled, data collection begins immediately\. However, if you have a large amount of data in your account to be collected \(certificates, things, policies, etc\.\) then the results of the check may not be available for some time after you have enabled it\. Detect

Q: How do I know what thresholds should be set in an security profile behavior?   
A: Start by creating a security profile behavior with low thresholds and attach it to a thing group consisting of a representative set of devices\. Device defender will alert you with the metric datapoints emitted by the device for the behaviors that are violated\. You can then fine\-tune the device behavior thresholds to match your use case\. 

Q: I created a behavior, but it is not triggering a violation when I expect it to\. How should I fix it?   
A: Keep in mind that when you define a behavior you are indicating how you expect your device to behave normally\. For example, if you have a security camera that only connects to one central server on TCP port 8888, you don’t expect it to make any other connections\. To be alerted if the camera makes a connection on another port, you could define a behavior such as:   

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
Once the behavior has been created, if the camera makes a TCP connection on TCP Port 443, the device behavior would be violated and would trigger an alert\. 

Q: One or more of my behaviors are in violation\. How do I clear the violation?   
A: Alarms will clear once the device returns to expected behavior, as defined by the behavior profiles you have defined\. Behavior profiles are evaluated upon receipt of metrics data for your device\. 

Q: I deleted a behavior that was in violation, how do I stop the alerts?   
A: Deleting a behaviour will stop all future violations and alerts for that behavior\. Prior alerts will need to be drained from your notification mechanism as normal\. However, when a behavior is deleted, the record of violations of that behavior will be retained for the same time period as all other violations in your account\. Device Metrics

Q: I’m submitting metrics reports that I know violate my behaviors, but no violations are being triggered\. What's wrong?   
A: Check that your metrics reports are being accepted by subscribing to the following MQTT topics:  

```
$aws/things/THING_NAME/defender/metrics/FORMAT/rejected
$aws/things/THING_NAME/defender/metrics/FORMAT/accepted
```
where “THING\_NAME” is the name of the thing reporting the metric and FORMAT is either “json” or “cbor” depending on the format of the metrics report the thing submits\.  
After you have subscribed, you should receive messages on these topics for each metric report submitted\. A "rejected" message indicates that there was a problem parsing the metric report\. An error message is included in the message payload to help you correct any errors in your metric report\. An "accepted" message indicates the metric report was parsed properly\. 

Q: What happens if I send an empty metric in my metric report?  
A: An empty list of ports or IP addresses is always considered in conformity with the corresponding behavior\. If the corresponding behavior was in violation, then the violation will be cleared\. 

Q: My device metric reports are getting rejected with an error message of "XXXX Not a Thing"\. How can I fix it?   
A: Make sure you are submitting metrics for a thing which is registered in your AWS IoT account\. requires things to be registered in your account to evaluate reported metrics against defined behaviors\.   
If you are submitting metrics from a registered thing, verify that you are sending a well\-formed metrics report using one of the supported formats\. \([Device Metrics Document Specification](device-defender-detect.md#DetectMetricsMessagesSpec)\)

Q: What value should I supply in the report id field in my device metrics report?   
A: This should be a unique value for each metric report, expressed as a positive integer\. A common practice is to use a [Unix epoch timestamp](https://en.wikipedia.org/wiki/Unix_time)\. 

Q: Should I create a dedicated MQTT connection for metrics?   
A: A separate MQTT connection is not required\. 

Q: What client id should I use when connecting to publish device metrics?   
You should use a registered thing name as the clientId when connecting to AWS IoT\. 

Q: Can I publish metrics for a device with a different clientId?   
It is possible to publish metrics on behalf of another thing, as long as that thing is registered in your AWS account\. This can be accomplished by publishing the metrics to the reserved topic for that device\. For example, "Thing\-1" would like to publish metrics for itself and also on behalf of "Thing\-2"\. "Thing\-1" collects its own metrics and publishes them on the MQTT topic:  

```
$aws/things/Thing-1/defender/metrics/json
```
"Thing\-1" will then obtain metrics from "Thing\-2" and publish these metrics on the MQTT topic:  

```
$aws/things/Thing-2/defender/metrics/json
```
As long as "Thing\-1" and "Thing\-2" are registered in your account, metric submission and evaluation will work properly\. 

Q: How many security profiles and behaviors can I have in my account?   
A: Refer to the Detect [Service Limits](device-defender-detect.md#detect-limits) section of the Developer Guide\. 

Q: What does a prototypical target role for an alert target look like?   
A: A role that allows to publish alerts on an alert target \(SNS topic\) requires 2 things:   

1. a trust relationship specifying iot\.amazonaws\.com as the trusted entity and 

1. an attached policy which grants AWS IoT permission to publish on a specified SNS topic, for example:

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