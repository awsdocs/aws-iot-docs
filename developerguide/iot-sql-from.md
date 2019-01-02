# FROM Clause<a name="iot-sql-from"></a>

The FROM clause subscribes your rule to a topic or topic filter\. A topic filter allows you to subscribe to a group of similar topics\.

Example:

Incoming payload published on topic `'a/b'`: `{temperature: 50}`

Incoming payload published on topic `'a/c'`: `{temperature: 50}`

SQL: `"SELECT temperature AS t FROM 'a/b'"`\.

The rule is subscribed to `'a/b'`, so the incoming payload is passed to the rule, and the outgoing payload \(passed to the rule actions\) is: `{t: 50}`\. The rule is not subscribed to `'a/c'`, so the rule is not triggered for the message published on `'a/c'`\.

You can use the \# wildcard character to match any subpath in a topic filter:

Example:

Incoming payload published on topic `'a/b'`: `{temperature: 50}`\.

Incoming payload published on topic `'a/c'`: `{temperature: 60}`\.

Incoming payload published on topic `'a/e/f'`: `{temperature: 70}`\.

Incoming payload published on topic `'b/x'`: `{temperature: 80}`\.

SQL: `"SELECT temperature AS t FROM 'a/#'"`\.

The rule is subscribed to any topic beginning with `'a'`, so it is executed three times, sending outgoing payloads of `{t: 50} (for a/b), {t: 60}` \(for a/c\), and `{t: 70}` \(for a/e/f\) to its actions\. It is not subscribed to `'b/x'`, so the rule will not be triggered for the \{temperature: 80\} message\.

You can use the '\+' character to match any one particular path element:

Example:

Incoming payload published on topic `'a/b'`: `{temperature: 50}`\.

Incoming payload published on topic `'a/c'`: `{temperature: 60}`\.

Incoming payload published on topic `'a/e/f'`: `{temperature: 70}`\.

Incoming payload published on topic `'b/x'`: `{temperature: 80}`\.

SQL: `"SELECT temperature AS t FROM 'a/+'"`\.

The rule is subscribed to all topics with two path elements where the first element is `'a'`\. The rule is executed for the messages sent to `'a/b'` and `'a/c'`, but not `'a/e/f'` or `'b/x'`\.

You can use functions and operators in the WHERE clause\. In the WHERE clause, you cannot reference any aliases created with the AS keyword in the SELECT\. \(The WHERE clause is evaluated first, to determine if the SELECT clause is evaluated\.\)