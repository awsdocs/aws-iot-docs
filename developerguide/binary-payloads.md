# Working with binary payloads<a name="binary-payloads"></a>

When the message payload should be handled as raw binary data, rather than a JSON object, use the \* operator to refer to it in a `SELECT` clause\. This works for non\-JSON payloads with some rule actions, such as the [S3 action](https://docs.aws.amazon.com/iot/latest/developerguide/iot-rule-actions.html#s3-rule)\.

To use \* to refer to the message payload as raw binary data, follow these rules:

1. The SQL statement and templates must not refer to JSON names other than \*\. 

1. The `SELECT` statement must have \* as the only item, or must have only functions\. See the following example\.

   ```
   SELECT * FROM 'a/b'
   ```

   ```
   SELECT encode(*, 'base64') AS data, timestamp() AS ts FROM 'a/b'
   ```

For rule actions that don't support binary payload input, such as [Lambda action](https://docs.aws.amazon.com/iot/latest/developerguide/iot-rule-actions.html#lambda-rule), you must decode binary payloads\. The Lambda rule action can receive binary data if it's base64 encoded and in a JSON payload\. You can do this by changing the rule to:

```
SELECT encode(*, 'base64') AS data FROM 'my_topic'
```

## Binary payload examples<a name="binary-payloads-examples"></a>

You can use the following `SELECT` clause with binary payloads because it doesn't refer to any JSON names\. 

```
SELECT * FROM 'a/b'
```

You cannot use the following `SELECT` with binary payloads because it refers to `device_type` in the WHERE clause\.

```
SELECT * FROM 'a/b' WHERE device_type = 'thermostat'
```

You cannot use the following `SELECT` with binary payloads because it violates rule \#2\.

```
SELECT *, timestamp() AS timestamp FROM 'a/b'
```

You can use the following `SELECT` with binary payloads because it complies with rule \#1 or rule \#2\.

```
SELECT * FROM 'a/b' WHERE timestamp() % 12 = 0
```

You cannot use the following AWS IoT rule with binary payloads because it violates rule \#1\.

```
{
    "sql": "SELECT * FROM 'a/b'"
    "actions": [{
        "republish": {
            "topic":"device/${device_id}"
         }
     }]
}
```