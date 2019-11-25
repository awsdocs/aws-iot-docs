# Audit<a name="device-defender-audit"></a>

An AWS IoT Device Defender audit looks at account\- and device\-related settings and policies to ensure security measures are in place\. An audit can help you detect any drifts from security best practices or access policies \(for example, multiple devices using the same identity, or overly permissive policies that allow one device to read and update data for many other devices\)\. You can run audits as needed \(*on\-demand audits*\) or schedule them to be run periodically \(*scheduled audits*\)\. 

An AWS IoT Device Defender audit runs a set of predefined checks for common IoT security best practices and device vulnerabilities\. Examples of predefined checks include policies that grant permission to read or update data on multiple devices, devices that share an identity \(X\.509 certificate\), or certificates that are expiring or have been revoked but are still active\.

## Issue Severity<a name="device-defender-audit-severity"></a>

Issue severity indicates the level of concern associated with each identified instance of noncompliance and the recommended time to remediation\.

Critical  
Noncompliant audit checks with this severity identify issues that require urgent attention\. Critical issues often allow bad actors with little sophistication and no insider knowledge or special credentials to easily gain access to or control of your assets\.

High  
Noncompliant audit checks with this severity require urgent investigation and remediation planning after critical issues are addressed\. Like critical issues, high severity issues often provide bad actors with access to or control of your assets\. However, high severity issues are often more difficult to exploit\. They might require special tools, insider knowledge, or specific setups\.

Medium  
Noncompliant audit checks with this severity present issues that need attention as part of your continuous security posture maintenance\. Medium severity issues might cause negative operational impact, such as unplanned outages due to malfunction of security controls\. These issues might also provide bad actors with limited access to or control of your assets, or might facilitate parts of their malicious actions\.

Low  
Noncompliant audit checks with this severity often indicate security best practices were overlooked or bypassed\. Although they might not cause an immediate security impact on their own, these lapses can be exploited by bad actors\. Like medium severity issues, low severity issues require attention as part of your continuous security posture maintenance\.

## Audit Checks<a name="device-defender-audit-checks"></a>

**Note**  
When a check is enabled, data collection starts immediately\. If there is a large amount of data in your account to be collected, results of the check might not be available for some time after you have enabled it\.

The following audit checks are supported:

------
#### [ REVOKED\_CA\_CERT\_CHECK ]

A CA certificate was revoked, but is still active in AWS IoT\.

Severity: **Critical**

------
#### [ Details ]

A CA certificate is marked as revoked in the certificate revocation list maintained by the issuing authority, but is still marked as ACTIVE or PENDING\_TRANSFER in AWS IoT\.

The following reason codes are returned when this check finds a noncompliant CA certificate:
+ CERTIFICATE\_REVOKED\_BY\_ISSUER

------
#### [ Why it matters ]

A revoked CA certificate should no longer be used to sign device certificates\. It might have been revoked because it was compromised\. Newly added devices with certificates signed using this CA certificate might pose a security threat\. 

------
#### [ How to fix it ]

1. Use [UpdateCACertificate](https://docs.aws.amazon.com/iot/latest/developerguide/api-iot-UpdateCACertificate.html) to mark the CA certificate as INACTIVE in AWS IoT\. You can also use mitigation actions to:
   + Apply the `UPDATE_CA_CERTIFICATE` mitigation action on your audit findings to make this change\. 
   + Apply the `PUBLISH_FINDINGS_TO_SNS` mitigation action if you want to implement a custom response in response to the Amazon SNS message\. 

   For more information, see [Mitigation Actions](device-defender-mitigation-actions.md)\.

1. Review the device certificate registration activity for the time after the CA certificate was revoked and consider revoking any device certificates that might have been issued with it during this time\. \(Use [ListCertificatesByCA](https://docs.aws.amazon.com/iot/latest/developerguide/api-iot-ListCertificatesByCA.html) to list the device certificates signed by the CA certificate and [UpdateCertificate](https://docs.aws.amazon.com/iot/latest/developerguide/api-iot-UpdateCertificate.html) to revoke a device certificate\.\)

------

 

------
#### [ DEVICE\_CERTIFICATE\_SHARED\_CHECK ]

Multiple, concurrent connections use the same X\.509 certificate to authenticate with AWS IoT\.

Severity: **Critical**

------
#### [ Details ]

When this check is enabled, data collection starts immediately, but results of the check are not available for at least two hours\.

When performed as part of an on\-demand audit, this check looks at the certificates and client IDs that were used by devices to connect during the 31 days before the start of the audit\. For scheduled audits, this check looks at data from the last time the audit was run to the time this instance of the audit started\. If you have taken steps to mitigate this condition during the time checked, note when the concurrent connections were made to determine if the problem persists\.

The following reason codes are returned when this check finds a noncompliant certificate:
+ CERTIFICATE\_SHARED\_BY\_MULTIPLE\_DEVICES

In addition, the findings returned by this check include the ID of the shared certificate, the IDs of the clients using the certificate to connect, and the connect/disconnect times\. Most recent results are listed first\.

------
#### [ Why it matters ]

Each device should have a unique certificate to authenticate with AWS IoT\. When multiple devices use the same certificate, this might indicate that a device has been compromised\. Its identity might have been cloned to further compromise the system\. 

------
#### [ How to fix it ]

Verify that the device certificate has not been compromised\. If it has, follow your security best practices to mitigate the situation\. 

If you are using the same certificate on multiple devices, you might want to:

1. Provision new, unique certificates and attach them to each device\. 

1. Verify that the new certificates are valid and the devices can use them to connect\.

1. Use [UpdateCertificate](https://docs.aws.amazon.com/iot/latest/developerguide/api-iot-UpdateCertificate.html) to mark the old certificate as REVOKED in AWS IoT\. You can also use mitigation actions to:
   + Apply the `UPDATE_DEVICE_CERTIFICATE` mitigation action on your audit findings to make this change\. 
   + Apply the `ADD_THINGS_TO_THING_GROUP` mitigation action to add the device to a group where you can take action on it\.
   + Apply the `PUBLISH_FINDINGS_TO_SNS` mitigation action if you want to implement a custom response in response to the Amazon SNS message\. 

   For more information, see [Mitigation Actions](device-defender-mitigation-actions.md)\. 

1. Detach the old certificate from each of the devices\.

------

 

------
#### [ DEVICE\_CERTIFICATE\_KEY\_QUALITY\_CHECK ]

AWS IoT customers often rely on TLS mutual authentication using X\.509 certificates for authenticating to AWS IoT message broker\. These certificates and their certificate authority certificates must be registereed in their AWS IoT account before they are used\. AWS IoT performs basic sanity checks on these certificates when they are registered\. These checks include:
+ They must be in a valid format
+ They must be signed by a registered certificate authority
+ They must still be within their validity period \(in other words, they have not expired\)
+ Their cryptographic key sizes must meet a minimum required size \(for RSA keys, they must be 2048 bits or larger\)\.

This audit check provides the following additional tests of the quality of your cryptographic key:
+ CVE\-2017\-15361 \- Check whether the key was generated using OpenSSL 0\.9\.8c\-1 up to versions before 0\.9\.8g\-9 on a Debian\-based operating system\. Those versions of OpenSSL use a random number generator that generates predictable numbers, making it easier for remote attackers to conduct brute force guessing attacks against cryptographic keys\.
+ CVE\-2008\-0166 \- Check whether the key was generated by the Infineon RSA library 1\.02\.013 in Infineon Trusted Platform Module \(TPM\) firmware, such as versions before 0000000000000422 \- 4\.34, before 000000000000062b \- 6\.43, and before 0000000000008521 \- 133\.33\. That library mishandles RSA key generation, making it easier for attackers to defeat some cryptographic protection mechamisms through targeted attacks\. Examples of affected technologies include BitLocker with TPM 1\.2, YubiKey 4 \(before 4\.3\.5\) PGP key generation, and the Cached User Data encryption feature in Chrome OS\.

AWS IoT Device Defender reports certificates as noncompliant if they fail these tests\.

Severity: **Critical**

------
#### [ Details ]

This check applies to device certificates that are ACTIVE or PENDING\_TRANSFER\.

The following reason codes are returned when this check finds a noncompliant certificate:
+ CERTIFICATE\_KEY\_VULNERABILITY\_CVE\-2017\-15361

  CERTIFICATE\_KEY\_VULNERABILITY\_CVE\-2008\-0166

------
#### [ Why it matters ]

When a device uses a vulnerable certificate, attackers can more easily compromise that device\.

------
#### [ How to fix it ]

Update your device certificates to replace those with known vulnerabilities\.

If you are using the same certificate on multiple devices, you might want to:

1. Provision new, unique certificates and attach them to each device\. 

1. Verify that the new certificates are valid and the devices can use them to connect\.

1. Use [UpdateCertificate](https://docs.aws.amazon.com/iot/latest/developerguide/api-iot-UpdateCertificate.html) to mark the old certificate as REVOKED in AWS IoT\. You can also use mitigation actions to:
   + Apply the `UPDATE_DEVICE_CERTIFICATE` mitigation action on your audit findings to make this change\. 
   + Apply the `ADD_THINGS_TO_THING_GROUP` mitigation action to add the device to a group where you can take action on it\.
   + Apply the `PUBLISH_FINDINGS_TO_SNS` mitigation action if you want to implement a custom response in response to the Amazon SNS message\. 

   For more information, see [Mitigation Actions](device-defender-mitigation-actions.md)\. 

1. Detach the old certificate from each of the devices\.

------

 

------
#### [ CA\_CERTIFICATE\_KEY\_QUALITY\_CHECK ]

AWS IoT customers often rely on TLS mutual authentication using X\.509 certificates for authenticating to AWS IoT message broker\. These certificates and their certificate authority certificates must be registereed in their AWS IoT account before they are used\. AWS IoT performs basic sanity checks on these certificates when they are registered, including:
+ The certificates are in a valid format\.
+ The certificates are within their validity period \(in other words, not expired\)\.
+ Their cryptographic key sizes meet a minimum required size \(for RSA keys, they must be 2048 bits or larger\)\.

This audit check provides the following additional tests of the quality of your cryptographic key:
+ CVE\-2017\-15361 \- Check whether the key was generated using OpenSSL 0\.9\.8c\-1 up to versions before 0\.9\.8g\-9 on a Debian\-based operating system\. Those versions of OpenSSL use a random number generator that generates predictable numbers, making it easier for remote attackers to conduct brute force guessing attacks against cryptographic keys\.
+ CVE\-2008\-0166 \- Check whether the key was generated by the Infineon RSA library 1\.02\.013 in Infineon Trusted Platform Module \(TPM\) firmware, such as versions before 0000000000000422 \- 4\.34, before 000000000000062b \- 6\.43, and before 0000000000008521 \- 133\.33\. That library mishandles RSA key generation, making it easier for attackers to defeat some cryptographic protection mechamisms through targeted attacks\. Examples of affected technologies include BitLocker with TPM 1\.2, YubiKey 4 \(before 4\.3\.5\) PGP key generation, and the Cached User Data encryption feature in Chrome OS\.

AWS IoT Device Defender reports certificates as noncompliant if they fail these tests\.

Severity: **Critical**

------
#### [ Details ]

This check applies to CA certificates that are ACTIVE or PENDING\_TRANSFER\.

The following reason codes are returned when this check finds a noncompliant certificate:
+ CERTIFICATE\_KEY\_VULNERABILITY\_CVE\-2017\-15361

  CERTIFICATE\_KEY\_VULNERABILITY\_CVE\-2008\-0166

------
#### [ Why it matters ]

Newly added devices signed using this CA certificate might pose a security threat\.

------
#### [ How to fix it ]

1. Use [UpdateCACertificate](https://docs.aws.amazon.com/iot/latest/developerguide/api-iot-UpdateCACertificate.html) to mark the CA certificate as INACTIVE in AWS IoT\. You can also use mitigation actions to:
   + Apply the `UPDATE_CA_CERTIFICATE` mitigation action on your audit findings to make this change\. 
   + Apply the `PUBLISH_FINDINGS_TO_SNS` mitigation action if you want to implement a custom response in response to the Amazon SNS message\. 

   For more information, see [Mitigation Actions](device-defender-mitigation-actions.md)\.

1. Review the device certificate registration activity for the time after the CA certificate was revoked and consider revoking any device certificates that might have been issued with it during this time\. \(Use [ListCertificatesByCA](https://docs.aws.amazon.com/iot/latest/developerguide/api-iot-ListCertificatesByCA.html) to list the device certificates signed by the CA certificate and [UpdateCertificate](https://docs.aws.amazon.com/iot/latest/developerguide/api-iot-UpdateCertificate.html) to revoke a device certificate\.\)

------

 

------
#### [ UNAUTHENTICATED\_COGNITO\_ROLE\_OVERLY\_PERMISSIVE\_CHECK ]

A policy attached to an unauthenticated Amazon Cognito identity pool role is considered too permissive because it grants permission to perform any of the following AWS IoT actions:
+ Manage or modify things\.
+ Read thing administrative data\.
+ Manage non\-thing related data or resources\.

Or, because it grants permission to perform the following AWS IoT actions on a broad set of devices:
+ Use MQTT to connect, publish, or subscribe to reserved topics \(including shadow or job execution data\)\.
+ Use API commands to read or modify shadow or job execution data\.

In general, devices that connect using an unauthenticated Amazon Cognito identity pool role should have only limited permission to publish and subscribe to thing\-specific MQTT topics or use the API commands to read and modify thing\-specific data related to shadow or job execution data\.

Severity: **Critical**

------
#### [ Manage or modify things ]

The following AWS IoT API actions are used to manage or modify things\. Permission to perform these actions should not be granted to devices that connect through an unauthenticated Amazon Cognito identity pool\.
+ `AddThingToThingGroup` 
+ `AttachThingPrincipal` 
+ `CreateThing` 
+ `DeleteThing` 
+ `DetachThingPrincipal` 
+ `ListThings` 
+ `ListThingsInThingGroup` 
+ `RegisterThing` 
+ `RemoveThingFromThingGroup` 
+ `UpdateThing` 
+ `UpdateThingGroupsForThing` 

Any role that grants permission to perform these actions on even a single resource is considered noncompliant\.

------
#### [ Read thing administrative data ]

The following AWS IoT API actions are used to read or modify thing data\. Devices that connect through an unauthenticated Amazon Cognito identity pool should not be given permission to perform these actions\.
+ `DescribeThing`
+ `ListJobExecutionsForThing`
+ `ListThingGroupsForThing`
+ `ListThingPrincipals`

**Example:**
+ noncompliant:

  ```
  {
    "Version": "2012-10-17",
    "Statement": [
      {
        "Effect": "Allow",
        "Action": [ 
            "iot:DescribeThing",
            "iot:ListJobExecutionsForThing",
            "iot:ListThingGroupsForThing",
            "iot:ListThingPrincipals"
        ],
        "Resource": [
          "arn:aws:iot:<region>:<account-id>:/thing/MyThing"
        ]
      }
    ]
  }
  ```

  This allows the device to perform the specified actions even though it is granted for one thing only\.

------
#### [ Manage non\-things ]

Devices that connect through an unauthenticated Amazon Cognito identity pool should not be given permission to perform AWS IoT API actions other than those discussed in these sections\. To manage your account with an application that connects through an unauthenticated Amazon Cognito identity pool, create a separate identity pool not used by devices\.

------
#### [ Subscribe/publish to MQTT topics ]

MQTT messages are sent through the AWS IoT message broker and are used by devices to perform many actions, including accessing and modifying shadow state and job execution state\. A policy that grants permission to a device to connect, publish, or subscribe to MQTT messages should restrict these actions to specific resources as follows:

Connect  
+ noncompliant:

  ```
  arn:aws:iot:<region>:<account-id>:client/*
  ```

  The wildcard \* allows any device to connect to AWS IoT\.

  ```
  arn:aws:iot:<region>:<account-id>:client/${iot:ClientId}
  ```

  Unless `iot:Connection.Thing.IsAttached` is set to true in the condition keys, this is equivalent to the wildcard \* in the previous example\.
+ compliant:

  ```
  {
    "Version": "2012-10-17",
    "Statement": [
      {
        "Effect": "Allow",
        "Action": [ "iot:Connect" ],
        "Resource": [
          "arn:aws:iot:<region>:<account-id>:client/${iot:Connection.Thing.ThingName}"
        ],
        "Condition": {
          "Bool": { "iot:Connection.Thing.IsAttached": "true" }
        }
      }
    ]
  }
  ```

  The resource specification contains a variable that matches the device name used to connect\. The condition statement further restricts the permission by checking that the certificate used by the MQTT client matches that attached to the thing with the name used\.

Publish  
+ noncompliant:

  ```
  arn:aws:iot:<region>:<account-id>:topic/$aws/things/*/shadow/update
  ```

  This allows the device to update the shadow of any device \(\* = all devices\)\.

  ```
  arn:aws:iot:<region>:<account-id>:topic/$aws/things/*
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
          "arn:aws:iot:<region>:<account-id>:topic/$aws/things/${iot:Connection.Thing.ThingName}/shadow/*"
        ],
      }
    ]
  }
  ```

  The resource specification contains a wildcard, but it only matches any shadow\-related topic for the device whose thing name is used to connect\.

Subscribe  
+ noncompliant:

  ```
  arn:aws:iot:<region>:<account-id>:topicfilter/$aws/things/*
  ```

  This allows the device to subscribe to reserved shadow or job topics for all devices\.

  ```
  arn:aws:iot:<region>:<account-id>:topicfilter/$aws/things/*
  ```

  The same as the previous example, but using the \# wildcard\.

  ```
  arn:aws:iot:<region>:<account-id>:topic/$aws/things/+/shadow/update
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
          "arn:aws:iot:<region>:<account-id>:topicfilter/$aws/things/${iot:Connection.Thing.ThingName}/shadow/*"
          "arn:aws:iot:<region>:<account-id>:topicfilter/$aws/things/${iot:Connection.Thing.ThingName}/jobs/*"
        ],
      }
    ]
  }
  ```

  The resource specifications contain wildcards, but they only match any shadow\-related topic and any job\-related topic for the device whose thing name is used to connect\.

Receive  
+ compliant:

  ```
  arn:aws:iot:<region>:<account-id>:topicfilter/$aws/things/*
  ```

  This is allowed because the device can receive messages only from topics on which it has permission to subscribe\.

------
#### [ Read/modify shadow or job data ]

A policy that grants permission to a device to perform an API action to access or modify device shadows or job execution data should restrict these actions to specific resources\. The API actions are:
+ `DeleteThingShadow`
+ `GetThingShadow`
+ `UpdateThingShadow`
+ `DescribeJobExecution`
+ `GetPendingJobExecutions`
+ `StartNextPendingJobExecutio`n
+ `UpdateJobExecution`

**Examples:**
+ noncompliant:

  ```
  arn:aws:iot:<region>:<account-id>:thing/*
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
          "arn:aws:iot:<region>:<account-id>:/thing/MyThing1",
          "arn:aws:iot:<region>:<account-id>:/thing/MyThing2"
        ]
      }
    ]
  }
  ```

  This allows the device to perform the specified actions on two things only\.

------
#### [ Details ]

For this check, AWS IoT Device Defender audits all Amazon Cognito identity pools that have been used to connect to the AWS IoT message broker during the 31 days before the audit execution\. All Amazon Cognito identity pools from which either an authenticated or unauthenticated Amazon Cognito identity connected are included in the audit\.

The following reason codes are returned when this check finds a noncompliant unauthenticated Amazon Cognito identity pool role:
+ ALLOWS\_ACCESS\_TO\_IOT\_ADMIN\_ACTIONS
+ ALLOWS\_BROAD\_ACCESS\_TO\_IOT\_DATA\_PLANE\_ACTIONS

------
#### [ Why it matters ]

Because unauthenticated identities are never authenticated by the user, they pose a much greater risk than authenticated Amazon Cognito identities\. If an unauthenticated identity is compromised, it can use administrative actions to modify account settings, delete resources, or gain access to sensitive data\. Or, with broad access to device settings, it can access or modify shadows and jobs for all devices in your account\. A guest user might use the permissions to compromise your entire fleet or launch a DDOS attack with messages\.

------
#### [ How to fix it ]

A policy attached to an unauthenticated Amazon Cognito identity pool role should grant only those permissions required for a device to do its job\. We recommend the following steps:

1. Create a new compliant role\.

1. Create a Amazon Cognito identity pool and attach the compliant role to it\.

1. Verify that your identities can access AWS IoT using the new pool\.

1. After verification is complete, attach the compliant role to the Amazon Cognito identity pool that was flagged as noncompliant\.

You can also use mitigation actions to:
+ Apply the `PUBLISH_FINDINGS_TO_SNS` mitigation action if you want to implement a custom response in response to the Amazon SNS message\. 

For more information, see [Mitigation Actions](device-defender-mitigation-actions.md)\. 

------

 

------
#### [ AUTHENTICATED\_COGNITO\_ROLE\_OVERLY\_PERMISSIVE\_CHECK ]

A policy attached to an authenticated Amazon Cognito identity pool role is considered too permissive because it grants permission to perform the following AWS IoT actions:
+ Manage or modify things\.
+ Manage non\-thing related data or resources\.

Or, because it grants permission to perform the following AWS IoT actions on a broad set of devices:
+ Read thing administrative data\.
+ Use MQTT to connect/publish/subscribe to reserved topics \(including shadow or job execution data\)\.
+ Use API commands to read or modify shadow or job execution data\.

In general, devices that connect using an authenticated Amazon Cognito identity pool role should have only limited permission to read thing\-specific administrative data, publish and subscribe to thing\-specific MQTT topics, or use the API commands to read and modify thing\-specific data related to shadow or job execution data\.

Severity: **Critical**

------
#### [ Manage or modify things ]

The following AWS IoT API actions are used to manage or modify things so permission to perform these should not be granted to devices connecting through an authenticated Amazon Cognito identity pool:
+ `AddThingToThingGroup` 
+ `AttachThingPrincipal` 
+ `CreateThing` 
+ `DeleteThing` 
+ `DetachThingPrincipal` 
+ `ListThings`
+ `ListThingsInThingGroup` 
+ `RegisterThing` 
+ `RemoveThingFromThingGroup` 
+ `UpdateThing` 
+ `UpdateThingGroupsForThing`

Any role that grants permission to perform these actions on even a single resource is considered noncompliant\.

------
#### [ Manage non\-things ]

Devices that connect through an authenticated Amazon Cognito identity pool should not be given permission to perform AWS IoT API actions other than those discussed in these sections\. To manage your account with an application that connects through an authenticated Amazon Cognito identity pool, create a separate identity pool not used by devices\.

------
#### [ Read thing administrative data ]

The following AWS IoT API actions are used to read thing data, so devices that connect through an authenticated Amazon Cognito identity pool should be given permission to perform these on a limited set of things only:
+ `DescribeThing`
+ `ListJobExecutionsForThing`
+ `ListThingGroupsForThing`
+ `ListThingPrincipals`

**Examples:**
+ noncompliant:

  ```
  arn:aws:iot:<region>:<account-id>:thing/*
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
            "iot:DescribeThing",
            "iot:ListJobExecutionsForThing",
            "iot:ListThingGroupsForThing",
            "iot:ListThingPrincipals"
        ],
        "Resource": [
          "arn:aws:iot:<region>:<account-id>:/thing/MyThing"
        ]
      }
    ]
  }
  ```

  This allows the device to perform the specified actions on only one thing\.
+ compliant:

  ```
  {
    "Version": "2012-10-17",
    "Statement": [
      {
        "Effect": "Allow",
        "Action": [ 
            "iot:DescribeThing",
            "iot:ListJobExecutionsForThing",
            "iot:ListThingGroupsForThing",
            "iot:ListThingPrincipals"
        ],
        "Resource": [
          "arn:aws:iot:<region>:<account-id>:/thing/MyThing*"
        ]
      }
    ]
  }
  ```

  This is compliant because, although the resource is specified with a wildcard \(\*\), it is preceded by a specific string, and that limits the set of things accessed to those with names that have the given prefix\.

------
#### [ Subscribe/publish to MQTT topics ]

MQTT messages are sent through the AWS IoT message broker and are used by devices to perform many different actions, including accessing and modifying shadow state and job execution state\. A policy that grants permission to a device to connect, publish, or subscribe to MQTT messages should restrict these actions to specific resources as follows:

Connect  
+ noncompliant:

  ```
  arn:aws:iot:<region>:<account-id>:client/*
  ```

  The wildcard \* allows any device to connect to AWS IoT\.

  ```
  arn:aws:iot:<region>:<account-id>:client/${iot:ClientId}
  ```

  Unless `iot:Connection.Thing.IsAttached` is set to true in the condition keys, this is equivalent to the wildcard \* in the previous example\.
+ compliant:

  ```
  {
    "Version": "2012-10-17",
    "Statement": [
      {
        "Effect": "Allow",
        "Action": [ "iot:Connect" ],
        "Resource": [
          "arn:aws:iot:<region>:<account-id>:client/${iot:Connection.Thing.ThingName}"
        ],
        "Condition": {
          "Bool": { "iot:Connection.Thing.IsAttached": "true" }
        }
      }
    ]
  }
  ```

  The resource specification contains a variable that matches the device name used to connect, and the condition statement further restricts the permission by checking that the certificate used by the MQTT client matches that attached to the thing with the name used\.

Publish  
+ noncompliant:

  ```
  arn:aws:iot:<region>:<account-id>:topic/$aws/things/*/shadow/update
  ```

  This allows the device to update the shadow of any device \(\* = all devices\)\.

  ```
  arn:aws:iot:<region>:<account-id>:topic/$aws/things/*
  ```

  This allows the device to read/update/delete the shadow of any device\.
+ compliant:

  ```
  {
    "Version": "2012-10-17",
    "Statement": [
      {
        "Effect": "Allow",
        "Action": [ "iot:Publish" ],
        "Resource": [
          "arn:aws:iot:<region>:<account-id>:topic/$aws/things/${iot:Connection.Thing.ThingName}/shadow/*"
        ],
      }
    ]
  }
  ```

  The resource specification contains a wildcard, but it only matches any shadow\-related topic for the device whose thing name is used to connect\.

Subscribe  
+ noncompliant:

  ```
  arn:aws:iot:<region>:<account-id>:topicfilter/$aws/things/*
  ```

  This allows the device to subscribe to reserved shadow or job topics for all devices\.

  ```
  arn:aws:iot:<region>:<account-id>:topicfilter/$aws/things/#
  ```

  The same as the previous example, but using the \# wildcard\.

  ```
  arn:aws:iot:<region>:<account-id>:topic/$aws/things/+/shadow/update
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
          "arn:aws:iot:<region>:<account-id>:topicfilter/$aws/things/${iot:Connection.Thing.ThingName}/shadow/*"
          "arn:aws:iot:<region>:<account-id>:topicfilter/$aws/things/${iot:Connection.Thing.ThingName}/jobs/*"
        ],
      }
    ]
  }
  ```

  The resource specifications contain wildcards, but they only match any shadow\-related topic and any job\-related topic for the device whose thing name is used to connect\.

Receive  
+ compliant:

  ```
  arn:aws:iot:<region>:<account-id>:topicfilter/$aws/things/*
  ```

  This is compliant because the device can receive messages only from topics on which it has permission to subscribe\.

------
#### [ Read or modify shadow or job data ]

A policy that grants permission to a device to perform an API action to access or modify device shadows or job execution data should restrict these actions to specific resources\. The API actions are:
+ `DeleteThingShadow`
+ `GetThingShadow`
+ `UpdateThingShadow`
+ `DescribeJobExecution`
+ `GetPendingJobExecutions`
+ `StartNextPendingJobExecution`
+ `UpdateJobExecution`

**Examples:**
+ noncompliant:

  ```
  arn:aws:iot:<region>:<account-id>:thing/*
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
          "arn:aws:iot:<region>:<account-id>:/thing/MyThing1",
          "arn:aws:iot:<region>:<account-id>:/thing/MyThing2"
        ]
      }
    ]
  }
  ```

  This allows the device to perform the specified actions on only two things\.

------
#### [ Details ]

For this check, AWS IoT Device Defender audits all Amazon Cognito identity pools that have been used to connect to the AWS IoT message broker during the 31 days before the audit execution\. All Amazon Cognito identity pools from which either an authenticated or unauthenticated Amazon Cognito identity connected are included in the audit\.

The following reason codes are returned when this check finds a noncompliant authenticated Amazon Cognito identity pool role:
+ ALLOWS\_BROAD\_ACCESS\_TO\_IOT\_THING\_ADMIN\_READ\_ACTIONS
+ ALLOWS\_ACCESS\_TO\_IOT\_NON\_THING\_ADMIN\_ACTIONS
+ ALLOWS\_ACCESS\_TO\_IOT\_THING\_ADMIN\_WRITE\_ACTIONS

------
#### [ Why it matters ]

If an authenticated identity is compromised, it can use administrative actions to modify account settings, delete resources, or gain access to sensitive data\.

------
#### [ How to fix it ]

A policy attached to an authenticated Amazon Cognito identity pool role should grant only those permissions required for a device to do its job\. We recommend the following steps:

1. Create a new compliant role\.

1. Create a Amazon Cognito identity pool and attach the compliant role to it\.

1. Verify that your identities can access AWS IoT using the new pool\.

1. After verification is complete, attach the role to the Amazon Cognito identity pool that was flagged as noncompliant\.

You can also use mitigation actions to:
+ Apply the `PUBLISH_FINDINGS_TO_SNS` mitigation action if you want to implement a custom response in response to the Amazon SNS message\. 

For more information, see [Mitigation Actions](device-defender-mitigation-actions.md)\. 

------

 

------
#### [ IOT\_POLICY\_OVERLY\_PERMISSIVE\_CHECK ]

An AWS IoT policy gives permissions that are too broad or unrestricted\. It grants permission to send or receive MQTT messages for a broad set of devices, or grants permission to access or modify shadow and job execution data for a broad set of devices\.  

In general, a policy for a device should grant access to resources associated with just that device and no or very few other devices\. With some exceptions, using a wildcard \(for example, "\*"\) to specify resources in such a policy is considered too broad or unrestricted\.

Severity: **Critical**

------
#### [ MQTT permissions ]

MQTT messages are sent through the AWS IoT message broker and are used by devices to perform many actions, including accessing and modifying shadow state and job execution state\. A policy that grants permission to a device to connect, publish, or subscribe to MQTT messages should restrict these actions to specific resources as follows:

Connect  
+ noncompliant:

  ```
  arn:aws:iot:<region>:<account-id>:client/*
  ```

  The wildcard \* allows any device to connect to AWS IoT\.

  ```
  arn:aws:iot:<region>:<account-id>:client/${iot:ClientId}
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
          "arn:aws:iot:<region>:<account-id>:client/${iot:Connection.Thing.ThingName}"
        ],
        "Condition": {
          "Bool": { "iot:Connection.Thing.IsAttached": "true" }
        }
      }
    ]
  }
  ```

  The resource specification contains a variable that matches the device name used to connect\. The condition statement further restricts the permission by checking that the certificate used by the MQTT client matches that attached to the thing with the name used\.

Publish  
+ noncompliant:

  ```
  arn:aws:iot:<region>:<account-id>:topic/$aws/things/*/shadow/update
  ```

  This allows the device to update the shadow of any device \(\* = all devices\)\.

  ```
  arn:aws:iot:<region>:<account-id>:topic/$aws/things/*
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
          "arn:aws:iot:<region>:<account-id>:topic/$aws/things/${iot:Connection.Thing.ThingName}/shadow/*"
        ],
      }
    ]
  }
  ```

  The resource specification contains a wildcard, but it only matches any shadow\-related topic for the device whose thing name is used to connect\.

Subscribe  
+ noncompliant:

  ```
  arn:aws:iot:<region>:<account-id>:topicfilter/$aws/things/*
  ```

  This allows the device to subscribe to reserved shadow or job topics for all devices\.

  ```
  arn:aws:iot:<region>:<account-id>:topicfilter/$aws/things/*
  ```

  The same as the previous example, but using the \# wildcard\.

  ```
  arn:aws:iot:<region>:<account-id>:topic/$aws/things/+/shadow/update
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
          "arn:aws:iot:<region>:<account-id>:topicfilter/$aws/things/${iot:Connection.Thing.ThingName}/shadow/*"
          "arn:aws:iot:<region>:<account-id>:topicfilter/$aws/things/${iot:Connection.Thing.ThingName}/jobs/*"
        ],
      }
    ]
  }
  ```

  The resource specifications contain wildcards, but they only match any shadow\-related topic and any job\-related topic for the device whose thing name is used to connect\.

Receive  
+ compliant:

  ```
  arn:aws:iot:<region>:<account-id>:topicfilter/$aws/things/*
  ```

  This is compliant because the device can only receive messages from topics on which it has permission to subscribe\.

------
#### [ Shadow and job permissions ]

A policy that grants permission to a device to perform an API action to access or modify device shadows or job execution data should restrict these actions to specific resources\. The API actions are:
+ `DeleteThingShadow`
+ `GetThingShado`w
+ `UpdateThingShado`w
+ `DescribeJobExecution`
+ `GetPendingJobExecutions`
+ `StartNextPendingJobExecution`
+ `UpdateJobExecution`

**Examples:**
+ noncompliant:

  ```
  arn:aws:iot:<region>:<account-id>:thing/*
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
          "arn:aws:iot:<region>:<account-id>:/thing/MyThing1",
          "arn:aws:iot:<region>:<account-id>:/thing/MyThing2"
        ]
      }
    ]
  }
  ```

  This allows the device to perform the specified actions on only two things\.

------
#### [ Details ]

The following reason code is returned when this check finds a noncompliant IoT policy:
+ ALLOWS\_BROAD\_ACCESS\_TO\_IOT\_DATA\_PLANE\_ACTIONS

------
#### [ Why it matters ]

A certificate, Amazon Cognito identity, or thing group with an overly permissive policy can, if compromised, impact the security of your entire account\. An attacker could use such broad access to read or modify shadows, jobs, or job executions for all your devices\. Or an attacker could use a compromised certificate to connect malicious devices or launch a DDOS attack on your network\.

------
#### [ How to fix it ]

Follow these steps to fix any noncompliant policies attached to things, thing groups, or other entities:

1. Use [CreatePolicyVersion](https://docs.aws.amazon.com/iot/latest/apireference/API_CreatePolicyVersion.html) to create a new, compliant version of the policy\. Set the `setAsDefault` flag to true\. \(This makes this new version operative for all entities that use the policy\.\)

1. Use [ListTargetsForPolicy](https://docs.aws.amazon.com/iot/latest/apireference/API_ListTargetsForPolicy.html) to get a list of targets \(certificates, thing groups\) that the policy is attached to and determine which devices are included in the groups or which use the certificates to connect\.

1. Verify that all associated devices are able to connect to AWS IoT\. If a device is unable to connect, use [ SetPolicyVersion](https://docs.aws.amazon.com/iot/latest/apireference/API_SetPolicyVersion.html) to roll back the default policy to the previous version, revise the policy, and try again\. 

You can use mitigation actions to:
+ Apply the `REPLACE_DEFAULT_POLICY_VERSION` mitigation action on your audit findings to make this change\. 
+ Apply the `PUBLISH_FINDINGS_TO_SNS` mitigation action if you want to implement a custom response in response to the Amazon SNS message\. 

For more information, see [Mitigation Actions](device-defender-mitigation-actions.md)\. 

Use [AWS IoT policy variables](https://docs.aws.amazon.com/iot/latest/developerguide/policy-variables.html) to dynamically reference AWS IoT resources in your policies\.

------

 

------
#### [ IOT\_ROLE\_ALIAS\_OVERLY\_PERMISSIVE\_CHECK ]

AWS IoT role alias provides a mechanism for connected devices to authenticate to AWS IoT using X\.509 certificates and then obtain short\-lived AWS credentials from an IAM role that is associated with an AWS IoT role alias\. The permissions for these credentials must be scoped down using access policies with authentication context variables\. If you misconfigure your policies, you could leave yourself exposed to an escalation of privilege attack\. This audit check ensures that the temporary credentials provided by AWS IoT role aliases are not overly permissive\. 

This check is triggered if one of the following conditions are found:
+ The policy provides administrative permissions to any services used in the past year by this role alias \(for example, "iot:\*", "dynamodb:\*", "iam:\*", and so on\)\.
+ The policy provides broad access to thing metadata actions, access to restricted AWS IoT actions, or broad access to AWS IoT data plane actions\.
+ The policy provides access to security auditing services such as "iam", "cloudtrail", "guardduty", "inspector", or "trustedadvisor"\.

Severity: **Critical**

------
#### [ Details ]

The following reason codes are returned when this check finds a noncompliant IoT policy:
+ ALLOWS\_BROAD\_ACCESS\_TO\_USED\_SERVICES
+ ALLOWS\_ACCESS\_TO\_SECURITY\_AUDITING\_SERVICES
+ ALLOWS\_BROAD\_ACCESS\_TO\_IOT\_THING\_ADMIN\_READ\_ACTIONS
+ ALLOWS\_ACCESS\_TO\_IOT\_NON\_THING\_ADMIN\_ACTIONS
+ ALLOWS\_ACCESS\_TO\_IOT\_THING\_ADMIN\_WRITE\_ACTIONS
+ ALLOWS\_BROAD\_ACCESS\_TO\_IOT\_DATA\_PLANE\_ACTIONS

------
#### [ Why it matters ]

By limiting permissions to those that are required for a device to perform its normal operations, you reduce the risks to your account if a device is compromised\.

------
#### [ How to fix it ]

Follow these steps to fix any noncompliant policies attached to things, thing groups, or other entities:

1. Follow the steps in [Authorizing Direct Calls to AWS Services](authorizing-direct-aws.md) to apply a more restrictive policy to your role alias\.

You can use mitigation actions to:
+ Apply the `PUBLISH_FINDINGS_TO_SNS` mitigation action if you want to implement a custom action in response to the Amazon SNS message\. 

For more information, see [Mitigation Actions](device-defender-mitigation-actions.md)\. 

------

 

------
#### [ IOT\_ROLE\_ALIAS\_ALLOWS\_ACCESS\_TO\_UNUSED\_SERVICES\_CHECK ]

AWS IoT role alias provides a mechanism for connected devices to authenticate to AWS IoT using X\.509 certificates and then obtain short\-lived AWS credentials from an IAM role that is associated with an AWS IoT role alias\. The permissions for these credentials must be scoped down using access policies with authentication context variables\. If you misconfigure your policies, you could leave yourself exposed to an escalation of privilege attack\. This audit check ensures that the temporary credentials provided by AWS IoT role aliases are not overly permissive\. 

This check is triggered if the role alias has access to services that haven't been used for the AWS IoT device in the last year\. For example, the audit reports if you have an IAM role linked to the role alias that has only used AWS IoT in the past year but the policy attached to the role also grants permission to `"iam:getRole"` and `"dynamodb:PutItem"`\.

Severity: **Medium**

------
#### [ Details ]

The following reason codes are returned when this check finds a noncompliant AWS IoT policy:
+ ALLOWS\_ACCESS\_TO\_UNUSED\_SERVICES

------
#### [ Why it matters ]

By limiting permissions to those services that are required for a device to perform its normal operations, you reduce the risks to your account if a device is compromised\.

------
#### [ How to fix it ]

Follow these steps to fix any noncompliant policies attached to things, thing groups, or other entities:

1. Follow the steps in [Authorizing Direct Calls to AWS Services](authorizing-direct-aws.md) to apply a more restrictive policy to your role alias\.

You can use mitigation actions to:
+ Apply the `PUBLISH_FINDINGS_TO_SNS` mitigation action if you want to implement a custom action in response to the Amazon SNS message\. 

For more information, see [Mitigation Actions](device-defender-mitigation-actions.md)\. 

------

 

------
#### [ CA\_CERT\_APPROACHING\_EXPIRATION\_CHECK ]

A CA certificate is expiring within 30 days or has expired\.

Severity: **Medium**

------
#### [ Details ]

This check applies to CA certificates that are ACTIVE or PENDING\_TRANSFER\.

The following reason codes are returned when this check finds a noncompliant CA certificate:
+ CERTIFICATE\_APPROACHING\_EXPIRATION
+ CERTIFICATE\_PAST\_EXPIRATION

------
#### [ Why it matters ]

An expired CA certificate should not be used to sign new device certificates\.

------
#### [ How to fix it ]

Consult your security best practices for how to proceed\. You might want to:

1. Register a new CA certificate with AWS IoT\.

1. Verify that you are able to sign device certificates using the new CA certificate\.

1. Use [UpdateCACertificate](https://docs.aws.amazon.com/iot/latest/developerguide/api-iot-UpdateCACertificate.html) to mark the old CA certificate as INACTIVE in AWS IoT\. You can also use mitigation actions to:
   + Apply the `UPDATE_CA_CERTIFICATE` mitigation action on your audit findings to make this change\. 
   + Apply the `PUBLISH_FINDINGS_TO_SNS` mitigation action if you want to implement a custom response in response to the Amazon SNS message\. 

   For more information, see [Mitigation Actions](device-defender-mitigation-actions.md)\. 

------

 

------
#### [ CONFLICTING\_CLIENT\_IDS\_CHECK ]

Multiple devices connect using the same client ID\.

Severity: **High**

------
#### [ Details ]

Multiple connections were made using the same client ID, causing an already connected device to be disconnected\. The MQTT specification allows only one active connection per client ID, so when another device connects using the same client ID, it knocks the previous one off the connection\.

When performed as part of an on\-demand audit, this check looks at how client IDs were used to connect during the 31 days prior to the start of the audit\. For scheduled audits, this check looks at data from the last time the audit was run to the time this instance of the audit started\. If you have taken steps to mitigate this condition during the time checked, note when the connections/disconnections were made to determine if the problem persists\.

The following reason codes are returned when this check finds noncompliance:
+ DUPLICATE\_CLIENT\_ID\_ACROSS\_CONNECTIONS

The findings returned by this check also include the client ID used to connect, principal IDs, and disconnect times\. The most recent results are listed first\.

------
#### [ Why it matters ]

Devices with conflicting IDs are forced to constantly reconnect, which might result in lost messages or leave a device unable to connect\.

This might indicate that a device or a device's credentials have been compromised, and might be part of a DDoS attack\. It is also possible that devices are not configured correctly in the account or a device has a bad connection and is forced to reconnect several times per minute\.

------
#### [ How to fix it ]

Register each device as a unique thing in AWS IoT, and use the thing name as the client ID to connect\. Or use a UUID as the client ID when connecting the device over MQTT\. You can also use mitigation actions to:
+ Apply the `PUBLISH_FINDINGS_TO_SNS` mitigation action if you want to implement a custom response in response to the Amazon SNS message\. 

For more information, see [Mitigation Actions](device-defender-mitigation-actions.md)\.

------

 

------
#### [ DEVICE\_CERT\_APPROACHING\_EXPIRATION\_CHECK ]

A device certificate is expiring within 30 days or has expired\.

Severity: **Medium**

------
#### [ Details ]

This check applies to device certificates that are ACTIVE or PENDING\_TRANSFER\.

The following reason codes are returned when this check finds a noncompliant device certificate:
+ CERTIFICATE\_APPROACHING\_EXPIRATION
+ CERTIFICATE\_PAST\_EXPIRATION

------
#### [ Why it matters ]

A device certificate should not be used after it expires\.

------
#### [ How to fix it ]

Consult your security best practices for how to proceed\. You might want to:

1. Provision a new certificate and attach it to the device\. 

1. Verify that the new certificate is valid and the device is able to use it to connect\.

1. Use [UpdateCertificate](https://docs.aws.amazon.com/iot/latest/developerguide/api-iot-UpdateCertificate.html) to mark the old certificate as INACTIVE in AWS IoT\. You can also use mitigation actions to:
   + Apply the `UPDATE_DEVICE_CERTIFICATE` mitigation action on your audit findings to make this change\. 
   + Apply the `ADD_THINGS_TO_THING_GROUP` mitigation action to add the device to a group where you can take action on it\.
   + Apply the `PUBLISH_FINDINGS_TO_SNS` mitigation action if you want to implement a custom response in response to the Amazon SNS message\. 

   For more information, see [Mitigation Actions](device-defender-mitigation-actions.md)\. 

1. Detach the old certificate from the device\. \(See [DetachThingPrincipal](https://docs.aws.amazon.com/iot/latest/developerguide/api-iot-DetachThingPrincipal.html)\.\)

------

 

------
#### [ REVOKED\_DEVICE\_CERT\_CHECK ]

A revoked device certificate is still active\.

Severity: **Medium**

------
#### [ Details ]

A device certificate is in its CA's [certificate revocation list](https://en.wikipedia.org/wiki/Certificate_revocation_list), but it is still active in AWS IoT\.

This check applies to device certificates that are ACTIVE or PENDING\_TRANSFER\.

The following reason codes are returned when this check finds noncompliance:
+ CERTIFICATE\_REVOKED\_BY\_ISSUER

------
#### [ Why it matters ]

A device certificate is usually revoked because it has been compromised\. It is possible that it has not yet been revoked in AWS IoT due to an error or oversight\.

------
#### [ How to fix it ]

Verify that the device certificate has not been compromised\. If it has, follow your security best practices to mitigate the situation\. You might want to:

1. Provision a new certificate for the device\.

1. Verify that the new certificate is valid and the device is able to use it to connect\.

1. Use [UpdateCertificate](https://docs.aws.amazon.com/iot/latest/developerguide/api-iot-UpdateCertificate.html) to mark the old certificate as REVOKED in AWS IoT\. You can also use mitigation actions to:
   + Apply the `UPDATE_DEVICE_CERTIFICATE` mitigation action on your audit findings to make this change\. 
   + Apply the `ADD_THINGS_TO_THING_GROUP` mitigation action to add the device to a group where you can take action on it\.
   + Apply the `PUBLISH_FINDINGS_TO_SNS` mitigation action if you want to implement a custom response in response to the Amazon SNS message\. 

   For more information, see [Mitigation Actions](device-defender-mitigation-actions.md)\. 

1. Detach the old certificate from the device\. \(See [DetachThingPrincipal](https://docs.aws.amazon.com/iot/latest/developerguide/api-iot-DetachThingPrincipal.html)\.\)

------

 

------
#### [ LOGGING\_DISABLED\_CHECK ]

AWS IoT logs are not enabled in CloudWatch\.

Severity: **Low**

------
#### [ Details ]

The following reason codes are returned when this check finds noncompliance:
+ LOGGING\_DISABLED

------
#### [ Why it matters ]

AWS IoT logs in CloudWatch provide visibility into behaviors in AWS IoT, including authentication failures and unexpected connects and disconnects that might indicate that a device has been compromised\.

------
#### [ How to fix it ]

Enable AWS IoT logs in CloudWatch\. See [Monitoring Tools](https://docs.aws.amazon.com/iot/latest/developerguide/monitoring_automated_manual.html)\. You can also use mitigation actions to:
+ Apply the `ENABLE_IOT_LOGGING` mitigation action on your audit findings to make this change\. 
+ Apply the `PUBLISH_FINDINGS_TO_SNS` mitigation action if you want to implement a custom response in response to the Amazon SNS message\. 

For more information, see [Mitigation Actions](device-defender-mitigation-actions.md)\. 

------

## How to Perform Audits<a name="device-defender-HowToProceed"></a>

1. Configure audit settings for your account\. Use [ UpdateAccountAuditConfiguration](AuditCommands.md#dd-api-iot-UpdateAccountAuditConfiguration) to enable those checks you want to be available for audits, set up optional notifications, and configure permissions\. 

   For some checks, AWS IoT starts collecting data as soon as the check is enabled\.

1. Create one or more audit schedules\. Use [ CreateScheduledAudit](AuditCommands.md#dd-api-iot-CreateScheduledAudit) to specify the checks you want to perform during an audit and how often these audits should be run\. 

   Or, you can run an on\-demand audit when necessary\. Use [ StartOnDemandAuditTask](AuditCommands.md#dd-api-iot-StartOnDemandAuditTask) to specify the checks you want to perform and start an audit running right away\. \(Results might not be ready for some time if you have recently enabled a check that is included in the on\-demand audit\.\)

1. You can use the [ AWS IoT console](https://console.aws.amazon.com/iot/home) to view the results of your audits\.

   Or, you can see the results of your audits with [ ListAuditFindings](AuditCommands.md#dd-api-iot-ListAuditFindings)\. With this command, you can filter the results by the type of check, a specific resource, or the time of the audit\. You can use this information to mitigate any problems found\.

1. You can apply mitigation actions that you defined in your AWS account to any noncompliant findings\. For more information, see [Apply Mitigation Actions](device-defender-mitigation-actions.md#defender-audit-apply-mitigation-actions)\.

## Notifications<a name="device-defender-audit-notifications"></a>

When an audit is completed, an SNS notification can be sent with a summary of the results of each audit check that was performed, including details about the number of noncompliant resources that were found\. Use the `auditNotificationTargetConfigurations` field in the input to the [ UpdateAccountAuditConfiguration](AuditCommands.md#dd-api-iot-UpdateAccountAuditConfiguration) command\. The SNS notification has the following payload:

### payload example<a name="device-defender-audit-notification-payload"></a>

```
{
    "accountId": "123456789012",
    "taskId": "4e2bcd1ccbc2a5dd15292a82ab80c380",
    "taskStatus": "FAILED|CANCELED|COMPLETED",
    "taskType": "ON_DEMAND_AUDIT_TASK|SCHEDULED_AUDIT_TASK",
    "scheduledAuditName": "myWeeklyAudit",
    "failedChecksCount": 0,
    "canceledChecksCount": 0,
    "nonCompliantChecksCount": 1,
    "compliantChecksCount": 0,
    "totalChecksCount": 1,
    "taskStartTime": 1524740766191,
    "auditDetails": [
        {
            "checkName": "DEVICE_CERT_APPROACHING_EXPIRATION_CHECK |
                          REVOKED_DEVICE_CERT_CHECK |
                          CA_CERT_APPROACHING_EXPIRATION_CHECK |
                          REVOKED_CA_CERT_CHECK |
                          DEVICE_CERTIFICATE_SHARED_CHECK |
                          IOT_POLICY_UNRESTRICTED_CHECK |
                          UNAUTHENTICATED_COGNITO_IDENTITY_UNRESTRICTED_ACCESS_CHECK |
                          AUTHENTICATED_COGNITO_IDENTITY_UNRESTRICTED_ACCESS_CHECK |
                          CONFLICTING_CLIENT_IDS_CHECK |
                          LOGGING_DISABLED_CHECK",
            
            "checkRunStatus": "FAILED |
                               CANCELED |
                               COMPLETED_COMPLIANT |
                               COMPLETED_NON_COMPLIANT",
            
            "nonCompliantResourcesCount": 1,
            "totalResourcesCount": 1,
            
            "message": "optional message if an error occurred",
            "errorCode": "INSUFFICIENT_PERMISSIONS|AUDIT_CHECK_DISABLED"
        }
    ]
}
```

### payload JSON schema<a name="device-defender-audit-notification-payload-schema"></a>

```
{
    "$schema": "http://json-schema.org/draft-07/schema#",
    "$id": "arn:aws:iot:::schema:auditnotification/1.0",
    "type": "object",
    "properties": {
        "accountId": {
            "type": "string"
        },
        "taskId": {
            "type": "string"
        },
        "taskStatus": {
            "type": "string",
            "enum": [
                "FAILED",
                "CANCELED",
                "COMPLETED"
            ]
        },
        "taskType": {
            "type": "string",
            "enum": [
                "SCHEDULED_AUDIT_TASK",
                "ON_DEMAND_AUDIT_TASK"
            ]
        },
        "scheduledAuditName": {
            "type": "string"
        },
        "failedChecksCount": {
            "type": "integer"
        },
        "canceledChecksCount": {
            "type": "integer"
        },
        "nonCompliantChecksCount": {
            "type": "integer"
        },
        "compliantChecksCount": {
            "type": "integer"
        },
        "totalChecksCount": {
            "type": "integer"
        },
        "taskStartTime": {
            "type": "integer"
        },
        "auditDetails": {
            "type": "array",
            "items": [
                {
                    "type": "object",
                    "properties": {
                        "checkName": {
                            "type": "string",
                            "enum": [
                                "DEVICE_CERT_APPROACHING_EXPIRATION_CHECK", 
                                "REVOKED_DEVICE_CERT_CHECK", 
                                "CA_CERT_APPROACHING_EXPIRATION_CHECK", 
                                "REVOKED_CA_CERT_CHECK", 
                                "LOGGING_DISABLED_CHECK"
                            ]
                        },
                        "checkRunStatus": {
                            "type": "string",
                            "enum": [
                                "FAILED",
                                "CANCELED",
                                "COMPLETED_COMPLIANT",
                                "COMPLETED_NON_COMPLIANT"
                            ]
                        },
                        "nonCompliantResourcesCount": {
                            "type": "integer"
                        },
                        "totalResourcesCount": {
                            "type": "integer"
                        },
                        "message": {
                            "type": "string",
                        },
                        "errorCode": {
                            "type": "string",
                            "enum": [
                                "INSUFFICIENT_PERMISSIONS",
                                "AUDIT_CHECK_DISABLED"
                            ]
                        }
                    },
                    "required": [
                        "checkName",
                        "checkRunStatus",
                        "nonCompliantResourcesCount",
                        "totalResourcesCount"
                    ]
                }
            ]
        }
    },
    "required": [
        "accountId",
        "taskId",
        "taskStatus",
        "taskType",
        "failedChecksCount",
        "canceledChecksCount",
        "nonCompliantChecksCount",
        "compliantChecksCount",
        "totalChecksCount",
        "taskStartTime",
        "auditDetails"
    ]
}
```

You can also view notifications in the AWS IoT console, along with information about the device, device statistics \(for example, last connection time, number of active connections, data transfer rate\), and historical alerts for the device\.

## Permissions<a name="device-defender-audit-permissions"></a>

This section contains information about how to set up the IAM roles and policies required to create, run, and manage AWS IoT Device Defender audits\. For more information, see the [AWS Identity and Access Management User Guide](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html)\.

### Give AWS IoT Device Defender permission to collect your data to run an audit<a name="device-defender-audit-permissions-collect"></a>

When you call [ UpdateAccountAuditConfiguration](AuditCommands.md#dd-api-iot-UpdateAccountAuditConfiguration), you must specify an IAM role with two policies: a permissions policy and a trust policy\. The permissions policy grants AWS IoT Device Defender permission to access your account data, using AWS IoT APIs, when it runs an audit\. The trust policy grants AWS IoT Device Defender permission to assume the required role\. 

#### permissions policy<a name="audit-account-permissions-policy"></a>

```
{
    "Version":"2012-10-17",
    "Statement":[
        {
            "Effect":"Allow",
            "Action":[
                "iot:GetLoggingOptions",
                "iot:GetV2LoggingOptions",
                "iot:ListCACertificates",
                "iot:ListCertificates",
                "iot:DescribeCACertificate",
                "iot:DescribeCertificate",
                "iot:ListPolicies",
                "iot:GetPolicy",
                "iot:GetEffectivePolicies",
                "iot:ListRoleAliases",
                "iot:DescribeRoleAlias",
                "cognito-identity:GetIdentityPoolRoles",
                "iam:ListRolePolicies",
                "iam:ListAttachedRolePolicies",
                "iam:GetRole",
                "iam:GetPolicy",
                "iam:GetPolicyVersion",
                "iam:GetRolePolicy",
                "iam:GenerateServiceLastAccessedDetails",
                "iam:GetServiceLastAccessedDetails"
            ],
            "Resource":[
                "*"
            ]
        }
    ]
}
```

#### trust policy<a name="audit-account-trust-policy"></a>

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "",
      "Effect": "Allow",
      "Principal": {
        "Service": "iot.amazonaws.com"
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
```

### Give AWS IoT Device Defender permission to publish notifications to an SNS topic<a name="device-defender-audit-permissions-publish"></a>

If you use the `auditNotificationTargetConfigurations` parameter in [ UpdateAccountAuditConfiguration](AuditCommands.md#dd-api-iot-UpdateAccountAuditConfiguration), you must specify an IAM role with two policies: a permissions policy and a trust policy\. The permissions policy grants permission to AWS IoT Device Defender to publish notifications to your SNS topic\. The trust policy grants AWS IoT Device Defender permission to assume the required role\.

#### permissions policy<a name="audit-account-sns-permissions-policy"></a>

```
{
  "Version":"2012-10-17",
  "Statement":[
      {
        "Effect":"Allow",
        "Action":[
          "sns:Publish"
        ],
        "Resource":[
          "arn:aws:sns:region:account-id:your-topic-name"
        ]
    }
  ]
}
```

#### trust policy<a name="audit-account-sns-trust-policy"></a>

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "",
      "Effect": "Allow",
      "Principal": {
        "Service": "iot.amazonaws.com"
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
```

### Give IAM users or groups permission to run AWS IoT Device Defender audit commands<a name="device-defender-audit-permissions-manage"></a>

To allow IAM users or groups to manage, run, or view the results of AWS IoT Device Defender, you must create and assign roles with attached policies that grant permission to run the appropriate commands\. The content of each policy depends on which commands you want the user or group to run\. 
+ `UpdateAccountAuditConfiguration`

#### policy<a name="audit-manage1-policy"></a>

You must create the IAM role with the attached policy in same account from which this command is run\. Cross account access is not allowed\. The policy should have `iam:PassRole` permissions \(permissions to pass this role\)\. 

In the following policy template, `audit-permissions-role-arn` is the role ARN that you pass to AWS IoT Device Defender in the `UpdateAccountAuditConfiguration` request using the `roleArn` parameter\. `audit-notifications-permissions-role-arn` is the role ARN that you pass to AWS IoT Device Defender in the `UpdateAccountAuditConfiguration` request using the `auditNotificationTargetConfigurations` parameter\. 

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "iot:UpdateAccountAuditConfiguration"
      ],
      "Resource": [
        "*"
      ]
    },
    {
      "Effect": "Allow",
      "Action": [
        "iam:PassRole"
      ],
      "Resource": [
        "arn:aws:iam::account-id:role/audit-permissions-role-arn",
        "arn:aws:iam::account-id:role/audit-notifications-permissions-role-arn"
      ]
    }
  ]
}
```
+ `DescribeAccountAuditConfiguration`
+ `DeleteAccountAuditConfiguration`
+ `StartOnDemandAuditTask`
+ `CancelAuditTask`
+ `DescribeAuditTask`
+ `ListAuditTasks`
+ `ListScheduledAudits`
+ `ListAuditFindings`

#### policy<a name="audit-manage2-policy"></a>

All of these commands require \* in the `Resource` field of the policy\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "iot:DescribeAccountAuditConfiguration",
        "iot:DeleteAccountAuditConfiguration",
        "iot:StartOnDemandAuditTask",
        "iot:CancelAuditTask",
        "iot:DescribeAuditTask",
        "iot:ListAuditTasks",
        "iot:ListScheduledAudits",
        "iot:ListAuditFindings"
      ],
      "Resource": [
        "*"
      ]
    }
  ]
}
```
+ `CreateScheduledAudit`
+ `UpdateScheduledAudit`
+ `DeleteScheduledAudit`
+ `DescribeScheduledAudit`

#### policy<a name="audit-manage3-policy"></a>

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "iot:CreateScheduledAudit",
        "iot:UpdateScheduledAudit",
        "iot:DeleteScheduledAudit",
        "iot:DescribeScheduledAudit"
      ],
      "Resource": [
          "arn:aws:iot:region:account-id:scheduledaudit/scheduled-audit-name"
      ]
    }
  ]
}
```

The format for an AWS IoT Device Defender scheduled audit role ARN is:

```
arn:aws:iot:region:account-id:scheduledaudit/scheduled-audit-name
```

## Service Limits<a name="device-defender-audit-limits"></a>


****  

| Resource | Limit | Description | 
| --- | --- | --- | 
| scheduled audits | 5 max\. | You can create up to 5 scheduled audits before a LimitExceeded exception occurs\. | 
| simultaneous in\-progress, on\-demand audits | 10 max\. | You can create up to 10 on\-demand audits before a LimitExceeded exception occurs\. | 