# SELECT clause<a name="iot-sql-select"></a>

The AWS IoT SELECT clause is essentially the same as the ANSI SQL SELECT clause, with some minor differences\.

The SELECT clause supports [Data types](iot-sql-data-types.md), [Operators](iot-sql-operators.md), [Functions](iot-sql-functions.md), [Literals](iot-sql-literals.md), [Case statements](iot-sql-case.md), [JSON extensions](iot-sql-json.md), [Substitution templates](iot-substitution-templates.md), [Nested object queries](iot-sql-nested-queries.md), and [Binary payloads](binary-payloads.md)\.

You can use the SELECT clause to extract information from incoming MQTT messages\. You can also use `SELECT *` to retrieve the entire incoming message payload\. For example:

```
Incoming payload published on topic 'topic/subtopic': {"color":"red", "temperature":50}
SQL statement: SELECT * FROM 'topic/subtopic'
Outgoing payload: {"color":"red", "temperature":50}
```

If the payload is a JSON object, you can reference keys in the object\. Your outgoing payload contains the key\-value pair\. For example:

```
Incoming payload published on topic 'topic/subtopic': {"color":"red", "temperature":50}
SQL statement: SELECT color FROM 'topic/subtopic'
Outgoing payload: {"color":"red"}
```

You can use the AS keyword to rename keys\. For example:

```
Incoming payload published on topic 'topic/subtopic':{"color":"red", "temperature":50}
SQL:SELECT color AS my_color FROM 'topic/subtopic'
Outgoing payload: {"my_color":"red"}
```

You can select multiple items by separating them with a comma\. For example:

```
Incoming payload published on topic 'topic/subtopic': {"color":"red", "temperature":50}
SQL: SELECT color as my_color, temperature as fahrenheit FROM 'topic/subtopic'
Outgoing payload: {"my_color":"red","fahrenheit":50}
```

You can select multiple items including '\*' to add items to the incoming payload\. For example:

```
Incoming payload published on topic 'topic/subtopic': {"color":"red", "temperature":50}
SQL: SELECT *, 15 as speed FROM 'topic/subtopic'
Outgoing payload: {"color":"red", "temperature":50, "speed":15}
```

You can use the `"VALUE"` keyword to produce outgoing payloads that are not JSON objects\. With SQL version `2015-10-08`, you can select only one item\. With SQL version `2016-03-23` or later, you can also select an array to output as a top\-level object\.

**Example**  

```
Incoming payload published on topic 'topic/subtopic': {"color":"red", "temperature":50}
SQL: SELECT VALUE color FROM 'topic/subtopic'
Outgoing payload: "red"
```

You can use `'.'` syntax to drill into nested JSON objects in the incoming payload\. For example:

```
Incoming payload published on topic 'topic/subtopic': {"color":{"red":255,"green":0,"blue":0}, "temperature":50}
SQL: SELECT color.red as red_value FROM 'topic/subtopic'
Outgoing payload: {"red_value":255}
```

For information about how to use JSON object and property names that include reserved characters, such as numbers or the hyphen \(minus\) character, see [JSON extensions](iot-sql-json.md)

You can use functions \(see [Functions](iot-sql-functions.md)\) to transform the incoming payload\. You can use parentheses for grouping\. For example:

```
Incoming payload published on topic 'topic/subtopic': {"color":"red", "temperature":50}
SQL: SELECT (temperature - 32) * 5 / 9 AS celsius, upper(color) as my_color FROM 'topic/subtopic'
Outgoing payload: {"celsius":10,"my_color":"RED"}
```
