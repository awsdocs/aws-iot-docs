# WHERE Clause<a name="iot-sql-where"></a>

The WHERE clause determines if the actions specified by a rule are carried out\. If the WHERE clause evaluates to true, the rule actions are performed\. Otherwise, the rule actions are not performed\. 

Example:

Incoming payload published on `a/b`: `{"color":"red", "temperature":40}`\.

SQL: `SELECT color AS my_color FROM 'a/b' WHERE temperature > 50 AND color <> 'red'`\.

In this case, the rule would be triggered, but the actions specified by the rule would not be performed\. There would be no outgoing payload\.

You can use functions and operators in the WHERE clause\. However, you cannot reference any aliases created with the AS keyword in the SELECT\. \(The WHERE clause is evaluated first, to determine if SELECT is evaluated\.\) 