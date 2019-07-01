# FROM Clause<a name="iot-sql-from"></a>

The FROM clause subscribes your rule to a topic or topic filter\. The topic or topic filter must be enclosed in single quotes \('\)\. The rule is triggered for each message sent to an MQTT topic that matches the topic filter specified here\. A topic filter allows you to subscribe to a group of similar topics\.

**Example:**

Incoming payload published on topic `'a/b'`: `{temperature: 50}`

Incoming payload published on topic `'a/c'`: `{temperature: 50}`

SQL: `"SELECT temperature AS t FROM 'a/b'"`\.

The rule is subscribed to `'a/b'`, so the incoming payload is passed to the rule, and the outgoing payload \(passed to the rule actions\) is: `{t: 50}`\. The rule is not subscribed to `'a/c'`, so the rule is not triggered for the message published on `'a/c'`\.

**\# Wildcard Example:**

You can use the '\#' \(multi\-level\) wildcard character to match one or more particular path elements:

Incoming payload published on topic `'a/b'`: `{temperature: 50}`\.

Incoming payload published on topic `'a/c'`: `{temperature: 60}`\.

Incoming payload published on topic `'a/e/f'`: `{temperature: 70}`\.

Incoming payload published on topic `'b/x'`: `{temperature: 80}`\.

SQL: `"SELECT temperature AS t FROM 'a/#'"`\.

The rule is subscribed to any topic beginning with `'a'`, so it is executed three times, sending outgoing payloads of `{t: 50}` \(for a/b\), `{t: 60}` \(for a/c\), and `{t: 70}` \(for a/e/f\) to its actions\. It is not subscribed to `'b/x'`, so the rule is not triggered for the `{temperature: 80}` message\.

**\+ Wildcard Example:**

You can use the '\+' \(single\-level\) wildcard character to match any one particular path element:

Incoming payload published on topic `'a/b'`: `{temperature: 50}`\.

Incoming payload published on topic `'a/c'`: `{temperature: 60}`\.

Incoming payload published on topic `'a/e/f'`: `{temperature: 70}`\.

Incoming payload published on topic `'b/x'`: `{temperature: 80}`\.

SQL: `"SELECT temperature AS t FROM 'a/+'"`\.

The rule is subscribed to all topics with two path elements where the first element is `'a'`\. The rule is executed for the messages sent to `'a/b'` and `'a/c'`, but not `'a/e/f'` or `'b/x'`\.