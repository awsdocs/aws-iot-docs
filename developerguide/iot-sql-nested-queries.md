# Nested Object Queries<a name="iot-sql-nested-queries"></a>

You can use nested SELECT clauses to query for attributes within arrays and inner JSON objects\. Supported by SQL version 2016\-03\-23 and later\.

Consider the following MQTT message:

```
{ 
    "e": [
        { "n": "temperature", "u": "Cel", "t": 1234, "v": 22.5 },
        { "n": "light", "u": "lm", "t": 1235, "v": 135 },
        { "n": "acidity", "u": "pH", "t": 1235, "v": 7 }
    ]
}
```

**Example**  
You can convert values to a new array with the following rule\.  

```
SELECT (SELECT VALUE n FROM e) as sensors FROM 'my/topic'
```

The rule generates the following output\.

```
{
    "sensors": [
        "temperature",
        "light",
        "acidity"
    ]
}
```

**Example**  
Using the same MQTT message, you can also query a specific value within a nested object with the following rule\.  

```
SELECT (SELECT v FROM e WHERE n = 'temperature') as temperature FROM 'my/topic'
```

The rule generates the following output\.

```
{
    "temperature": [
        {
            "v": 22.5
        }
    ]
}
```

**Example**  
You can also flatten the output with a more complicated rule\.  

```
SELECT get((SELECT v FROM e WHERE n = 'temperature'), 0).v as temperature FROM 'topic'
```

The rule generates the following output\.

```
{
    "temperature": 22.5
}
```