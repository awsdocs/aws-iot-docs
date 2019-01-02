# AWS IoT SQL Reference<a name="iot-sql-reference"></a>

In AWS IoT, rules are defined using an SQL\-like syntax\. SQL statements are composed of three types of clauses:

** SELECT **  
Required\. Extracts information from the incoming message payload and performs transformations\.

**FROM**  
The MQTT message topic filter\. When a message with a matching topic is received through the AWS IoT message broker, the rule is triggered\. Required for rules that will be triggered by messages that pass through the message broker; Optional for rules that will only be triggered using the [Basic Ingest](iot-basic-ingest.md) feature\. 

** WHERE**  
Optional\. Adds conditional logic that determines if a rule is evaluated and its actions are executed\.

An example SQL statement looks like this:

```
SELECT color AS rgb FROM 'a/b' WHERE temperature > 50
```

An example MQTT message \(also called an incoming payload\) looks like this:

```
{
    "color":"red",
    "temperature":100
}
```

If this message is published on the `'a/b'` topic, the rule is triggered and the SQL statement is evaluated\. The SQL statement extracts the value of the `color` property if the `"temperature"` property is greater than 50\. The WHERE clause specifies the condition `temperature > 50`\. The `AS` keyword renames the `"color"` property to `"rgb"`\. The result \(also called an outgoing payload\) looks like this:

```
{
    "rgb":"red"
}
```

This data is then forwarded to the rule's action, which sends the data for more processing\. For more information about rule actions, see [AWS IoT Rule Actions](iot-rule-actions.md)\.