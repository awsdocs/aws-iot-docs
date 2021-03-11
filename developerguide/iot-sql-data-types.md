# Data types<a name="iot-sql-data-types"></a>

The AWS IoT rules engine supports all JSON data types\.


**Supported data types**  

| Type | Meaning | 
| --- | --- | 
| Int | A discrete Int\. 34 digits maximum\. | 
| Decimal |  A `Decimal` with a precision of 34 digits, with a minimum non\-zero magnitude of 1E\-999 and a maximum magnitude 9\.999\.\.\.E999\.  Some functions return `Decimal` values with double precision rather than 34\-digit precision\.  With SQL V2 \(2016\-03\-23\), numeric values that are whole numbers, such as `10.0`, are processed as an `Int` value \(`10`\) instead of the expected `Decimal` value \(`10.0`\)\. To reliably process whole number numeric values as `Decimal` values, use SQL V1 \(2015\-10\-08\) for the rule query statement\.   | 
| Boolean | True or False\. | 
| String | A UTF\-8 string\. | 
| Array | A series of values that don't have to have the same type\. | 
| Object | A JSON value consisting of a key and a value\. Keys must be strings\. Values can be any type\. | 
| Null | Null as defined by JSON\. It's an actual value that represents the absence of a value\. You can explicitly create a Null value by using the Null keyword in your SQL statement\. For example: "SELECT NULL AS n FROM 'topic/subtopic'"  | 
| Undefined |  Not a value\. This isn't explicitly representable in JSON except by omitting the value\. For example, in the object `{"foo": null}`, the key "foo" returns NULL, but the key "bar" returns `Undefined`\. Internally, the SQL language treats `Undefined` as a value, but it isn't representable in JSON, so when serialized to JSON, the results are `Undefined`\. <pre> {"foo":null, "bar":undefined} </pre> is serialized to JSON as: <pre> {"foo":null}</pre> Similarly, `Undefined` is converted to an empty string when serialized by itself\. Functions called with invalid arguments \(for example, wrong types, wrong number of arguments, and so on\) return `Undefined`\.   | 

## Conversions<a name="iot-sql-conversions"></a>

The following table lists the results when a value of one type is converted to another type \(when a value of the incorrect type is given to a function\)\. For example, if the absolute value function "abs" \(which expects an `Int` or `Decimal`\) is given a `String`, it attempts to convert the `String` to a `Decimal`, following these rules\. In this case, 'abs\("\-5\.123"\)' is treated as 'abs\(\-5\.123\)'\.

**Note**  
There are no attempted conversions to `Array`, `Object`, `Null`, or `Undefined`\.


**To decimal**  

| Argument type | Result | 
| --- | --- | 
| Int | A Decimal with no decimal point\. | 
| Decimal | The source value\. | 
| Boolean | Undefined\. \(You can explicitly use the cast function to transform true = 1\.0, false = 0\.0\.\) | 
| String | The SQL engine tries to parse the string as a Decimal\. AWS IoT attempts to parse strings matching the regular expression:^\-?\\d\+\(\\\.\\d\+\)?\(\(?i\)E\-?\\d\+\)?$\. "0", "\-1\.2", "5E\-12" are all examples of strings that are converted automatically to Decimals\. | 
| Array | Undefined\. | 
| Object | Undefined\. | 
| Null | Null\. | 
| Undefined | Undefined\. | 


**To int**  

| Argument type | Result | 
| --- | --- | 
| Int | The source value\. | 
| Decimal | The source value rounded to the nearest Int\. | 
| Boolean | Undefined\. \(You can explicitly use the cast function to transform true = 1\.0, false = 0\.0\.\) | 
| String |  The SQL engine tries to parse the string as a Decimal\. AWS IoT attempts to parse strings matching the regular expression:^\-?\\d\+\(\\\.\\d\+\)?\(\(?i\)E\-?\\d\+\)?$\. "0", "\-1\.2", "5E\-12" are all examples of strings that are converted automatically to Decimals\. AWS IoT attempts to convert the String to a Decimal, and then truncates the decimal places of that Decimal to make an Int\. | 
| Array | Undefined\. | 
| Object | Undefined\. | 
| Null | Null\. | 
| Undefined | Undefined\. | 


**To Boolean**  

| Argument type | Result | 
| --- | --- | 
| Int | Undefined\. \(You can explicitly use the cast function to transform 0 = False, any\_nonzero\_value = True\.\) | 
| Decimal | Undefined\. \(You can explicitly use the cast function to transform 0 = False, any\_nonzero\_value = True\.\) | 
| Boolean | The original value\. | 
| String | "true"=True and "false"=False \(case insensitive\)\. Other string values are Undefined\. | 
| Array | Undefined\. | 
| Object | Undefined\. | 
| Null | Undefined\. | 
| Undefined | Undefined\. | 


**To string**  

| Argument type | Result | 
| --- | --- | 
| Int | A string representation of the Int in standard notation\. | 
| Decimal | A string representing the Decimal value, possibly in scientific notation\.  | 
| Boolean | "true" or "false"\. All lowercase\. | 
| String | The original value\. | 
| Array | The Array serialized to JSON\. The resultant string is a comma\-separated list, enclosed in square brackets\. A String is quoted\. A Decimal, Int, Boolean, and Null is not\. | 
| Object | The object serialized to JSON\. The resultant string is a comma\-separated list of key\-value pairs and begins and ends with curly braces\. A String is quoted\. A Decimal, Int, Boolean, and Null is not\. | 
| Null | Undefined\. | 
| Undefined | Undefined\. | 