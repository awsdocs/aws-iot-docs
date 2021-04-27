# JSON extensions<a name="iot-sql-json"></a>

You can use the following extensions to ANSI SQL syntax to make it easier to work with nested JSON objects\.

"\." Operator

This operator accesses members in embedded JSON objects and functions identically to ANSI SQL and JavaScript\. For example: 

```
SELECT foo.bar AS bar.baz FROM 'topic/subtopic'
```

selects the value of the `bar` property in the `foo` object from the following message payload sent to the `topic/subtopic` topic\.

```
{
  "foo": {
    "bar": "RED",
    "bar1": "GREEN",
    "bar2": "BLUE"
  }
}
```

If a JSON property name includes a hyphen character or numeric characters, the simple 'dot' notation will not work\. Instead, you must use the [get function](iot-sql-functions.md#iot-sql-function-get) to extract the property's value\. 

 In this example the following message is sent to the `iot/rules` topic\. 

```
{
  "mydata": {
    "item2": {
      "0": {
        "my-key": "myValue"
      }
    }
  }
}
```

Normally, the value of `my-key` would be identified as in this query\.

```
SELECT * from iot/rules WHERE mydata.item2.0.my-key= "myValue"
```

However, because the property name `my-key` contains a hyphen and `item2` contains a numeric character, the [get function](iot-sql-functions.md#iot-sql-function-get) must be used as the following query shows\.

```
SELECT * from 'iot/rules' WHERE get(get(get(mydata,"item2"),"0"),"my-key") = "myValue"
```

 `*` Operator

This functions in the same way as the `*` wildcard in ANSI SQL\. It's used in the SELECT clause only and creates a new JSON object containing the message data\. If the message payload is not in JSON format, `*` returns the entire message payload as raw bytes\. For example: 

```
SELECT * FROM 'topic/subtopic'
```

**Applying a Function to an Attribute Value**  
The following is an example JSON payload that might be published by a device:

```
{
    "deviceid" : "iot123",
    "temp" : 54.98,
    "humidity" : 32.43,
    "coords" : {
        "latitude" : 47.615694,
        "longitude" : -122.3359976
    }
}
```

The following example applies a function to an attribute value in a JSON payload:

```
SELECT temp, md5(deviceid) AS hashed_id FROM topic/#
```

The result of this query is the following JSON object:

```
{
   "temp": 54.98,
   "hashed_id": "e37f81fb397e595c4aeb5645b8cbbbd1"
}
```