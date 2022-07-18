# Timestream<a name="timestream-rule-action"></a>

The Timestream rule action writes attributes \(measures\) from an MQTT message into an Amazon Timestream table\. For more information about Amazon Timestream, see [What Is Amazon Timestream?](https://docs.aws.amazon.com/timestream/latest/developerguide/what-is-timestream.html)\.

**Note**  
Amazon Timestream is not available in all AWS Regions\. If Amazon Timestream is not available in your Region, it won't appear in the list of rule actions\.

The attributes that this rule stores in the Timestream database are those that result from the rule's query statement\. The value of each attribute in the query statement's result is parsed to infer its data type \(as in a [DynamoDBv2](dynamodb-v2-rule-action.md) action\)\. Each attribute's value is written to its own record in the Timestream table\. To specify or change an attribute's data type, use the [`cast()`](iot-sql-functions.md#iot-sql-function-cast) function in the query statement\. For more information about the contents of each Timestream record, see [Amazon Timestream record content](#timestream-rule-action-data)\.

**Note**  
With SQL V2 \(2016\-03\-23\), numeric values that are whole numbers, such as `10.0`, are converted their Integer representation \(`10`\)\. Explicitly casting them to a `Decimal` value, such as by using the [cast\(\)](iot-sql-functions.md#iot-sql-function-cast) function, does not prevent this behavior—the result is still an `Integer` value\. This can cause type mismatch errors that prevent data from being recorded in the Timestream database\. To reliably process whole number numeric values as `Decimal` values, use SQL V1 \(2015\-10\-08\) for the rule query statement\.

**Note**  
The maximum number of values that a Timestream rule action can write into an Amazon Timestream table is 100\. For more information, see [Amazon Timestream Quota's Reference](https://docs.aws.amazon.com/timestream/latest/developerguide/ts-limits.html#limits.default)\. 

## Requirements<a name="timestream-rule-action-requirements"></a>

This rule action has the following requirements:
+ An IAM role that AWS IoT can assume to perform the `timestream:DescribeEndpoints` and `timestream:WriteRecords` operations\. For more information, see [Granting an AWS IoT rule the access it requires](iot-create-role.md)\.

  In the AWS IoT console, you can choose, update, or create a role to allow AWS IoT to perform this rule action\.
+ If you use a customer\-managed AWS Key Management Service \(AWS KMS\) to encrypt data at rest in Timestream, the service must have permission to use the AWS KMS key on the caller's behalf\. For more information, see [How AWS services use AWS KMS](https://docs.aws.amazon.com/kms/latest/developerguide/service-integration.html)\.

## Parameters<a name="timestream-rule-action-parameters"></a>

When you create an AWS IoT rule with this action, you must specify the following information:

`databaseName`  
The name of an Amazon Timestream database that has the table to receive the records this action creates\. See also `tableName`\.  
Supports [substitution templates](iot-substitution-templates.md): API and AWS CLI only

`dimensions`  
Metadata attributes of the time series that are written in each measure record\. For example, the name and Availability Zone of an EC2 instance or the name of the manufacturer of a wind turbine are dimensions\.    
`name`  
The metadata dimension name\. This is the name of the column in the database table record\.  
Dimensions can't be named: `measure_name`, `measure_value`, or `time`\. These names are reserved\. Dimension names can't start with `ts_` or `measure_value` and they can't contain the colon \(`:`\) character\.  
Supports [substitution templates](iot-substitution-templates.md): No  
`value`  
The value to write in this column of the database record\.  
Supports [substitution templates](iot-substitution-templates.md): Yes

`roleArn`  
The Amazon Resource Name \(ARN\) of the role that grants AWS IoT permission to write to the Timestream database table\. For more information, see [Requirements](#timestream-rule-action-requirements)\.  
Supports [substitution templates](iot-substitution-templates.md): No

`tableName`  
The name of the database table into which to write the measure records\. See also `databaseName`\.  
Supports [substitution templates](iot-substitution-templates.md): API and AWS CLI only

`timestamp`  
 The value to use for the entry's timestamp\. If blank, the time that the entry was processed is used\.     
`unit`  
The precision of the timestamp value that results from the expression described in `value`\.  
Valid values: `SECONDS` \| `MILLISECONDS` \| `MICROSECONDS` \| `NANOSECONDS`\. The default is `MILLISECONDS`\.  
`value`  
An expression that returns a long epoch time value\.  
You can use the [time\_to\_epoch\(String, String\)](iot-sql-functions.md#iot-sql-function-time-to-epoch) function to create a valid timestamp from a date or time value passed in the message payload\. 

## Amazon Timestream record content<a name="timestream-rule-action-data"></a>

The data written to the Amazon Timestream table by this action include a timestamp, metadata from the Timestream rule action, and the result of the rule's query statement\.

For each attribute \(measure\) in the result of the query statement, this rule action writes a record to the specified Timestream table with these columns\.


|  Column name  |  Attribute type  |  Value  |  Comments  | 
| --- | --- | --- | --- | 
|  *dimension\-name*  |  DIMENSION  |  The value specified in the Timestream rule action entry\.  |  Each **Dimension** specified in the rule action entry creates a column in the Timestream database with the dimension's name\.  | 
|  measure\_name  |  MEASURE\_NAME  |  The attribute's name  |  The name of the attribute in the result of the query statement whose value is specified in the `measure_value::data-type` column\.  | 
|  measure\_value::*data\-type*  |  MEASURE\_VALUE  |  The value of the attribute in the result of the query statement\. The attribute's name is in the `measure_name` column\.  |  The value is interpreted\* and cast as the best match of: `bigint`, `boolean`, `double`, or `varchar`\. Amazon Timestream creates a separate column for each data type\. The value in the message can be cast to another data type by using the [`cast()`](iot-sql-functions.md#iot-sql-function-cast) function in the rule's query statement\.  | 
|  time  |  TIMESTAMP  |  The date and time of the record in the database\.  |  This value is assigned by rules engine or the `timestamp` property, if it is defined\.  | 

\* The attribute value read from the message payload is interpreted as follows\. See the [Examples](#timestream-rule-action-examples) for an illustration of each of these cases\.
+ An unquoted value of `true` or `false` is interpreted as a `boolean` type\.
+ A decimal numeric is interpreted as a `double` type\.
+ A numeric value without a decimal point is interpreted as a `bigint` type\.
+ A quoted string is interpreted as a `varchar` type\.
+ Objects and array values are converted to JSON strings and stored as a `varchar` type\.

## Examples<a name="timestream-rule-action-examples"></a>

The following JSON example defines a Timestream rule action with a substitution template in an AWS IoT rule\.

```
{
  "topicRulePayload": {
    "sql": "SELECT * FROM 'iot/topic'",
    "ruleDisabled": false,
    "awsIotSqlVersion": "2016-03-23",
    "actions": [
      {
        "timestream": {
          "roleArn": "arn:aws:iam::123456789012:role/aws_iot_timestream",
          "tableName": "devices_metrics",
          "dimensions": [
            {
              "name": "device_id",
              "value": "${clientId()}"
            },
            {
              "name": "device_firmware_sku",
              "value": "My Static Metadata"
            }
          ],
          "databaseName": "record_devices"
        }
      }
    ]
  }
}
```

Using the Timestream topic rule action defined in the previous example with the following message payload results in the Amazon Timestream records written in the table that follows\.

```
{
  "boolean_value": true,
  "integer_value": 123456789012,
  "double_value": 123.456789012,
  "string_value": "String value",
  "boolean_value_as_string": "true",
  "integer_value_as_string": "123456789012",
  "double_value_as_string": "123.456789012",
  "array_of_integers": [23,36,56,72],
  "array of strings": ["red", "green","blue"],
  "complex_value": {
    "simple_element": 42,
    "array_of_integers": [23,36,56,72],
    "array of strings": ["red", "green","blue"]
  }
}
```

The following table displays the database columns and records that using the specified topic rule action to process the previous message payload creates\. The `device_firmware_sku` and `device_id` columns are the DIMENSIONS defined in the topic rule action\. The Timestream topic rule action creates the `time` column and the `measure_name` and `measure_value::*` columns, which it fills with the values from the result of the topic rule action's query statement\. 


| device\_firmware\_sku | device\_id | measure\_name | measure\_value::bigint | measure\_value::varchar | measure\_value::double | measure\_value::boolean | time | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| My Static Metadata | iotconsole\-159EXAMPLE738\-0 | complex\_value | \- | \{"simple\_element":42,"array\_of\_integers":\[23,36,56,72\],"array of strings":\["red","green","blue"\]\} | \- | \- | 2020\-08\-26 22:42:16\.423000000 | 
| My Static Metadata | iotconsole\-159EXAMPLE738\-0 | integer\_value\_as\_string | \- | 123456789012 | \- | \- | 2020\-08\-26 22:42:16\.423000000 | 
| My Static Metadata | iotconsole\-159EXAMPLE738\-0 | boolean\_value | \- | \- | \- | TRUE | 2020\-08\-26 22:42:16\.423000000 | 
| My Static Metadata | iotconsole\-159EXAMPLE738\-0 | integer\_value | 123456789012 | \- | \- | \- | 2020\-08\-26 22:42:16\.423000000 | 
| My Static Metadata | iotconsole\-159EXAMPLE738\-0 | string\_value | \- | String value | \- | \- | 2020\-08\-26 22:42:16\.423000000 | 
| My Static Metadata | iotconsole\-159EXAMPLE738\-0 | array\_of\_integers | \- | \[23,36,56,72\] | \- | \- | 2020\-08\-26 22:42:16\.423000000 | 
| My Static Metadata | iotconsole\-159EXAMPLE738\-0 | array of strings | \- | \["red","green","blue"\] | \- | \- | 2020\-08\-26 22:42:16\.423000000 | 
| My Static Metadata | iotconsole\-159EXAMPLE738\-0 | boolean\_value\_as\_string | \- | TRUE | \- | \- | 2020\-08\-26 22:42:16\.423000000 | 
| My Static Metadata | iotconsole\-159EXAMPLE738\-0 | double\_value | \- | \- | 123\.456789012 | \- | 2020\-08\-26 22:42:16\.423000000 | 
| My Static Metadata | iotconsole\-159EXAMPLE738\-0 | double\_value\_as\_string | \- | 123\.45679 | \- | \- | 2020\-08\-26 22:42:16\.423000000 | 