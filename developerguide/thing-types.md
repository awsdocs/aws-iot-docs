# Thing types<a name="thing-types"></a>

Thing types allow you to store description and configuration information that is common to all things associated with the same thing type\. This simplifies the management of things in the registry\. For example, you can define a LightBulb thing type\. All things associated with the LightBulb thing type share a set of attributes: serial number, manufacturer, and wattage\. When you create a thing of type LightBulb \(or change the type of an existing thing to LightBulb\) you can specify values for each of the attributes defined in the LightBulb thing type\. 

Although thing types are optional, their use makes it easier to discover things\.
+ Things with a thing type can have up to 50 attributes\.
+ Things without a thing type can have up to three attributes\.
+ A thing can be associated with only one thing type\.
+ There is no limit on the number of thing types you can create in your account\.

Thing types are immutable\. You can't change a thing type name after it has been created\. You can deprecate a thing type at any time to prevent new things from being associated with it\. You can also delete thing types that have no things associated with them\. 

## Create a thing type<a name="create-thing-type"></a>

You can use the CreateThingType command to create a thing type:

```
$ aws iot create-thing-type 

                --thing-type-name "LightBulb" --thing-type-properties "thingTypeDescription=light bulb type, searchableAttributes=wattage,model"
```

The CreateThingType command returns a response that contains the thing type and its ARN:

```
{
    "thingTypeName": "LightBulb",
    "thingTypeId": "df9c2d8c-894d-46a9-8192-9068d01b2886",
    "thingTypeArn": "arn:aws:iot:us-west-2:123456789012:thingtype/LightBulb"
}
```

## List thing types<a name="list-thing-types"></a>

You can use the ListThingTypes command to list thing types:

```
$ aws iot list-thing-types
```

The ListThingTypes command returns a list of the thing types defined in your AWS account:

```
{
    "thingTypes": [
        {
            "thingTypeName": "LightBulb",
            "thingTypeProperties": {
                "searchableAttributes": [
                    "wattage",
                    "model"
                ],
                "thingTypeDescription": "light bulb type"
            },
            "thingTypeMetadata": {
                "deprecated": false,
                "creationDate": 1468423800950
            }
        }
    ]
}
```

## Describe a thing type<a name="describe-thing-type"></a>

You can use the DescribeThingType command to get information about a thing type:

```
$ aws iot describe-thing-type --thing-type-name "LightBulb"
```

The DescribeThingType command returns information about the specified type:

```
{
    "thingTypeProperties": {
        "searchableAttributes": [
            "model", 
            "wattage"
        ], 
        "thingTypeDescription": "light bulb type"
    }, 
    "thingTypeId": "df9c2d8c-894d-46a9-8192-9068d01b2886", 
    "thingTypeArn": "arn:aws:iot:us-west-2:123456789012:thingtype/LightBulb", 
    "thingTypeName": "LightBulb", 
    "thingTypeMetadata": {
        "deprecated": false, 
        "creationDate": 1544466338.399
    }
}
```

## Associate a thing type with a thing<a name="associate-thing-type"></a>

You can use the CreateThing command to specify a thing type when you create a thing:

```
$ aws iot create-thing --thing-name "MyLightBulb" --thing-type-name "LightBulb" --attribute-payload "{\"attributes\": {\"wattage\":\"75\", \"model\":\"123\"}}"
```

You can use the UpdateThing command at any time to change the thing type associated with a thing:

```
$ aws iot update-thing --thing-name "MyLightBulb"
                --thing-type-name "LightBulb" --attribute-payload  "{\"attributes\": {\"wattage\":\"75\", \"model\":\"123\"}}"
```

You can also use the UpdateThing command to disassociate a thing from a thing type\.

## Deprecate a thing type<a name="deprecate-thing-type"></a>

Thing types are immutable\. They can't be changed after they are defined\. You can, however, deprecate a thing type to prevent users from associating any new things with it\. All existing things associated with the thing type are unchanged\.

To deprecate a thing type, use the DeprecateThingType command:

```
$ aws iot deprecate-thing-type --thing-type-name "myThingType"
```

You can use the DescribeThingType command to see the result:

```
$ aws iot describe-thing-type --thing-type-name "StopLight":
```

```
{
    "thingTypeName": "StopLight",
    "thingTypeProperties": {
        "searchableAttributes": [
            "wattage",
            "numOfLights",
            "model"
        ],
        "thingTypeDescription": "traffic light type",
    },
    "thingTypeMetadata": {
        "deprecated": true,
        "creationDate": 1468425854308,
        "deprecationDate": 1468446026349
    }
}
```

Deprecating a thing type is a reversible operation\. You can undo a deprecation by using the `--undo-deprecate` flag with the DeprecateThingType CLI command:

```
$ aws iot deprecate-thing-type --thing-type-name "myThingType" --undo-deprecate
```

You can use the DescribeThingType CLI command to see the result:

```
$ aws iot describe-thing-type --thing-type-name "StopLight":
```

```
{
    "thingTypeName": "StopLight",
    "thingTypeArn": "arn:aws:iot:us-east-1:123456789012:thingtype/StopLight",
    "thingTypeId": "12345678abcdefgh12345678ijklmnop12345678"
    "thingTypeProperties": {
        "searchableAttributes": [
            "wattage",
            "numOfLights",
            "model"
        ],
        "thingTypeDescription": "traffic light type"
    },
    "thingTypeMetadata": {
        "deprecated": false,
        "creationDate": 1468425854308,
    }
}
```

## Delete a thing type<a name="delete-thing-types"></a>

You can delete thing types only after they have been deprecated\. To delete a thing type, use the DeleteThingType command:

```
$ aws iot delete-thing-type --thing-type-name "StopLight"
```

**Note**  
You must wait five minutes after you deprecate a thing type before you can delete it\.