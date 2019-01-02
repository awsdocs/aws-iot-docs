# Device Shadow Service Documents<a name="device-shadow-document"></a>

The Device Shadow service respects all rules of the JSON specification\. Values, objects, and arrays are stored in the device's shadow document\.


+ [Document Properties](#document-structure)
+ [Versioning of a Device Shadow](#versioning)
+ [Client Token](#client-token)
+ [Example Document](#device-shadow-example)
+ [Empty Sections](#device-shadow-empty-fields)
+ [Arrays](#device-shadow-arrays)

## Document Properties<a name="document-structure"></a>

A device's shadow document has the following properties:

`state`    
`desired`  
The desired state of the thing\. Applications can write to this portion of the document to update the state of a thing without having to directly connect to a thing\.  
`reported`  
The reported state of the thing\. Things write to this portion of the document to report their new state\. Applications read this portion of the document to determine the state of a thing\.

`metadata`  
Information about the data stored in the `state` section of the document\. This includes timestamps, in Epoch time, for each attribute in the `state` section, which enables you to determine when they were updated\.  
Metadata do not contribute to the document size for service limits or pricing\. For more information, see [AWS IoT Service Limits](http://alpha-docs-aws.amazon.com/general/latest/gr/aws_service_limits.html#limits_iot)\.

`timestamp`  
Indicates when the message was transmitted by AWS IoT\. By using the timestamp in the message and the timestamps for individual attributes in the `desired` or `reported` section, a thing can determine how old an updated item is, even if it doesn't feature an internal clock\.

`clientToken`  
A string unique to the device that enables you to associate responses with requests in an MQTT environment\.

`version`  
The document version\. Every time the document is updated, this version number is incremented\. Used to ensure the version of the document being updated is the most recent\.

For more information, see [Shadow Document Syntax](device-shadow-document-syntax.md)\.

## Versioning of a Device Shadow<a name="versioning"></a>

The Device Shadow service supports versioning on every update message \(both request and response\), which means that with every update of a device's shadow, the version of the JSON document is incremented\. This ensures two things:

+ A client can receive an error if it attempts to overwrite a shadow using an older version number\. The client is informed it must resync before it can update a device's shadow\.

+ A client can decide not to act on a received message if the message has a lower version than the version stored by the client\. 

In some cases, a client might bypass version matching by not submitting a version\.

## Client Token<a name="client-token"></a>

You can use a client token with MQTT\-based messaging to verify the same client token is contained in a request and request response\. This ensures the response and request are associated\.

**Note**  
The client token can be no longer than 64 bytes\. A client token that is longer than 64 bytes will cause a 400 \(Bad Request\) response and an *Invalid clientToken* error message\.

## Example Document<a name="device-shadow-example"></a>

Here is an example shadow document:

```
{
    "state" : {
        "desired" : {
          "color" : "RED",
          "sequence" : [ "RED", "GREEN", "BLUE" ]
        },
        "reported" : {
          "color" : "GREEN"
        }
    },
    "metadata" : {
        "desired" : {
            "color" : {
                "timestamp" : 12345
            },
            "sequence" : {
                "timestamp" : 12345
            }
        },
        "reported" : {
            "color" : {
                "timestamp" : 12345
            }
        }
    },
    "version" : 10,
    "clientToken" : "UniqueClientToken",
    "timestamp": 123456789
}
```

## Empty Sections<a name="device-shadow-empty-fields"></a>

A shadow document contains a `desired` section only if it has a desired state\. For example, the following is a valid state document with no `desired` section:

```
{
    "reported" : { "temp": 55 }
}
```

The `reported` section can also be empty:

```
{
    "desired" : { "color" : "RED" }
}
```

If an update causes the `desired` or `reported` sections to become null, the section is removed from the document\. To remove the `desired` section from a document \(in response, for example, to a device updating its state\), set the desired section to `null`: 

```
{ 
    "state": {
        "reported": {
            "color": "red" 
        }, 
        "desired": null 
    } 
}
```

It is also possible a shadow document will not contain `desired` or `reported` sections\. In that case, the shadow document is empty\. For example, this is a valid document:

```
{
}
```

## Arrays<a name="device-shadow-arrays"></a>

Shadows support arrays, but treat them as normal values in that an update to an array replaces the whole array\. It is not possible to update part of an array\.

Initial state:

```
{
    "desired" : { "colors" : ["RED", "GREEN", "BLUE" ] }
}
```

Update:

```
{
    "desired" : { "colors" : ["RED"] }
}
```

Final state:

```
{
    "desired" : { "colors" : ["RED"] }
}
```

Arrays can't have null values\. For example, the following array is not valid and will be rejected\.

```
{
    "desired" : { 
        "colors" : [ null, "RED", "GREEN" ]
    }
}
```