# WHERE Clause<a name="iot-sql-where"></a>

The WHERE clause determines if a rule is evaluated for a message sent to an MQTT topic to which the rule is subscribed\. If the WHERE clause evaluates to true, the rule is evaluated\. Otherwise, the rule is not evaluated\.

Example:

Incoming payload published on `a/b`: `{"color":"red", "temperature":40}`\.

SQL: `SELECT color AS my_color FROM 'a/b' WHERE temperature > 50 AND color <> 'red'`\.

In this case, the rule would not be evaluated; there would be no outgoing payload; and rules actions would not be triggered\.

You can use functions and operators in the WHERE clause\. However, you cannot reference any aliases created with the AS keyword in the SELECT\. \(The WHERE clause is evaluated first, to determine if SELECT is evaluated\.\) 