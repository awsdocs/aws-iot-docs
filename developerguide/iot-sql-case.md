# Case statements<a name="iot-sql-case"></a>

Case statements can be used for branching execution, like a switch statement, or if/else statements\.

Syntax:

```
CASE v WHEN t[1] THEN r[1] 
  WHEN t[2] THEN r[2] ... 
  WHEN t[n] THEN r[n] 
  ELSE r[e] END
```

The expression `v` is evaluated and matched for equality against each `t[i]` expression\. If a match is found, the corresponding `r[i]` expression becomes the result of the case statement\. If there is more than one possible match, the first match is selected\. If there are no matches, the else statement's `re` is used as the result\. If there is no match and no else statement, the result of the case statement is `Undefined`\. For example:

Incoming payload published on topic `a/b`: `{"color":"yellow"}` 

SQL statement: `SELECT CASE color WHEN 'green' THEN 'go' WHEN 'yellow' THEN 'caution' WHEN 'red' THEN 'stop' ELSE 'you are not at a stop light' END as instructions FROM 'a/b'`

The resulting output payload would be: `{"instructions":"caution"}`\.

Case statements require at least one WHEN clause\. An ELSE clause is not required\.

**Note**  
If v is `Undefined`, the result of the case statement is `Undefined`\.