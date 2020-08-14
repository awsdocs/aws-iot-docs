# Conflicting MQTT client IDs<a name="audit-chk-conflicting-client-ids"></a>

Multiple devices connect using the same client ID\.

This check appears as `CONFLICTING_CLIENT_IDS_CHECK` in the CLI and API\.

Severity: **High**

## Details<a name="audit-chk-conflicting-client-ids-details"></a>

Multiple connections were made using the same client ID, causing an already connected device to be disconnected\. The MQTT specification allows only one active connection per client ID, so when another device connects using the same client ID, it knocks the previous one off the connection\.

When performed as part of an on\-demand audit, this check looks at how client IDs were used to connect during the 31 days prior to the start of the audit\. For scheduled audits, this check looks at data from the last time the audit was run to the time this instance of the audit started\. If you have taken steps to mitigate this condition during the time checked, note when the connections/disconnections were made to determine if the problem persists\.

The following reason codes are returned when this check finds noncompliance:
+ DUPLICATE\_CLIENT\_ID\_ACROSS\_CONNECTIONS

The findings returned by this check also include the client ID used to connect, principal IDs, and disconnect times\. The most recent results are listed first\.

## Why it matters<a name="audit-chk-conflicting-client-ids-why-it-matters"></a>

Devices with conflicting IDs are forced to constantly reconnect, which might result in lost messages or leave a device unable to connect\.

This might indicate that a device or a device's credentials have been compromised, and might be part of a DDoS attack\. It is also possible that devices are not configured correctly in the account or a device has a bad connection and is forced to reconnect several times per minute\.

## How to fix it<a name="audit-chk-conflicting-client-ids-how-to-fix"></a>

Register each device as a unique thing in AWS IoT, and use the thing name as the client ID to connect\. Or use a UUID as the client ID when connecting the device over MQTT\. You can also use mitigation actions to:
+ Apply the `PUBLISH_FINDINGS_TO_SNS` mitigation action if you want to implement a custom response in response to the Amazon SNS message\. 

For more information, see [Mitigation actions](device-defender-mitigation-actions.md)\.