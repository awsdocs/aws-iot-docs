# JSON Extensions<a name="iot-sql-json"></a>

You can use the following extensions to ANSI SQL syntax to make it easier to work with nested JSON objects\.

"\." Operator

This operator accesses members in embedded JSON objects and functions identically to ANSI SQL and JavaScript\. For example: 

```
SELECT foo.bar AS bar.baz FROM 'a/b'
```

 `*` Operator

This functions in the same way as the `*` wildcard in ANSI SQL\. It's used in the SELECT clause only and creates a new JSON object containing the message data\. If the message payload is not in JSON format, `*` returns the entire message payload as raw bytes\. For example: 

```
SELECT * FROM 'a/b'
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