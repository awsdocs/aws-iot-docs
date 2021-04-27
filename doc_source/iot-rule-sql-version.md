# SQL versions<a name="iot-rule-sql-version"></a>

The AWS IoT rules engine uses an SQL\-like syntax to select data from MQTT messages\. The SQL statements are interpreted based on an SQL version specified with the `awsIotSqlVersion` property in a JSON document that describes the rule\. For more information about the structure of JSON rule documents, see [Creating a Rule](iot-create-rule.md)\. The `awsIotSqlVersion` property lets you specify which version of the AWS IoT SQL rules engine that you want to use\. When a new version is deployed, you can continue to use an earlier version or change your rule to use the new version\. Your current rules continue to use the version with which they were created\. 

The following JSON example shows you how to specify the SQL version using the `awsIotSqlVersion` property\.

```
{
    "sql": "expression",
    "ruleDisabled": false,
    "awsIotSqlVersion": "2016-03-23",
    "actions": [{
        "republish": {
            "topic": "my-mqtt-topic",
            "roleArn": "arn:aws:iam::123456789012:role/my-iot-role"
        }
    }]
}
```

AWS IoT currently supports the following SQL versions:
+ `2015-10-08` – The original SQL version built on 2015\-10\-08\.
+ `2016-03-23` – The SQL version built on 2016\-03\-23\.
+ `beta` – The most recent beta SQL version\. If you use this version, it might introduce breaking changes to your rules\.

## What's new in the 2016\-03\-23 SQL rules engine version<a name="sql-2016-03-23-beta"></a>
+ Fixes for selecting nested JSON objects\.
+ Fixes for array queries\.
+ Intra\-object query support\. For more information, see [Nested object queries](iot-sql-nested-queries.md)\.
+ Support to output an array as a top\-level object\.
+ Addition of the `encode(value, encodingScheme)` function, which can be applied on JSON and non\-JSON format data\. For more information, see the [encode function](iot-sql-functions.md#iot-sql-encode-payload)\.

### Output an `Array` as a top\-level object<a name="return-array-rule"></a>

This feature allows a rule to return an array as a top\-level object\. For example, given the following MQTT message:

```
{
    "a": {"b":"c"},
    "arr":[1,2,3,4]
}
```

And the following rule:

```
SELECT VALUE arr FROM 'topic'
```

The rule generates the following output\.

```
[1,2,3,4]
```