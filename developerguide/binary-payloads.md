# Working with binary payloads<a name="binary-payloads"></a>

If you send a raw binary payload, AWS IoT Core routes it downstream to an Amazon S3 bucket through an [S3 action](https://docs.aws.amazon.com/iot/latest/developerguide/iot-rule-actions.html#s3-rule)\. The raw binary payload is then encoded as base64 and attached to JSON\. To handle your message payload as raw binary data \(rather than a JSON object\), you can use the \* operator to refer to it in a SELECT clause\. 

## Binary payload examples<a name="binary-payloads-examples"></a>

When you use \* to refer to the message payload as raw binary data, you can add data to the rule\. If you have an empty or a JSON payload, the resulting payload can have data added using the rule\. The following shows examples of supported `SELECT` clauses\.
+ You can use the following `SELECT` clauses with only a \* for binary payloads\.
  + 

    ```
    SELECT * FROM 'topic/subtopic'
    ```
  + 

    ```
    SELECT * FROM 'topic/subtopic' WHERE timestamp() % 12 = 0
    ```
+ You can also add data and use the following `SELECT` clauses\.
  + 

    ```
    SELECT *, principal() as principal, timestamp() as time FROM 'topic/subtopic'
    ```
  + 

    ```
    SELECT encode(*, 'base64') AS data, timestamp() AS ts FROM 'topic/subtopic'
    ```
+ You can also use these `SELECT` clauses with binary payloads\.
  + The following refers to `device_type` in the WHERE clause\.

    ```
    SELECT * FROM 'topic/subtopic' WHERE device_type = 'thermostat'
    ```
  + The following is also supported\.

    ```
    {
        "sql": "SELECT * FROM 'topic/subtopic'"
        "actions": [{
            "republish": {
                "topic":"device/${device_id}"
             }
         }]
    }
    ```

The following rule actions don't support binary payloads so you must decode them\.
+ Some rule actions don't support binary payload input, such as a [Lambda action](https://docs.aws.amazon.com/iot/latest/developerguide/iot-rule-actions.html#lambda-rule), so you must decode binary payloads\. The Lambda rule action can receive binary data, if it's base64 encoded and in a JSON payload\. You can do this by changing the rule to the following\.

  ```
  SELECT encode(*, 'base64') AS data FROM 'my_topic'
  ```
+ The SQL statement doesn't support string as input\. To convert a string input to JSON, you can run the following command\.

  ```
  SELECT decode(encode(*, 'base64'), 'base64') AS payload FROM 'topic'
  ```