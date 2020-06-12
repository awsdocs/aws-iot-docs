# Substitution templates<a name="iot-substitution-templates"></a>

You can use a substitution template to augment the JSON data returned when a rule is triggered and AWS IoT performs an action\. The syntax for a substitution template is `${`*expression*`}`, where *expression* can be any expression supported by AWS IoT in a SELECT or WHERE clause\. This includes functions, operators, and information present in the original message payload\.

**Important**  
Because an expression in a substitution template is evaluated separately from the "SELECT \.\.\." statement, you cannot reference an alias created using the AS clause\. You can reference only information present in the original payload, in addition to supported functions and operators\.

For more information about supported expressions, see [AWS IoT SQL reference](iot-sql-reference.md)\.

Substitution templates appear in the action parameters within a rule: 

```
{
    "sql": "SELECT *, timestamp() AS timestamp FROM 'my/iot/topic'",
    "ruleDisabled": false,
    "actions": [{
        "republish": {
            "topic": "${topic()}/republish",
            "roleArn": "arn:aws:iam::123456789012:role/my-iot-role"
        }
    }]
}
```

If this rule is triggered by the following JSON published to `my/iot/topic`:

```
{
    "deviceid": "iot123",
    "temp": 54.98,
    "humidity": 32.43,
    "coords": {
        "latitude": 47.615694,
        "longitude": -122.3359976
    }
}
```

Then this rule publishes the following JSON to `my/iot/topic/republish`, which AWS IoT substitutes from `${topic()}/republish`:

```
{
    "deviceid": "iot123",
    "temp": 54.98,
    "humidity": 32.43,
    "coords": {
        "latitude": 47.615694,
        "longitude": -122.3359976
    },
    "timestamp": 1579637878451
}
```