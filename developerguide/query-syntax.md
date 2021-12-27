# Query syntax<a name="query-syntax"></a>

In fleet indexing, you use a query syntax to specify queries\. 

## Supported features<a name="supported-query-syntax"></a>

The query syntax supports the following features:
+ Terms and phrases
+ Searching fields
+ Prefix search
+ Range search
+ Boolean operators `AND`, `OR`, `NOT` and `–`\. The hyphen is used to exclude something from search results \(for example, `thingName:(tv* AND -plasma)`\)\.
+ Grouping
+ Field grouping
+ Escaping special characters \(such as with *\\*\)

## Unsupported features<a name="unsupported-query-syntax"></a>

The query syntax doesn't support the following features:
+ Leading wildcard search \(such as "\*xyz"\), but searching for "\*" matches all things
+ Regular expressions
+ Boosting
+ Ranking
+ Fuzzy searches
+ Proximity search
+ Sorting
+ Aggregation

## Notes<a name="query-syntax-limitations"></a>

A few things to note about the query language:
+ The default operator is AND\. A query for `"thingName:abc thingType:xyz"` is equivalent to `"thingName:abc AND thingType:xyz"`\.
+ If a field isn't specified, AWS IoT searches for the term in all the registry, Device Shadow, and Device Defender fields\.
+ All field names are case sensitive\.
+ Search is case insensitive\. Words are separated by white\-space characters as defined by Java's `Character.isWhitespace(int)`\.
+ Indexing of Device Shadow data \(unnamed shadows and named shadows\) includes reported, desired, delta, and metadata sections\.
+ Device shadow and registry versions aren't searchable, but are present in the response\.
+ The maximum number of terms in a query is seven\.