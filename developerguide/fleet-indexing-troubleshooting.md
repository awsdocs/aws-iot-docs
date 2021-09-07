# Fleet indexing troubleshooting guide<a name="fleet-indexing-troubleshooting"></a>

## Troubleshooting aggregation queries for the fleet indexing service<a name="aggregation-troubleshooting"></a>

If you are having type mismatch errors, you can use CloudWatch Logs to troubleshoot the problem\. CloudWatch Logs must be enabled before logs are written by the Fleet Indexing service\. For more information, see [Monitor AWS IoT using CloudWatch Logs](cloud-watch-logs.md)\.

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

If a device has been disconnected for approximately an hour, the connectivity status `timestamp` value might be missing\. For persistent sessions, the value might be missing after a client has been disconnected longer than the configured time\-to\-live \(TTL\) for the persistent session\. The connectivity status data is indexed only for connections where the client ID has a matching thing name\. \(The client ID is the value used to connect a device to AWS IoT Core\.\)

## Troubleshooting fleet metrics<a name="fleet-metrics-troubleshooting"></a>

**Can't create a fleet metric**

Downgrading data sources by updating fleet indexing configuration is not supported\. 

If you try to create a fleet metric with downgraded data sources \(for example, previously the data sources were registry data, shadow data, and device connectivity data, and now the data sources are registry data and shadow data and without device connectivity data\), you'll see errors and you won't be able to create a fleet metric\.

Modifying custom fields used by existing fleet metrics is not supported\.

**Can't see data points in CloudWatch**

If you're able to create a fleet metric but you can't see data points in CloudWatch, it's likely that you don't have a thing that meets the query string criteria\. 

See this example command of how to create a fleet metric: 

```
aws iot create-fleet-metric --metric-name "example_FM" --query-string "thingName:TempSensor* AND attributes.temperature>80" --period 60 --aggregation-field "attributes.temperature" --aggregation-type name=Statistics,values=count
```

If you don't have a thing that meets the query string criteria `--query-string "thingName:TempSensor* AND attributes.temperature>80"`:
+ With `values=count`, you'll be able to create a fleet metric and there'll be data points to show in CloudWatch\. The data points of the value `count` is always 0\.
+ With `values` other than `count`, you'll be able to create a fleet metric but you won't see the fleet metric in CloudWatch and there'll be no data points to show in CloudWatch\.