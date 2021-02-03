# How to manage things with the registry<a name="thing-registry"></a>

You use the AWS IoT console or the AWS CLI to interact with the registry\. The following sections show how to use the CLI to work with the registry\.

**When naming your thing objects:**
+ You should not use personally identifiable information in your thing name\. The thing name can appear in unencrypted communications and reports\. 
+  You should not use a colon character \( : \) in a thing name\. The colon character is used as a delimiter by other AWS IoT services and this can cause them to parse strings with thing names incorrectly\. 

## Create a thing<a name="create-thing"></a>

The following command shows how to use the AWS IoT CreateThing command from the CLI to create a thing\. You can't change a thing's name after you create it\. To change a thing's name, you must create a new thing, give it the new name, and then delete the old thing\.

```
$ aws iot create-thing --thing-name "MyLightBulb" --attribute-payload "{\"attributes\": {\"wattage\":\"75\", \"model\":\"123\"}}"
```

The CreateThing command displays the name and Amazon Resource Name \(ARN\) of your new thing:

```
{
    "thingArn": "arn:aws:iot:us-east-1:123456789012:thing/MyLightBulb",
    "thingName": "MyLightBulb",
    "thingId": "12345678abcdefgh12345678ijklmnop12345678"
}
```

**Note**  
We do not recommend using personally identifiable information in your thing names\.

## List things<a name="list-things"></a>

You can use the ListThings command to list all things in your account:

```
$ aws iot list-things
```

```
{
    "things": [
       {
            "attributes": {
                "model": "123",
                "wattage": "75"
            },
            "version": 1,
            "thingName": "MyLightBulb"
        },
        {
            "attributes": {
                "numOfStates":"3"
             },
            "version": 11,
            "thingName": "MyWallSwitch"
        }
    ]
}
```

You can use the ListThings command to search for all things of a specific thing type:

```
$  aws iot list-things --thing-type-name "LightBulb"
```

```
{
    "things": [
        {
            "thingTypeName": "LightBulb",
            "attributes": {
                "model": "123",
                "wattage": "75"
            },
            "version": 1,
            "thingName": "MyRGBLight"
        },
        {
            "thingTypeName": "LightBulb",
            "attributes": {
                "model": "123",
                "wattage": "75"
            },
            "version": 1,
            "thingName": "MySecondLightBulb"
        }
    ]
}
```

You can use the ListThings command to search for all things that have an attribute with a specific value\. This command searches only searchable attributes\.

```
$  aws iot list-things --attribute-name "wattage" --attribute-value "75"
```

```
{
    "things": [
        {
            "thingTypeName": "StopLight",
            "attributes": {
                "model": "123",
                "wattage": "75"
            },
            "version": 3,
            "thingName": "MyLightBulb"
        },
        {
            "thingTypeName": "LightBulb",
            "attributes": {
                "model": "123",
                "wattage": "75"
            },
            "version": 1,
            "thingName": "MyRGBLight"
        },
        {
            "thingTypeName": "LightBulb",
            "attributes": {
                "model": "123",
                "wattage": "75"
            },
            "version": 1,
            "thingName": "MySecondLightBulb"
        }
    ]
}
```

If [fleet indexing](iot-indexing.md) is enabled, you can use the [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/iot/search-index.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/iot/search-index.html) command to search on searchable and non\-searchable thing attributes, device shadow values, and connectivity values\. For more information about what you can query by using the search\-index command, see [Example thing queries](example-queries.md) and the CLI reference on the [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/iot/search-index.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/iot/search-index.html) command\.

## Describe things<a name="search-things"></a>

You can use the DescribeThing command to display more detailed information about a thing:

```
$ aws iot describe-thing --thing-name "MyLightBulb"
{
    "version": 3,
    "thingName": "MyLightBulb",
    "thingArn": "arn:aws:iot:us-east-1:123456789012:thing/MyLightBulb",
    "thingId": "12345678abcdefgh12345678ijklmnop12345678",
    "defaultClientId": "MyLightBulb",
    "thingTypeName": "StopLight",
    "attributes": {
        "model": "123",
        "wattage": "75"
    }
}
```

## Update a thing<a name="update-thing"></a>

You can use the UpdateThing command to update a thing\. Note that this command updates only the thing's attributes\. You can't change a thing's name\. To change a thing's name, you must create a new thing, give it the new name, and then delete the old thing\.

```
$ aws iot update-thing --thing-name "MyLightBulb" --attribute-payload "{\"attributes\": {\"wattage\":\"150\", \"model\":\"456\"}}"
```

The UpdateThing command does not produce output\. You can use the DescribeThing command to see the result:

```
$ aws iot describe-thing --thing-name "MyLightBulb"
{
    "attributes": {
        "model": "456",
        "wattage": "150"
    },
    "version": 2,
    "thingName": "MyLightBulb"
}
```

## Delete a thing<a name="delete-thing"></a>

You can use the DeleteThing command to delete a thing:

```
$ aws iot delete-thing --thing-name "MyThing"
```

This command returns successfully with no error if the deletion is successful or you specify a thing that doesn't exist\.

## Attach a principal to a thing<a name="attach-thing-principal"></a>

A physical device must have an X\.509 certificate to communicate with AWS IoT\. You can associate the certificate on your device with the thing in the registry that represents your device\. To attach a certificate to your thing, use the AttachThingPrincipal command:

```
$ aws iot attach-thing-principal --thing-name "MyLightBulb" --principal "arn:aws:iot:us-east-1:123456789012:cert/a0c01f5835079de0a7514643d68ef8414ab739a1e94ee4162977b02b12842847"
```

The AttachThingPrincipal command does not produce any output\.

## Detach a principal from a thing<a name="detach-thing-principal"></a>

You can use the DetachThingPrincipal command to detach a certificate from a thing:

```
$ aws iot detach-thing-principal --thing-name "MyLightBulb" --principal "arn:aws:iot:us-east-1:123456789012:cert/a0c01f5835079de0a7514643d68ef8414ab739a1e94ee4162977b02b12842847"
```

The DetachThingPrincipal command does not produce any output\.