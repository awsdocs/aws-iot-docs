# WHERE clause<a name="iot-sql-where"></a>

The WHERE clause determines if the actions specified by a rule are carried out\. If the WHERE clause evaluates to true, the rule actions are performed\. Otherwise, the rule actions are not performed\. 

The WHERE clause supports [Data types](iot-sql-data-types.md), [Operators](iot-sql-operators.md), [Functions](iot-sql-functions.md), [Literals](iot-sql-literals.md), [Case statements](iot-sql-case.md), [JSON extensions](iot-sql-json.md), [Substitution templates](iot-substitution-templates.md), and [Nested object queries](iot-sql-nested-queries.md)\.

Example:

Incoming payload published on `topic/subtopic`: `{"color":"red", "temperature":40}`\.

SQL: `SELECT color AS my_color FROM 'topic/subtopic' WHERE temperature > 50 AND color <> 'red'`\.

In this case, the rule would be triggered, but the actions specified by the rule would not be performed\. There would be no outgoing payload\.

You can use functions and operators in the WHERE clause\. However, you cannot reference any aliases created with the AS keyword in the SELECT\. The WHERE clause is evaluated first, to determine if SELECT is evaluated\. 

Example with non-JSON payload:

Incoming non-JSON payload published on `topic/subtopic`: `80`\.

SQL: `SELECT decode(encode(*, 'base64'), 'base64') AS value FROM 'topic/subtopic' WHERE decode(encode(*, 'base64'), 'base64') > 50`\.

In this case, the rule would be triggered, and the actions specified by the rule would be performed\. Outgoing payload would be transformed by the SELECT clause as a JSON payload `{"value":80}`\.
