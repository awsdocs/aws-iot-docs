# Getting Statistics About Your Device Fleet<a name="index-aggregate"></a>

You can use the get\-statistics CLI command or the [GetStatistics](https://docs.aws.amazon.com/iot/latest/apireference/API_UpdateIndexingConfiguration.html) API to search an index for aggregate data\. For example, you might want to find the number of devices that are currently connected to AWS IoT:

aws iot get\-statistics \-\-index\-name AWS\_Things \-\-query\-string "connectivity\.connected:true"\.

This command returns the number of things that have a property called `connectivity.connected` set to `true` in their device shadow:

```
{
  "statistics" : {
    "count" : 1000
  }
}
```

The get\-statistics CLI command takes the following parameters:

`index-name`  
The name of the index to search\. The default value is `AWS_Things`\.

`query-string`  
The query used to search the index\. You can specify `"*"` to get the count of all indexed things in your AWS account\.

`query-version`  
The version of the query to use\. The default value is `2017-09-30`\.

The get\-statistics CLI command returns data in a JSON object\. Currently, the only statistic returned is `count`:

```
{
  "statistics" : {
    "count" : 1000
  }
}
```