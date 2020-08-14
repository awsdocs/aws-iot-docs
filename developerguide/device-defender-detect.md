# Detect<a name="device-defender-detect"></a>

AWS IoT Device Defender Detect lets you identify unusual behavior that might indicate a compromised device by monitoring the behavior of your devices\. Using a combination of cloud\-side metrics \(from AWS IoT\) and device\-side metrics \(from agents that you install on your devices\) you can detect:
+ Changes in connection patterns\.
+ Devices that communicate to unauthorized or unrecognized endpoints\.
+ Changes in inbound and outbound device traffic patterns\.

You create security profiles, which contain definitions of expected device behaviors, and assign them to a group of devices or to all the devices in your fleet\. AWS IoT Device Defender Detect uses these security profiles to detect anomalies and send alerts through Amazon CloudWatch metrics and Amazon Simple Notification Service notifications\.

AWS IoT Device Defender Detect can detect security issues frequently found in connected devices: 
+ Traffic from a device to a known malicious IP address or to an unauthorized endpoint that indicates a potential malicious command and control channel\.
+ Anomalous traffic, such as a spike in outbound traffic, that indicates a device is participating in a DDoS\.
+ Devices with remote management interfaces and ports that are remotely accessible\.
+ A spike in the rate of messages sent to your account \(for example, from a rogue device that can result in excessive per\-message charges\)\.Use cases:

Measure attack surface  
You can use AWS IoT Device Defender Detect to measure the attack surface of your devices\. For example, you can identify devices with service ports that are often the target of attack campaigns \(telnet service running on ports 23/2323, SSH service running on port 22, HTTP/S services running on ports 80/443/8080/8081\)\. While these service ports might have legitimate reasons to be used on the devices, they are also usually part of the attack surface for adversaries and carry associated risks\. After AWS IoT Device Defender Detect alerts you to the attack surface, you can minimize it \(by eliminating unused network services\) or run additional assessments to identify security weaknesses \(for example, telnet configured with common, default, or weak passwords\)\. 

Detect device behavioral anomalies with possible security root causes  
You can use AWS IoT Device Defender Detect to alert you to unexpected device behavioral metrics \(the number of open ports, number of connections, an unexpected open port, connections to unexpected IP addresses\) that might indicate a security breach\. For example, a higher than expected number of TCP connections might indicate a device is being used for a DDoS attack\. A process listening on a port other than the one you expect might indicate a backdoor installed on a device for remote control\. You can use AWS IoT Device Defender Detect to probe the health of your device fleets and verify your security assumptions \(for example, no device is listening on port 23 or 2323\)\.   
You can enable machine learning \(ML\)\-based threat detection to automatically identify potential threats\. 

Detect an incorrectly configured device  
A spike in the number or size of messages sent from a device to your account might indicate an incorrectly configured device\. Such a device might increase your per\-message charges\. Similarly, a device with many authorization failures might require a reconfigured policy\.

## Monitoring the behavior of unregistered devices<a name="detect-unregistered-devices"></a>

AWS IoT Device Defender Detect makes it possible to identify unusual behaviors for devices that are not registered in the AWS IoT registry\. You can define security profiles that are specific to one of the following target types:
+ All devices
+ All registered devices \(things in the AWS IoT registry\)
+ All unregistered devices
+ Devices in a thing group

A security profile defines a set of expected behaviors for devices in your account and specifies the actions to take when an anomaly is detected\. Security profiles should be attached to the most specific targets to give you granular control over which devices are being evaluated against that profile\.

Unregistered devices must provide a consistent MQTT client identifier or thing name \(for devices that report device metrics\) over the device lifetime so all violations and metrics are attributed to the same device\. 

**Important**  
Messages reported by devices are rejected if the thing name contains control characters or if the thing name is longer than 128 bytes of UTF\-8 encoded characters\.