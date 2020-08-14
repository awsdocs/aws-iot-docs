# Logging disabled<a name="audit-chk-logging-disabled"></a>

AWS IoT logs are not enabled in Amazon CloudWatch\. Verifies both V1 and V2 logging\.

This check appears as `LOGGING_DISABLED_CHECK` in the CLI and API\.

Severity: **Low**

## Details<a name="audit-chk-logging-disabled-details"></a>

The following reason codes are returned when this check finds noncompliance:
+ LOGGING\_DISABLED

## Why it matters<a name="audit-chk-logging-disabled-why-it-matters"></a>

AWS IoT logs in CloudWatch provide visibility into behaviors in AWS IoT, including authentication failures and unexpected connects and disconnects that might indicate that a device has been compromised\.

## How to fix it<a name="audit-chk-logging-disabled-how-to-fix"></a>

Enable AWS IoT logs in CloudWatch\. See [ Monitoring Tools](security-logging.md#monitoring_automated_manual)\. You can also use mitigation actions to:
+ Apply the `ENABLE_IOT_LOGGING` mitigation action on your audit findings to make this change\. 
+ Apply the `PUBLISH_FINDINGS_TO_SNS` mitigation action if you want to implement a custom response in response to the Amazon SNS message\. 

For more information, see [Mitigation actions](device-defender-mitigation-actions.md)\. 