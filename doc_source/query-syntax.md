# Query syntax<a name="query-syntax"></a>

Queries are speciﬁed using a query syntax\.

The query syntax supports the following features\.
+ Terms and phrases
+ Searching fields
+ Prefix search
+ Range search
+ Boolean operators `AND`, `OR`, `NOT` and `–`\. The hyphen is used to exclude something from search results \(for example, `thingName:(tv* AND -plasma)`\)\)\.
+ Grouping
+ Field grouping
+ Escaping special characters \(as with *\\*\)

The query syntax does not support the following features:
+ Leading wildcard search \(such as "\*xyz"\), but searching for "\*" matches all things
+ Regular expressions
+ Boosting
+ Ranking
+ Fuzzy searches
+ Proximity search
+ Sorting
+ Aggregation

A few things to note about the query language:
+ The default operator is AND\. A query for `"thingName:abc thingType:xyz"` is equivalent to `"thingName:abc AND thingType:xyz"`\.
+ If a field isn't specified, AWS IoT searches for the term in all fields\.
+ All field names are case sensitive\.
+ Search is case insensitive\. Words are separated by white space characters as defined by Java's `Character.isWhitespace(int)`\.
+ Indexing of device shadow data includes reported, desired, delta, and metadata sections\.
+ Device shadow and registry versions are not searchable, but are present in the response\.
+ The maximum number of terms in a query is 5\.