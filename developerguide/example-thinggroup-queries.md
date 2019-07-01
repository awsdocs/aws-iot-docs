# Example Thing Group Queries<a name="example-thinggroup-queries"></a>

Queries are specified in a query string using a query syntax and passed to the [https://docs.aws.amazon.com/iot/latest/apireference/API_SearchIndex.html](https://docs.aws.amazon.com/iot/latest/apireference/API_SearchIndex.html) API\. The following table lists some example query strings\.


****  

| Query String | Result | 
| --- | --- | 
|  abc  |  Queries for "abc" in any field\.  | 
|  thingGroupName:myGroupThingName  |  Queries for a thing group with name "myGroupThingName"\.  | 
|  thingGroupName:my\*  |  Queries for thing groups with names that begin with "my"\.  | 
|  thingGroupName:ab?  |  Queries for thing groups with names that have "ab" plus one additional character, for example: "aba", "abb", "abc", etc\.  | 
|  attributes\.myAttribute:75  |  Queries for thing groups with an attribute named "myAttribute" that has the value 75\.  | 
|  attributes\.myAttribute:\[75 TO 80\]  |  Queries for thing groups with an attribute named "myAttribute" whose value falls within a numeric range \(75–80, inclusive\)\.  | 
|  attributes\.myAttribute:\[75 TO 80\]  |  Queries for thing groups with an attribute named "myAttribute" whose value falls within the numeric range \(>75 and <=80\)\.  | 
|  attributes\.myAttribute:\["abcd" TO "abcf"\]  |  Queries for thing groups with an attribute named "myAttribute" whose value is within an alphanumeric string range\. This query returns thing groups with a "serialNumber" attribute with values "abcd", "abce", or "abcf"\.  | 
|  attributes\.myAttribute:i\*t  |  Queries for thing groups with an attribute named "myAttribute" whose value is 'i', followed by any number of characters, followed by 't'\.  | 
|  attributes\.attr1:abc AND attributes\.attr2<5 NOT attributes\.attr3>10  |  Queries for thing groups that combine terms using Boolean expressions\. This query returns thing groups that have an attribute named "attr1" with a value "abc", an attribute named "attr2" that is less than 5, and an attribute named "attr3" that is not greater than 10\.  | 
|  NOT attributes\.myAttribute:cde  |  Queries for thing groups where the attribute named "myAttribute" is not "cde"\.  | 
|  parentGroupNames:\(myParentThingGroupName\)  |   Queries for thing groups whose parent group name matches "myParentThingGroupName"\.  | 
|  parentGroupNames:\(myParentThingGroupName OR myRootThingGroupName\)  |  Queries for thing groups whose parent group name matches "myParentThingGroupName" or "myRootThingGroupName"\.  | 
|  parentGroupNames:\(myParentThingGroupNa\*\)  |  Queries for thing groups whose parent group name begins with "myParentThingGroupNa"\.  | 