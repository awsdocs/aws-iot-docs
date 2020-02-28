# Troubleshooting Aggregation Queries for the Fleet Indexing Service<a name="aggregation-troubleshooting"></a>

If you are having type mismatch errors, you can use CloudWatch Logs to troubleshoot the problem\. CloudWatch Logs must be enabled before logs are written by the Fleet Indexing service\. For more information, see [CloudWatch Logs](https://docs.aws.amazon.com/iot/latest/developerguide/cloud-watch-logs.html)\.

When you make aggregation queries on non\-managed fields, you can only specify a field you defined in the `customFields` argument passed to `UpdateIndexingConfiguration` or update\-indexing\-configuration\. If the field value is inconsistent with the configured field data type, this value is ignored when you perform an aggregation query\.

The Fleet Indexing service emits an error log to CloudWatch Logs when a field cannot be indexed because of a mismatched type\. The error log contains the field name, the value that could not be converted, and the thing name for the device\. The following is an example error log:

```
{
  "timestamp": "2017-02-20 20:31:22.932",
  "logLevel": "ERROR",
  "traceId": "79738924-1025-3a00-a669-7bec69f7f07a",
  "accountId": "000000000000",
  "status": "SucceededWithIssues",
  "eventType": "IndexingCustomFieldFailed",
  "thingName": "thing0",
  "failedCustomFields": [
    {
      "Name": "attributeName1",
      "Value": "apple",
      "ExpectedType": "String"
    },
    {
      "Name": "attributeName2",
      "Value": "2",
      "ExpectedType": "Boolean"
    }
  ]
}
```

If a device has been disconnected for approximately an hour, the connectivity status `timestamp` value might be missing\. For persistent sessions, the value might be missing after a client has been disconnected longer than the configured time\-to\-live \(TTL\) for the persistent session\. The connectivity status data is indexed only for connections where the client ID has a matching thing name\. \(The client ID is the value used to connect a device to \.\)