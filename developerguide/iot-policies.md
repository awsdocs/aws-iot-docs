# Policies<a name="iot-policies"></a>

 policies are JSON documents\. They follow the same conventions as IAM policies\. supports named policies so many identities can reference the same policy document\. Named policies are versioned so they can be easily rolled back\.

 policies allow you to control access to the data plane\. The data plane consists of operations that allow you to connect to the message broker, send and receive MQTT messages, and get or update a device's shadow\.

An policy is a JSON document that contains one or more policy statements\. Each statement contains:
+ `Effect`, which specifies whether the action is allowed or denied\.
+ `Action`, which specifies the action the policy is allowing or denying\.
+ `Resource`, which specifies the resource or resources on which the action is allowed or denied\.

**Topics**
+ [Policy Actions](iot-policy-actions.md)
+ [Action Resources](iot-action-resources.md)
+ [Policy Variables](iot-policy-variables.md)
+ [Example AWS IoT Policies](example-iot-policies.md)
+ [Authorization with Amazon Cognito Identities](cog-iot-policies.md)