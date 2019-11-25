# AWS IoT Policies<a name="iot-policies"></a>

AWS IoT policies are JSON documents\. They follow the same conventions as IAM policies\. AWS IoT supports named policies so many identities can reference the same policy document\. Named policies are versioned so they can be easily rolled back\.

AWS IoT policies allow you to control access to the AWS IoT data plane\. The AWS IoT data plane consists of operations that allow you to connect to the AWS IoT message broker, send and receive MQTT messages, and get or update a device's shadow\.

An AWS IoT policy is a JSON document that contains one or more policy statements\. Each statement contains:
+ `Effect`, which specifies whether the action is allowed or denied\.
+ `Action`, which specifies the action the policy is allowing or denying\.
+ `Resource`, which specifies the resource or resources on which the action is allowed or denied\.