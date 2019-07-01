# SQL Versions<a name="iot-rule-sql-version"></a>

The AWS IoT rules engine uses an SQL\-like syntax to select data from MQTT messages\. The SQL statements are interpreted based on an SQL version specified with the `awsIotSqlVersion` property in a JSON document that describes the rule\. For more information about the structure of JSON rule documents, see [Creating a Rule](iot-create-rule.md)\. The `awsIotSqlVersion` property allows you to specify which version of the AWS IoT SQL rules engine you want to use\. When a new version is deployed, you can continue to use an older version or change your rule to use the new version\. Your current rules continue to use the version with which they were created\. 

The following JSON example shows how to specify the SQL version using the `awsIotSqlVersion` property:

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

Current supported versions are:
+ `2015-10-08`, the original SQL version built on 2015\-10\-08\.
+ `2016-03-23`, the SQL version built on 2016\-03\-23\.
+ `beta`, the most recent beta SQL version\. The use of this version might introduce breaking changes to your rules\.

## What's New in the 2016\-03\-23 SQL Rules Engine Version<a name="sql-2016-03-23-beta"></a>
+ Fixes for selecting nested JSON objects\.
+ Fixes for array queries\.
+ Inter\-object query support\.
+ Support to output an array as a top\-level object\.
+ Addition of the encode \(*value*, *encodingScheme*\) function, which can be applied on both JSON and non\-JSON format data\.

### Inter\-Object Queries<a name="inter-obj-query"></a>

This feature allows you to query for an attribute in a JSON object\. For example, given the following MQTT message:

```
{ 
    "e": [
       { "n": "temperature", "u": "Cel", "t": 1234, "v":22.5 },
       { "n": "light", "u": "lm", "t": 1235, "v":135 },
       { "n": "acidity", "u": "pH", "t": 1235, "v":7 }
    ]
}
```

And the following rule:

```
SELECT (SELECT v FROM e WHERE n = 'temperature') as temperature FROM 'my/topic'
```

The rule generates the following output:

```
{"temperature": [{"v":22.5}]}
```

Using the same MQTT message, given a slightly more complicated rule such as: 

```
SELECT get((SELECT v FROM e WHERE n = 'temperature'),1).v as temperature FROM 'topic'
```

The rule generates the following output:

```
{"temperature":22.5}
```

### Output an `Array` as a Top\-Level Object<a name="return-array-rule"></a>

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

The rule generates the following output:

```
[1,2,3,4]
```

### Encode Function<a name="encode-function"></a>

Encodes the payload, which potentially might be non\-JSON data, into its string representation based on the specified encoding scheme\.