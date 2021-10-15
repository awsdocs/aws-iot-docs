# Example thing queries<a name="example-queries"></a>

Queries are specified in a query string using a query syntax and passed to the [https://docs.aws.amazon.com/iot/latest/apireference/API_SearchIndex.html](https://docs.aws.amazon.com/iot/latest/apireference/API_SearchIndex.html) API\. The following table lists some example query strings\.


****  

| Query string | Result | 
| --- | --- | 
|  abc  |  Queries for "abc" in any registry or shadow field\.  | 
|  thingName:myThingName  |  Queries for a thing with name "myThingName"\.  | 
|  thingName:my\*  |  Queries for things with names that begin with "my"\.  | 
|  thingName:ab?  |  Queries for things with names that have "ab" plus one additional character \(for example: "aba", "abb", "abc", and so on\.\)  | 
|  thingTypeName:aa  |  Queries for things that are associated with type aa\.  | 
|  attributes\.myAttribute:75  |  Queries for things with an attribute named "myAttribute" that has the value 75\.  | 
|  attributes\.myAttribute:\[75 TO 80\]  |  Queries for things with an attribute named "myAttribute" whose value falls within a numeric range \(75–80, inclusive\)\.  | 
|  attributes\.myAttribute:\{75 TO 80\]  |  Queries for things with an attribute named "myAttribute" whose value falls within the numeric range \(>75 and <=80\)\.  | 
|  attributes\.serialNumber:\["abcd" TO "abcf"\]  |  Queries for things with an attribute named "serialNumber" whose value is within an alphanumeric string range\. This query returns things with a "serialNumber" attribute with values "abcd", "abce", or "abcf"\.  | 
|  attributes\.myAttribute:i\*t  |  Queries for things with an attribute named "myAttribute" whose value is 'i', followed by any number of characters, followed by 't'\.  | 
|  attributes\.attr1:abc AND attributes\.attr2<5 NOT attributes\.attr3>10  |  Queries for things that combine terms using Boolean expressions\. This query returns things that have an attribute named "attr1" with a value "abc", an attribute named "attr2" that is less than 5, and an attribute named "attr3" that is not greater than 10\.  | 
|  shadow\.hasDelta:true  |  Queries for things whose shadow has a delta element\.  | 
|  NOT attributes\.model:legacy  |  Queries for things where the attribute named "model" is not "legacy"\.  | 
|  shadow\.reported\.stats\.battery:\{70 TO 100\} \(v2 OR v3\) NOT attributes\.model:legacy  |  Queries for things with the following: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/iot/latest/developerguide/example-queries.html)  | 
|  shadow\.reported\.myvalues:2  |  Queries for things where the `myvalues` array in the shadow's reported section contains a value of 2\.  | 
|  shadow\.reported\.location:\* NOT shadow\.desired\.stats\.battery:\*  |  Queries for things with the following: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/iot/latest/developerguide/example-queries.html)  | 
|  connectivity\.connected:true  |  Queries for all connected devices\.  | 
|  connectivity\.connected:false  | Queries for all disconnected devices\. | 
|  connectivity\.connected:true AND connectivity\.timestamp : \[1557651600000 TO 1557867600000\]  | Queries for all connected devices with a connect timestamp >= 1557651600000 and <= 1557867600000\. Timestamps are given in milliseconds since epoch\. | 
|  connectivity\.connected:false AND connectivity\.timestamp : \[1557651600000 TO 1557867600000\]  | Queries for all disconnected devices with a disconnect timestamp >= 1557651600000 and <= 1557867600000\. Timestamps are given in milliseconds since epoch\. | 
|  connectivity\.connected:true AND connectivity\.timestamp > 1557651600000  | Queries for all connected devices with a connect timestamp > 1557651600000\. Timestamps are given in milliseconds since epoch\. | 
|  connectivity\.connected:\*  | Queries for all devices with connectivity information present\. | 
|  connectivity\.disconnectReason:\*  | Queries for all devices with connectivity disconnectReason present\. | 
|  connectivity\.disconnectReason:CLIENT\_INITIATED\_DISCONNECT  | Queries for all devices disconnected due to CLIENT\_INITIATED\_DISCONNECT\. | 