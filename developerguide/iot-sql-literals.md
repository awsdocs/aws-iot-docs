# Literals<a name="iot-sql-literals"></a>

You can directly specify literal objects in the SELECT and WHERE clauses of your rule SQL, which can be useful for passing information\. 

**Note**  
Literals are available only when using SQL version 2016\-03\-23 or later\.

JSON object syntax is used \(key\-value pairs, comma\-separated, where keys are strings and values are JSON values, wrapped in curly brackets \{\}\)\. For example:

Incoming payload published on topic `topic/subtopic`: `{"lat_long": [47.606,-122.332]}`

SQL statement: `SELECT {'latitude': get(lat_long, 0),'longitude':get(lat_long, 1)} as lat_long FROM 'topic/subtopic'`

The resulting outgoing payload would be: `{"lat_long":{"latitude":47.606,"longitude":-122.332}}`\. 

You can also directly specify arrays in the SELECT and WHERE clauses of your rule SQL, which allows you to group information\. JSON syntax is used \(wrap comma\-separated items in square brackets \[\] to create an array literal\)\. For example:

Incoming payload published on topic `topic/subtopic`: `{"lat": 47.696, "long": -122.332}`

SQL statement: `SELECT [lat,long] as lat_long FROM 'topic/subtopic'`

The resulting output payload would be: `{"lat_long": [47.606,-122.332]}`\.