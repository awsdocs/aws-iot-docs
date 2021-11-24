# Querying for aggregate data<a name="index-aggregate"></a>

AWS IoT provides four APIs \(`GetStatistics`, `GetCardinality`, `GetPercentiles`, and `GetBucketsAggregation`\) that allow you to search your device fleet for aggregate data\. 

**Note**  
 For issues with missing or unexpected values for the aggregation APIs, read [Fleet indexing troubleshooting guide](fleet-indexing-troubleshooting.md)\. 

## GetStatistics<a name="get-statistics"></a>

The [GetStatistics](https://docs.aws.amazon.com/iot/latest/apireference/API_GetStatistics.html) API and the get\-statistics CLI command return the count, average, sum, minimum, maximum, sum of squares, variance, and standard deviation for the specified aggregated field\.

The get\-statistics CLI command takes the following parameters:

`index-name`  
The name of the index to search\. The default value is `AWS_Things`\.

`query-string`  
The query used to search the index\. You can specify `"*"` to get the count of all indexed things in your AWS account\.

`aggregationField`  
Optional\. The field to aggregate\. This field must be a managed or custom field defined when you call update\-indexing\-configuration\. If you don't specify an aggregation field, `registry.version` is used as aggregation field\.

`query-version`  
The version of the query to use\. The default value is `2017-09-30`\.

The type of aggregation field can affect the statistics returned\. 

### GetStatistics with string values<a name="string-aggregation"></a>

If you aggregate on a string field, calling `GetStatistics` returns a count of devices that have attributes that match the query\. For example:

```
aws iot get-statistics --aggregation-field 'attributes.stringAttribute'
            --query-string '*'
```

This command returns the number of devices that contain an attribute named `stringAttribute`:

```
{
  "statistics": {
    "count": 3
  }
}
```

### GetStatistics with Boolean values<a name="boolean-aggregation"></a>

When you call `GetStatistics` with a boolean aggregation field:
+ AVERAGE is the percentage of devices that match the query\.
+ MINIMUM is 0 or 1 according to the following rules:
  + If all the values for the aggregation field are `false`, MINIMUM is 0\.
  + If all the values for the aggregation field are `true`, MINIMUM is 1\.
  + If the values for the aggregation field are a mixture of `false` and `true`, MINIMUM is 0\.
+ MAXIMUM is 0 or 1 according to the following rules:
  + If all the values for the aggregation field are `false`, MAXIMUM is 0\.
  + If all the values for the aggregation field are `true`, MAXIMUM is 1\.
  + If the values for the aggregation field are a mixture of `false` and `true`, MAXIMUM is 1\.
+ SUM is the sum of the integer equivalent of the boolean values\.
+ COUNT is the count of things that match the query string criteria and contain a valid aggregation field value\.

### GetStatistics with numerical values<a name="numerical-aggregation"></a>

When you call `GetStatistics` and specify an aggregation field of type `Number`, `GetStatistics` returns the following values:

count  
The count of things that match the query string criteria and contain a valid aggregation field value\.

average  
The average of the numerical values that match the query\.

sum  
The sum of the numerical values that match the query\.

minimum  
The smallest of the numerical values that match the query\.

maximum  
The largest of the numerical values that match the query\.

sumOfSquares  
The sum of the squares of the numerical values that match the query\.

variance  
The variance of the numerical values that match the query\. The variance of a set of values is the average of the squares of the differences of each value from the average value of the set\.

stdDeviation  
The standard deviation of the numerical values that match the query\. The standard deviation of a set of values is a measure of how spread out the values are\.

The following example shows how to call get\-statistics with a numerical custom field\.

```
aws iot get-statistics --aggregation-field 'attributes.numericAttribute2'
            --query-string '*'
```

```
{
  "statistics": {
    "count": 3,
    "average": 33.333333333333336,
    "sum": 100.0,
    "minimum": -125.0,
    "maximum": 150.0,
    "sumOfSquares": 43750.0,
    "variance": 13472.22222222222,
    "stdDeviation": 116.06990230986766
  }
}
```

For numerical aggregation fields, if the field values exceed the maximum double value, the statistics values are empty

## GetCardinality<a name="get-cardinality"></a>

The [GetCardinality](https://docs.aws.amazon.com/iot/latest/apireference/API_GetCardinality.html) API and the get\-cardinality CLI command return the approximate count of unique values that match the query\. For example, you might want to find the number of devices with battery levels at less than 50 percent:

```
aws iot get-cardinality --index-name AWS_Things --query-string "batterylevel
          > 50" --aggregation-field "shadow.reported.batterylevel"
```

This command returns the number of things with battery levels at more than 50 percent:

```
{
    "cardinality": 100
}
```

`cardinality` is always returned by get\-cardinality even if there are no matching fields\. For example:

```
aws iot get-cardinality --query-string "thingName:Non-existent*"
          --aggregation-field "attributes.customField_STR"
```

```
{
    "cardinality": 0
}
```

The get\-cardinality CLI command takes the following parameters:

`index-name`  
The name of the index to search\. The default value is `AWS_Things`\.

`query-string`  
The query used to search the index\. You can specify `"*"` to get the count of all indexed things in your AWS account\.

`aggregationField`  
The field to aggregate\.

`query-version`  
The version of the query to use\. The default value is `2017-09-30`\.

## GetPercentiles<a name="get-percentiles"></a>

The [GetPercentiles](https://docs.aws.amazon.com/iot/latest/apireference/API_GetPercentiles.html) API and the get\-percentiles CLI command groups the aggregated values that match the query into percentile groupings\. The default percentile groupings are: 1,5,25,50,75,95,99, although you can specify your own when you call `GetPercentiles`\. This function returns a value for each percentile group specified \(or the default percentile groupings\)\. The percentile group "1" contains the aggregated field value that occurs in approximately one percent of the values that match the query\. The percentile group "5" contains the aggregated field value that occurs in approximately five percent of the values that match the query, and so on\. The result is an approximation, the more values that match the query, the more accurate the percentile values\.

The following example shows how to call the get\-percentiles CLI command\.

```
aws iot get-percentiles --query-string "thingName:*" --aggregation-field
          "attributes.customField_NUM" --percents 10 20 30 40 50 60 70 80 90 99
```

```
{
    "percentiles": [
        {
            "value": 3.0,
            "percent": 80.0
        },
        {
            "value": 2.5999999999999996,
            "percent": 70.0
        },
        {
            "value": 3.0,
            "percent": 90.0
        },
        {
            "value": 2.0,
            "percent": 50.0
        },
        {
            "value": 2.0,
            "percent": 60.0
        },
        {
            "value": 1.0,
            "percent": 10.0
        },
        {
            "value": 2.0,
            "percent": 40.0
        },
        {
            "value": 1.0,
            "percent": 20.0
        },
        {
            "value": 1.4,
            "percent": 30.0
        },
        {
            "value": 3.0,
            "percent": 99.0
        }
    ]
}
```

The following command shows the output returned from get\-percentiles when there are no matching documents\.

```
aws iot get-percentiles --query-string "thingName:Non-existent*"
          --aggregation-field "attributes.customField_NUM"
```

```
{
    "percentiles": []
}
```

The get\-percentile CLI command takes the following parameters:

`index-name`  
The name of the index to search\. The default value is `AWS_Things`\.

`query-string`  
The query used to search the index\. You can specify `"*"` to get the count of all indexed things in your AWS account\.

`aggregationField`  
The field to aggregate, which must be of `Number` type\.

`query-version`  
The version of the query to use\. The default value is `2017-09-30`\.

`percents`  
Optional\. You can use this parameter to specify custom percentile groupings\.

## GetBucketsAggregation<a name="get-buckets"></a>

The [GetBucketsAggregation](https://docs.aws.amazon.com/iot/latest/apireference/API_GetBucketsAggregation.html) API and the get\-buckets\-aggregation CLI command return a list of buckets and the total number of things that fit the query string criteria\.

The following example shows how to call the get\-buckets\-aggregation CLI command\.

```
aws iot get-buckets-aggregation --query-string '*' --index-name AWS_Things --aggregation-field 'shadow.reported.batterylevelpercent' --buckets-aggregation-type 'termsAggregation={maxBuckets=5}'
```

This command returns the following:

```
{
    "totalCount": 20,
    "buckets": [
        {
            "keyValue": "100",
            "count": 12
        },
        {
            "keyValue": "90",
            "count": 5
        },
        {
            "keyValue": "75",
            "count": 3
        }
    ]
}
```

The get\-buckets\-aggregation CLI command takes the following parameters:

`index-name`  
The name of the index to search\. The default value is `AWS_Things`\.

`query-string`  
The query used to search the index\. You can specify `"*"` to get the count of all indexed things in your AWS account\.

`aggregation-field`  
The field to aggregate\.

`buckets-aggregation-type`  
The basic control of the response shape and the bucket aggregation type to perform\.

## Authorization<a name="index-aggregate-authorization"></a>

You can specify the thing groups index as a resource ARN in an AWS IoT policy action, as follows\.


| Action | Resource | 
| --- | --- | 
|  `iot:GetStatistics`  |  An index ARN \(for example, `arn:aws:iot:your-aws-region:index/AWS_Things` or `arn:aws:iot:your-aws-region:index/AWS_ThingGroups`\)\.  | 