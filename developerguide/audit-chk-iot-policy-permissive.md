# AWS IoT policies overly permissive<a name="audit-chk-iot-policy-permissive"></a>

An AWS IoT policy gives permissions that are too broad or unrestricted\. It grants permission to send or receive MQTT messages for a broad set of devices, or grants permission to access or modify shadow and job execution data for a broad set of devices\.  

In general, a policy for a device should grant access to resources associated with just that device and no or very few other devices\. With some exceptions, using a wildcard \(for example, "\*"\) to specify resources in such a policy is considered too broad or unrestricted\.

This check appears as `IOT_POLICY_OVERLY_PERMISSIVE_CHECK` in the CLI and API\.

Severity: **Critical**

## Details<a name="audit-chk-iot-policy-permissive-details"></a>

The following reason code is returned when this check finds a noncompliant AWS IoT policy:
+ ALLOWS\_BROAD\_ACCESS\_TO\_IOT\_DATA\_PLANE\_ACTIONS

## Why it matters<a name="audit-chk-iot-policy-permissive-why-it-matters"></a>

A certificate, Amazon Cognito identity, or thing group with an overly permissive policy can, if compromised, impact the security of your entire account\. An attacker could use such broad access to read or modify shadows, jobs, or job executions for all your devices\. Or an attacker could use a compromised certificate to connect malicious devices or launch a DDOS attack on your network\.

## How to fix it<a name="audit-chk-iot-policy-permissive-how-to-fix"></a>

Follow these steps to fix any noncompliant policies attached to things, thing groups, or other entities:

1. Use [CreatePolicyVersion](https://docs.aws.amazon.com/iot/latest/apireference/API_CreatePolicyVersion.html) to create a new, compliant version of the policy\. Set the `setAsDefault` flag to true\. \(This makes this new version operative for all entities that use the policy\.\)

1. Use [ListTargetsForPolicy](https://docs.aws.amazon.com/iot/latest/apireference/API_ListTargetsForPolicy.html) to get a list of targets \(certificates, thing groups\) that the policy is attached to and determine which devices are included in the groups or which use the certificates to connect\.

1. Verify that all associated devices are able to connect to AWS IoT\. If a device is unable to connect, use [ SetPolicyVersion](https://docs.aws.amazon.com/iot/latest/apireference/API_SetPolicyVersion.html) to roll back the default policy to the previous version, revise the policy, and try again\. 

You can use mitigation actions to:
+ Apply the `REPLACE_DEFAULT_POLICY_VERSION` mitigation action on your audit findings to make this change\. 
+ Apply the `PUBLISH_FINDINGS_TO_SNS` mitigation action if you want to implement a custom response in response to the Amazon SNS message\. 

For more information, see [Mitigation actions](dd-mitigation-actions.md)\. 

Use [AWS IoT policy variables](iot-policy-variables.md) to dynamically reference AWS IoT resources in your policies\.

## MQTT permissions<a name="audit-chk-iot-policy-permissive-mqtt-permissions"></a>

MQTT messages are sent through the AWS IoT message broker and are used by devices to perform many actions, including accessing and modifying shadow state and job execution state\. A policy that grants permission to a device to connect, publish, or subscribe to MQTT messages should restrict these actions to specific resources as follows:

**Connect**  
+ noncompliant:

  ```
  arn:aws:iot:region:account-id:client/*
  ```

  The wildcard \* allows any device to connect to AWS IoT\.

  ```
  arn:aws:iot:region:account-id:client/${iot:ClientId}
  ```

  Unless `iot:Connection.Thing.IsAttached` is set to true in the condition keys, this is equivalent to the wildcard \* as in the previous example\.
+ compliant:

  ```
  {
    "Version": "2012-10-17",
    "Statement": [
      {
        "Effect": "Allow",
        "Action": [ "iot:Connect" ],
        "Resource": [
          "arn:aws:iot:region:account-id:client/${iot:Connection.Thing.ThingName}"
        ],
        "Condition": {
          "Bool": { "iot:Connection.Thing.IsAttached": "true" }
        }
      }
    ]
  }
  ```

  The resource specification contains a variable that matches the device name used to connect\. The condition statement further restricts the permission by checking that the certificate used by the MQTT client matches that attached to the thing with the name used\.

**Publish**  
+ noncompliant:

  ```
  arn:aws:iot:region:account-id:topic/$aws/things/*/shadow/update
  ```

  This allows the device to update the shadow of any device \(\* = all devices\)\.

  ```
  arn:aws:iot:region:account-id:topic/$aws/things/*
  ```

  This allows the device to read, update, or delete the shadow of any device\.
+ compliant:

  ```
  {
    "Version": "2012-10-17",
    "Statement": [
      {
        "Effect": "Allow",
        "Action": [ "iot:Publish" ],
        "Resource": [
          "arn:aws:iot:region:account-id:topic/$aws/things/${iot:Connection.Thing.ThingName}/shadow/*"
        ],
      }
    ]
  }
  ```

  The resource specification contains a wildcard, but it only matches any shadow\-related topic for the device whose thing name is used to connect\.

**Subscribe**  
+ noncompliant:

  ```
  arn:aws:iot:region:account-id:topicfilter/$aws/things/*
  ```

  This allows the device to subscribe to reserved shadow or job topics for all devices\.

  ```
  arn:aws:iot:region:account-id:topicfilter/$aws/things/*
  ```

  The same as the previous example, but using the \# wildcard\.

  ```
  arn:aws:iot:region:account-id:topic/$aws/things/+/shadow/update
  ```

  This allows the device to see shadow updates on any device \(\+ = all devices\)\.
+ compliant:

  ```
  {
    "Version": "2012-10-17",
    "Statement": [
      {
        "Effect": "Allow",
        "Action": [ "iot:Subscribe" ],
        "Resource": [
          "arn:aws:iot:region:account-id:topicfilter/$aws/things/${iot:Connection.Thing.ThingName}/shadow/*"
          "arn:aws:iot:region:account-id:topicfilter/$aws/things/${iot:Connection.Thing.ThingName}/jobs/*"
        ],
      }
    ]
  }
  ```

  The resource specifications contain wildcards, but they only match any shadow\-related topic and any job\-related topic for the device whose thing name is used to connect\.

**Receive**  
+ compliant:

  ```
  arn:aws:iot:region:account-id:topic/$aws/things/*
  ```

  This is compliant because the device can only receive messages from topics on which it has permission to subscribe\.

## Shadow and job permissions<a name="shadow-job-permissions"></a>

A policy that grants permission to a device to perform an API action to access or modify device shadows or job execution data should restrict these actions to specific resources\. The following are the API actions:
+ `DeleteThingShadow`
+ `GetThingShadow`
+ `UpdateThingShadow`
+ `DescribeJobExecution`
+ `GetPendingJobExecutions`
+ `StartNextPendingJobExecution`
+ `UpdateJobExecution`

**Examples**
+ noncompliant:

  ```
  arn:aws:iot:region:account-id:thing/*
  ```

  This allows the device to perform the specified action on any thing\.
+ compliant:

  ```
  {
    "Version": "2012-10-17",
    "Statement": [
      {
        "Effect": "Allow",
        "Action": [ 
            "iot:DeleteThingShadow",
            "iot:GetThingShadow",
            "iot:UpdateThingShadow",
            "iot:DescribeJobExecution",
            "iot:GetPendingJobExecutions",
            "iot:StartNextPendingJobExecution",
            "iot:UpdateJobExecution"
        ],
        "Resource": [
          "arn:aws:iot:region:account-id:/thing/MyThing1",
          "arn:aws:iot:region:account-id:/thing/MyThing2"
        ]
      }
    ]
  }
  ```

  This allows the device to perform the specified actions on only two things\.