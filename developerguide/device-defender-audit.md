# Audit<a name="device-defender-audit"></a>

An AWS IoT Device Defender audit looks at account\- and device\-related settings and policies to ensure security measures are in place\. An audit can help you detect any drifts from security best practices or access policies \(for example, multiple devices using the same identity, or overly permissive policies that allow one device to read and update data for many other devices\)\. You can run audits as needed \(*on\-demand audits*\) or schedule them to be run periodically \(*scheduled audits*\)\. 

An AWS IoT Device Defender audit runs a set of predefined checks for common IoT security best practices and device vulnerabilities\. Examples of predefined checks include policies that grant permission to read or update data on multiple devices, devices that share an identity \(X\.509 certificate\), or certificates that are expiring or have been revoked but are still active\.

## Issue severity<a name="device-defender-audit-severity"></a>

Issue severity indicates the level of concern associated with each identified instance of noncompliance and the recommended time to remediation\.

**Critical**  
Non compliant audit checks with this severity identify issues that require urgent attention\. Critical issues often allow bad actors with little sophistication and no insider knowledge or special credentials to easily gain access to or control of your assets\.

**High**  
Non compliant audit checks with this severity require urgent investigation and remediation planning after critical issues are addressed\. Like critical issues, high severity issues often provide bad actors with access to or control of your assets\. However, high severity issues are often more difficult to exploit\. They might require special tools, insider knowledge, or specific setups\.

**Medium**  
Non compliant audit checks with this severity present issues that need attention as part of your continuous security posture maintenance\. Medium severity issues might cause negative operational impact, such as unplanned outages due to malfunction of security controls\. These issues might also provide bad actors with limited access to or control of your assets, or might facilitate parts of their malicious actions\.

**Low**  
Non compliant audit checks with this severity often indicate security best practices were overlooked or bypassed\. Although they might not cause an immediate security impact on their own, these lapses can be exploited by bad actors\. Like medium severity issues, low severity issues require attention as part of your continuous security posture maintenance\.

## Next steps<a name="device-defender-audit-severity-next-steps"></a>

To understand the types of audit checks that can be performed, see [Audit checks](device-defender-audit-checks.md)\. For information about service quotas that apply to audits, see [Service Quotas](https://docs.aws.amazon.com/general/latest/gr/iot-core.html#limits_iot)\.