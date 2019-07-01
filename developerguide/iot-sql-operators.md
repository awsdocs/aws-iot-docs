# Operators<a name="iot-sql-operators"></a>

The following operators can be used in SELECT and WHERE clauses\. 

## AND operator<a name="iot-sql-operators-and"></a>

Returns a `Boolean` result\. Performs a logical AND operation\. Returns true if left and right operands are true\. Otherwise, returns false\. `Boolean` operands or case insensitive "true" or "false" string operands are required\.

*Syntax:* ` expression AND expression`\.


**AND Operator**  

| Left Operand | Right Operand | Output | 
| --- | --- | --- | 
| Boolean | Boolean | Boolean\. True if both operands are true\. Otherwise, false\. | 
| String/Boolean | String/Boolean | If all strings are "true" or "false" \(case insensitive\), they are converted to Boolean and processed normally as boolean AND boolean\. | 
| Other Value | Other Value | Undefined\. | 

## OR operator<a name="iot-sql-operators-or"></a>

Returns a `Boolean` result\. Performs a logical OR operation\. Returns true if either the left or the right operands are true\. Otherwise, returns false\. `Boolean` operands or case insensitive "true" or "false" string operands are required\.

*Syntax:* ` expression OR expression`\.


**OR Operator**  

| Left Operand | Right Operand | Output | 
| --- | --- | --- | 
| Boolean | Boolean | Boolean\. True if either operand is true\. Otherwise, false\. | 
| String/Boolean | String/Boolean | If all strings are "true" or "false" \(case insensitive\), they are converted to Booleans and processed normally as boolean OR boolean\. | 
| Other Value | Other Value | Undefined\. | 

## NOT operator<a name="iot-sql-operators-not"></a>

Returns a `Boolean` result\. Performs a logical NOT operation\. Returns true if the operand is false\. Otherwise, returns true\. A `Boolean` operand or case insensitive "true" or "false" string operand is required\.

*Syntax:* `NOT expression`\.


**NOT Operator**  

| Operand | Output | 
| --- | --- | 
| Boolean | Boolean\. True if operand is false\. Otherwise, true\. | 
| String | If string is "true" or "false" \(case insensitive\), it is converted to the corresponding boolean value, and the opposite value is returned\. | 
| Other Value | Undefined\. | 

## > operator<a name="iot-sql-operators-greater"></a>

Returns a `Boolean` result\. Returns true if the left operand is greater than the right operand\. Both operands are converted to a `Decimal`, and then compared\. 

*Syntax:* `expression > expression`\.


**> Operator**  

| Left Operand | Right Operand | Output | 
| --- | --- | --- | 
| Int/Decimal | Int/Decimal | Boolean\. True if the left operand is greater than the right operand\. Otherwise, false\. | 
| String/Int/Decimal | String/Int/Decimal | If all strings can be converted to Decimal, then Boolean\. Returns true if the left operand is greater than the right operand\. Otherwise, false\. | 
| Other Value | Undefined\. | Undefined\. | 

## >= operator<a name="iot-sql-operators-greater-equal"></a>

Returns a `Boolean` result\. Returns true if the left operand is greater than or equal to the right operand\. Both operands are converted to a `Decimal`, and then compared\. 

*Syntax:* `expression >= expression`\.


**>= Operator**  

| Left Operand | Right Operand | Output | 
| --- | --- | --- | 
| Int/Decimal | Int/Decimal | Boolean\. True if the left operand is greater than or equal to the right operand\. Otherwise, false\. | 
| String/Int/Decimal | String/Int/Decimal | If all strings can be converted to Decimal, then Boolean\. Returns true if the left operand is greater than or equal to the right operand\. Otherwise, false\. | 
| Other Value | Undefined\. | Undefined\. | 

## < operator<a name="iot-sql-operators-less"></a>

Returns a `Boolean` result\. Returns true if the left operand is less than the right operand\. Both operands are converted to a `Decimal`, and then compared\. 

*Syntax:* `expression < expression`\.


**< Operator**  

| Left Operand | Right Operand | Output | 
| --- | --- | --- | 
| Int/Decimal | Int/Decimal | Boolean\. True if the left operand is less than the right operand\. Otherwise, false\. | 
| String/Int/Decimal | String/Int/Decimal | If all strings can be converted to Decimal, then Boolean\. Returns true if the left operand is less than the right operand\. Otherwise, false\. | 
| Other Value | Undefined | Undefined | 

## <= operator<a name="iot-sql-operators-less-equal"></a>

Returns a `Boolean` result\. Returns true if the left operand is less than or equal to the right operand\. Both operands are converted to a `Decimal`, and then compared\. 

*Syntax:* `expression <= expression`\.


**<= Operator**  

| Left Operand | Right Operand | Output | 
| --- | --- | --- | 
| Int/Decimal | Int/Decimal | Boolean\. True if the left operand is less than or equal to the right operand\. Otherwise, false\. | 
| String/Int/Decimal | String/Int/Decimal | If all strings can be converted to Decimal, then Boolean\. Returns true if the left operand is less than or equal to the right operand\. Otherwise, false\. | 
| Other Value | Undefined | Undefined | 

## <> operator<a name="iot-sql-operators-not-eq"></a>

Returns a `Boolean` result\. Returns true if both left and right operands are not equal\. Otherwise, returns false\. 

*Syntax:* ` expression <> expression`\.


**<> Operator**  

| Left Operand | Right Operand | Output | 
| --- | --- | --- | 
| Int | Int | True if left operand is not equal to right operand\. Otherwise, false\. | 
| Decimal | Decimal | True if left operand is not equal to right operand\. Otherwise, false\.Int is converted to Decimal before being compared\. | 
| String | String | True if left operand is not equal to right operand\. Otherwise, false\. | 
| Array | Array | True if the items in each operand are not equal and not in the same order\. Otherwise, false | 
| Object | Object | True if the keys and values of each operand are not equal\. Otherwise, false\. The order of keys/values is unimportant\. | 
| Null | Null | False\. | 
| Any Value | Undefined | Undefined\. | 
| Undefined | Any Value | Undefined\. | 
| Mismatched Type | Mismatched Type | True\. | 

## = operator<a name="iot-sql-operators-eq"></a>

Returns a `Boolean` result\. Returns true if both left and right operands are equal\. Otherwise, returns false\. 

*Syntax:* ` expression = expression`\.


**= Operator**  

| Left Operand | Right Operand | Output | 
| --- | --- | --- | 
| Int | Int | True if left operand is equal to right operand\. Otherwise, false\. | 
| Decimal | Decimal | True if left operand is equal to right operand\. Otherwise, false\.Int is converted to Decimal before being compared\. | 
| String | String | True if left operand is equal to right operand\. Otherwise, false\. | 
| Array | Array | True if the items in each operand are equal and in the same order\. Otherwise, false\. | 
| Object | Object | True if the keys and values of each operand are equal\. Otherwise, false\. The order of keys/values is unimportant\. | 
| Any Value | Undefined | Undefined\. | 
| Undefined | Any Value | Undefined\. | 
| Mismatched Type | Mismatched Type | False\. | 

## \+ operator<a name="iot-sql-operators-plus"></a>

The "\+" is an overloaded operator\. It can be used for string concatenation or addition\. 

*Syntax:* ` expression + expression`\.


**\+ Operator**  

| Left Operand | Right Operand | Output | 
| --- | --- | --- | 
| String | Any Value | Converts the right operand to a string and concatenates it to the end of the left operand\. | 
| Any Value | String | Converts the left operand to a string and concatenates the right operand to the end of the converted left operand\. | 
| Int | Int | Int value\. Adds operands together\. | 
| Int/Decimal | Int/Decimal | Decimal value\. Adds operands together\. | 
| Other Value | Other Value | Undefined\. | 

## \- operator<a name="iot-sql-operators-sub"></a>

Subtracts the right operand from the left operand\. 

*Syntax:* ` expression - expression`\.


**\- Operator**  

| Left Operand | Right Operand | Output | 
| --- | --- | --- | 
| Int | Int | Int value\. Subtracts right operand from left operand\. | 
| Int/Decimal | Int/Decimal | Decimal value\. Subtracts right operand from left operand\. | 
| String/Int/Decimal | String/Int/Decimal | If all strings convert to decimals correctly, a Decimal value is returned\. Subtracts right operand from left operand\. Otherwise, returns Undefined\. | 
| Other Value | Other value | Undefined\. | 
| Other Value | Other Value | Undefined\. | 

## \* operator<a name="iot-sql-operators-mult"></a>

Multiplies the left operand by the right operand\. 

*Syntax:* ` expression * expression`\.


**\* Operator**  

| Left Operand | Right Operand | Output | 
| --- | --- | --- | 
| Int | Int | Int value\. Multiplies the left operand by the right operand\. | 
| Int/Decimal | Int/Decimal | Decimal value\. Multiplies the left operand by the right operand\. | 
| String/Int/Decimal | String/Int/Decimal | If all strings convert to decimals correctly, a Decimal value is returned\. Multiplies the left operand by the right operand\. Otherwise, returns Undefined\. | 
| Other Value | Other value | Undefined\. | 

## / operator<a name="iot-sql-operators-div"></a>

Divides the left operand by the right operand\. 

*Syntax:* ` expression / expression`\.


**/ Operator**  

| Left Operand | Right Operand | Output | 
| --- | --- | --- | 
| Int | Int | Int value\. Divides the left operand by the right operand\. | 
| Int/Decimal | Int/Decimal | Decimal value\. Divides the left operand by the right operand\. | 
| String/Int/Decimal | String/Int/Decimal | If all strings convert to decimals correctly, a Decimal value is returned\. Divides the left operand by the right operand\. Otherwise, returns Undefined\. | 
| Other Value | Other value | Undefined\. | 

## % operator<a name="iot-sql-operators-mod"></a>

Returns the remainder from dividing the left operand by the right operand\. 

*Syntax:* ` expression % expression`\.


**% Operator**  

| Left Operand | Right Operand | Output | 
| --- | --- | --- | 
| Int | Int | Int value\. Returns the remainder from dividing the left operand by the right operand\. | 
| String/Int/Decimal | String/Int/Decimal | If all strings convert to decimals correctly, a Decimal value is returned\. Returns the remainder from dividing the left operand by the right operand\. Otherwise, Undefined\. | 
| Other Value | Other value | Undefined\. | 