# Literals<a name="iot-sql-literals"></a>

You can directly specify literal objects in the SELECT and WHERE clauses of your rule SQL, which can be useful for passing information\. 

**Note**  
Literals are only available when using SQL Version 2016\-03\-23 or newer\.

JSON object syntax is used \(key\-value pairs, comma\-separated, where keys are strings and values are JSON values, wrapped in curly brackets \{\}\)\. For example:

Incoming payload published on topic `a/b`: `{"lat_long": [47.606,-122.332]}`

SQL statement: `SELECT {'latitude': get(lat_long, 0),'longitude':get(lat_long, 1)} as lat_long FROM 'a/b'`

The resulting outgoing payload would be: `{"lat_long":{"latitude":47.606,"longitude":-122.332}}`\. 

You can also directly specify arrays in the SELECT and WHERE clauses of your rule SQL, which allows you to group information\. JSON syntax is used \(wrap comma\-separated items in square brackets \[\] to create an array literal\)\. For example:

Incoming payload published on topic `a/b`: `{"lat": 47.696, "long": -122.332}`

SQL statement: `SELECT [lat,long] as lat_long FROM 'a/b'`

The resulting output payload would be: `{"lat_long": [47.606,-122.332]}`\.