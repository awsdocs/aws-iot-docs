# Example thing queries<a name="example-queries"></a>

Specify queries in a query string using a query syntax\. The queries are passed to the [https://docs.aws.amazon.com/iot/latest/apireference/API_SearchIndex.html](https://docs.aws.amazon.com/iot/latest/apireference/API_SearchIndex.html) API\. The following table lists some example query strings\.


****  

| Query string | Result | 
| --- | --- | 
|  abc  |  Queries for "abc" in any registry, shadow \(classic unnamed shadow and named shadow\), or Device Defender violations field\.  | 
|  thingName:myThingName  |  Queries for a thing with name "myThingName"\.  | 
|  thingName:my\*  |  Queries for things with names that begin with "my"\.  | 
|  thingName:ab?  |  Queries for things with names that have "ab" plus one additional character \(for example, "aba", "abb", "abc", and so on\.\)  | 
|  thingTypeName:aa  |  Queries for things that are associated with type "aa"\.  | 
|  attributes\.myAttribute:75  |  Queries for things with an attribute named "myAttribute" that has the value 75\.  | 
|  attributes\.myAttribute:\[75 TO 80\]  |  Queries for things with an attribute named "myAttribute" that has a value that falls within a numeric range \(75–80, inclusive\)\.  | 
|  attributes\.myAttribute:\{75 TO 80\]  |  Queries for things with an attribute named "myAttribute" that has a value that falls within the numeric range \(>75 and <=80\)\.  | 
|  attributes\.serialNumber:\["abcd" TO "abcf"\]  |  Queries for things with an attribute named "serialNumber" that has a value within an alphanumeric string range\. This query returns things with a "serialNumber" attribute with values "abcd", "abce", or "abcf"\.  | 
|  attributes\.myAttribute:i\*t  |  Queries for things with an attribute named "myAttribute" where the value is 'i', followed by any number of characters, followed by 't'\.  | 
|  attributes\.attr1:abc AND attributes\.attr2<5 NOT attributes\.attr3>10  |  Queries for things that combine terms using Boolean expressions\. This query returns things that have an attribute named "attr1" with a value "abc", an attribute named "attr2" that is less than 5, and an attribute named "attr3" that is not greater than 10\.  | 
|  shadow\.hasDelta:true  |  Queries for things with an unnamed shadow that has a delta element\.  | 
|  NOT attributes\.model:legacy  |  Queries for things where the attribute named "model" is not "legacy"\.  | 
|  shadow\.reported\.stats\.battery:\{70 TO 100\} \(v2 OR v3\) NOT attributes\.model:legacy  |  Queries for things with the following: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/iot/latest/developerguide/example-queries.html)  | 
|  shadow\.reported\.myvalues:2  |  Queries for things where the `myvalues` array in the shadow's reported section contains a value of 2\.  | 
|  shadow\.reported\.location:\* NOT shadow\.desired\.stats\.battery:\*  |  Queries for things with the following: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/iot/latest/developerguide/example-queries.html)  | 
|  shadow\.name\.<shadowName>\.hasDelta:true  |  Queries for things that have a shadow with the given name and also a delta element\.   | 
|  shadow\.name\.<shadowName>\.desired\.filament:\*  |  Queries for things that have a shadow with the given name and also a desired filament property\.   | 
| shadow\.name\.<shadowName>\.reported\.location:\* |  Queries for things that have a shadow with the given name and where the `location` attribute exists in the named shadow's reported section\.  | 
|  connectivity\.connected:true  |  Queries for all connected devices\.  | 
|  connectivity\.connected:false  | Queries for all disconnected devices\. | 
|  connectivity\.connected:true AND connectivity\.timestamp : \[1557651600000 TO 1557867600000\]  | Queries for all connected devices with a connect timestamp >= 1557651600000 and <= 1557867600000\. Timestamps are given in milliseconds since epoch\. | 
|  connectivity\.connected:false AND connectivity\.timestamp : \[1557651600000 TO 1557867600000\]  | Queries for all disconnected devices with a disconnect timestamp >= 1557651600000 and <= 1557867600000\. Timestamps are given in milliseconds since epoch\. | 
|  connectivity\.connected:true AND connectivity\.timestamp > 1557651600000  | Queries for all connected devices with a connect timestamp > 1557651600000\. Timestamps are given in milliseconds since epoch\. | 
|  connectivity\.connected:\*  | Queries for all devices with connectivity information present\. | 
|  connectivity\.disconnectReason:\*  | Queries for all devices with connectivity disconnectReason present\. | 
|  connectivity\.disconnectReason:CLIENT\_INITIATED\_DISCONNECT  | Queries for all devices disconnected due to CLIENT\_INITIATED\_DISCONNECT\. | 
|  deviceDefender\.violationCount:\[0 TO 100\]  | Queries for things with a Device Defender violations count value that falls within the numeric range \(0\-100, inclusive\)\.  | 
|  deviceDefender\.<device\-SecurityProfile>\.disconnectBehavior\.inViolation:true  | Queries for things that are in violation for the behavior disconnectBehavior as defined in the security profile device\-SecurityProfile\. Note that inViolation:false is not a valid query\.  | 
|  deviceDefender\.<device\-SecurityProfile>\.disconnectBehavior\.lastViolationValue\.number>2  | Queries for things that are in violation for the behavior disconnectBehavior as defined in the security profile device\-SecurityProfile with a last violation event value greater than 2\.  | 
|  deviceDefender\.<device\-SecurityProfile>\.disconnectBehavior\.lastViolationTime>1634227200000  |  Queries for things that are in violation for the behavior `disconnectBehavior` as defined in the security profile device\-SecurityProfile with a last violation event after a specified epoch time\.   | 