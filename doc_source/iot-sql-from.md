# FROM clause<a name="iot-sql-from"></a>

The FROM clause subscribes your rule to a [topic](topics.md#topicnames) or [topic filter](topics.md#topicfilters)\. You must enclose the topic or topic filter in single quotes \('\)\. The rule is triggered for each message sent to an MQTT topic that matches the topic filter specified here\. A topic filter allows you to subscribe to a group of similar topics\.

**Example:**

Incoming payload published on topic `'topic/subtopic'`: `{temperature: 50}`

Incoming payload published on topic `'topic/subtopic-2'`: `{temperature: 50}`

SQL: `"SELECT temperature AS t FROM 'topic/subtopic'"`\.

The rule is subscribed to `'topic/subtopic'`, so the incoming payload is passed to the rule\. The outgoing payload, passed to the rule actions, is: `{t: 50}`\. The rule is not subscribed to `'topic/subtopic-2'`, so the rule is not triggered for the message published on `'topic/subtopic-2'`\.

**\# Wildcard Example:**

You can use the '\#' \(multi\-level\) wildcard character to match one or more particular path elements:

Incoming payload published on topic `'topic/subtopic'`: `{temperature: 50}`\.

Incoming payload published on topic `'topic/subtopic-2'`: `{temperature: 60}`\.

Incoming payload published on topic `'topic/subtopic-3/details'`: `{temperature: 70}`\.

Incoming payload published on topic `'topic-2/subtopic-x'`: `{temperature: 80}`\.

SQL: `"SELECT temperature AS t FROM 'topic/#'"`\.

The rule is subscribed to any topic that begins with `'topic'`, so it is executed three times, sending outgoing payloads of `{t: 50}` \(for topic/subtopic\), `{t: 60}` \(for topic/subtopic\-2\), and `{t: 70}` \(for topic/subtopic\-3/details\) to its actions\. It is not subscribed to `'topic-2/subtopic-x'`, so the rule is not triggered for the `{temperature: 80}` message\.

**\+ Wildcard Example:**

You can use the '\+' \(single\-level\) wildcard character to match any one particular path element:

Incoming payload published on topic `'topic/subtopic'`: `{temperature: 50}`\.

Incoming payload published on topic `'topic/subtopic-2'`: `{temperature: 60}`\.

Incoming payload published on topic `'topic/subtopic-3/details'`: `{temperature: 70}`\.

Incoming payload published on topic `'topic-2/subtopic-x'`: `{temperature: 80}`\.

SQL: `"SELECT temperature AS t FROM 'topic/+'"`\.

The rule is subscribed to all topics with two path elements where the first element is `'topic'`\. The rule is executed for the messages sent to `'topic/subtopic'` and `'topic/subtopic-2'`, but not `'topic/subtopic-3/details'` \(it has more levels than the topic filter\) or `'topic-2/subtopic-x'` \(it does not start with `topic`\)\.