# SELECT clause<a name="iot-sql-select"></a>

The AWS IoT SELECT clause is essentially the same as the ANSI SQL SELECT clause, with some minor differences\.

The SELECT clause supports [Data types](iot-sql-data-types.md), [Operators](iot-sql-operators.md), [Functions](iot-sql-functions.md), [Literals](iot-sql-literals.md), [Case statements](iot-sql-case.md), [JSON extensions](iot-sql-json.md), [Substitution templates](iot-substitution-templates.md), and [Nested object queries](iot-sql-nested-queries.md)\.

You can use the SELECT clause to extract information from incoming MQTT messages\. You can also use `SELECT *` to retrieve the entire incoming message payload\. For example:

```
Incoming payload published on topic 'a/b': {"color":"red", "temperature":50}
SQL statement: SELECT * FROM 'a/b'
Outgoing payload: {"color":"red", "temperature":50}
```

If the payload is a JSON object, you can reference keys in the object\. Your outgoing payload contains the key\-value pair\. For example:

```
Incoming payload published on topic 'a/b': {"color":"red", "temperature":50}
SQL statement: SELECT color FROM 'a/b'
Outgoing payload: {"color":"red"}
```

You can use the AS keyword to rename keys\. For example:

```
Incoming payload published on topic 'a/b':{"color":"red", "temperature":50}
SQL:SELECT color AS my_color FROM 'a/b'
Outgoing payload: {"my_color":"red"}
```

You can select multiple items by separating them with a comma\. For example:

```
Incoming payload published on topic 'a/b': {"color":"red", "temperature":50}
SQL: SELECT color as my_color, temperature as fahrenheit FROM 'a/b'
Outgoing payload: {"my_color":"red","fahrenheit":50}
```

You can select multiple items including '\*' to add items to the incoming payload\. For example:

```
Incoming payload published on topic 'a/b': {"color":"red", "temperature":50}
SQL: SELECT *, 15 as speed FROM 'a/b'
Outgoing payload: {"color":"red", "temperature":50, "speed":15}
```

You can use the `"VALUE"` keyword to produce outgoing payloads that are not JSON objects\. With SQL version `2015-10-08`, you can select only one item\. With SQL version `2016-03-23` or later, you can also select an array to output as a top\-level object\.

**Example**  

```
Incoming payload published on topic 'a/b': {"color":"red", "temperature":50}
SQL: SELECT VALUE color FROM 'a/b'
Outgoing payload: "red"
```

You can use `'.'` syntax to drill into nested JSON objects in the incoming payload\. For example:

```
Incoming payload published on topic 'a/b': {"color":{"red":255,"green":0,"blue":0}, "temperature":50}
SQL: SELECT color.red as red_value FROM 'a/b'
Outgoing payload: {"red_value":255}
```

You can use functions \(see [Functions](iot-sql-functions.md)\) to transform the incoming payload\. You can use parentheses for grouping\. For example:

```
Incoming payload published on topic 'a/b': {"color":"red", "temperature":50}
SQL: SELECT (temperature – 32) * 5 / 9 AS celsius, upper(color) as my_color FROM 'a/b'
Outgoing payload: {"celsius":10,"my_color":"RED"}
```

## Working with binary payloads<a name="binary-payloads"></a>

When the message payload should be handled as raw binary data, rather than a JSON object, use the \* operator to refer to it in a `SELECT` clause\. This works for non\-JSON payloads with some rule actions, such as the [S3 action](https://docs.aws.amazon.com/iot/latest/developerguide/iot-rule-actions.html#s3-rule)\.

To use \* to refer to the message payload as raw binary data, follow these rules:

1. The SQL statement and templates must not refer to JSON names other than \*\. 

1. The SELECT statement must have \* as the only item, or must have only functions, for example:

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

### Binary payload examples<a name="binary-payloads-examples"></a>

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