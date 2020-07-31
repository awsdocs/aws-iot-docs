# AWS IoT SQL reference<a name="iot-sql-reference"></a>

In AWS IoT, rules are defined using an SQL\-like syntax\. SQL statements are composed of three types of clauses:

**SELECT**  
Required\. Extracts information from the payload of an incoming message and performs transformations on the information\. The messages to use are identified by the [topic filter](topics.md#topicfilters) specified in the FROM clause\.  
The SELECT clause supports [Data types](iot-sql-data-types.md), [Operators](iot-sql-operators.md), [Functions](iot-sql-functions.md), [Literals](iot-sql-literals.md), [Case statements](iot-sql-case.md), [JSON extensions](iot-sql-json.md), [Substitution templates](iot-substitution-templates.md), [Nested object queries](iot-sql-nested-queries.md), and [Binary payloads](binary-payloads.md)\.

**FROM**  
The MQTT message [topic filter](topics.md#topicfilters) that identifies the messages to extract data from\. The rule is triggered for each message sent to an MQTT topic that matches the topic filter specified here\. Required for rules that are triggered by messages that pass through the message broker\. Optional for rules that are only triggered using the [Basic Ingest](iot-basic-ingest.md) feature\. 

**WHERE**  
\(Optional\) Adds conditional logic that determines whether the actions specified by a rule are carried out\.   
The WHERE clause supports [Data types](iot-sql-data-types.md), [Operators](iot-sql-operators.md), [Functions](iot-sql-functions.md), [Literals](iot-sql-literals.md), [Case statements](iot-sql-case.md), [JSON extensions](iot-sql-json.md), [Substitution templates](iot-substitution-templates.md), and [Nested object queries](iot-sql-nested-queries.md)\.

An example SQL statement looks like this:

```
SELECT color AS rgb FROM 'topic/subtopic' WHERE temperature > 50
```

An example MQTT message \(also called an incoming payload\) looks like this:

```
{
    "color":"red",
    "temperature":100
}
```

If this message is published on the `'topic/subtopic'` topic, the rule is triggered and the SQL statement is evaluated\. The SQL statement extracts the value of the `color` property if the `"temperature"` property is greater than 50\. The WHERE clause specifies the condition `temperature > 50`\. The `AS` keyword renames the `"color"` property to `"rgb"`\. The result \(also called an *outgoing payload*\) looks like this:

```
{
    "rgb":"red"
}
```

This data is then forwarded to the rule's action, which sends the data for more processing\. For more information about rule actions, see [AWS IoT rule actions](iot-rule-actions.md)\.