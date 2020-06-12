# Device Shadow service documents<a name="device-shadow-document"></a>

The Device Shadow service respects all rules of the JSON specification\. Values, objects, and arrays are stored in the device's shadow document\.

**Topics**
+ [Shadow document examples](#device-shadow-document-syntax)
+ [Document properties](#document-structure)
+ [Delta state](#delta-state)
+ [Versioning shadow documents](#versioning)
+ [Client tokens in shadow documents](#client-token)
+ [Empty shadow document properties](#device-shadow-empty-fields)
+ [Array values in shadow documents](#device-shadow-arrays)

## Shadow document examples<a name="device-shadow-document-syntax"></a><a name="device-shadow-example"></a>

The Device Shadow service uses these documents in UPDATE, GET, and DELETE operations using the [REST API](device-shadow-rest-api.md) or [MQTT Pub/Sub Messages](device-shadow-mqtt.md)\.

**Topics**
+ [Request state document](#device-shadow-example-request-json)
+ [Response state documents](#device-shadow-example-response-json)
+ [Error response document](#device-shadow-example-error-json)
+ [Shadow name list response document](#device-shadow-list-json)

### Request state document<a name="device-shadow-example-request-json"></a>

A request state document has the following format:

```
{
    "state": {
        "desired": {
            "attribute1": integer2,
            "attribute2": "string2",
            ...
            "attributeN": boolean2
        },
        "reported": {
            "attribute1": integer1,
            "attribute2": "string1",
            ...
            "attributeN": boolean1
        }
    },
    "clientToken": "token",
    "version": version
}
```
+ `state` — Updates affect only the fields specified\. Typically, you'll use either the `desired` or the `reported` property, but not both in the same request\.
  + `desired` — The state properties and values requested to be updated in the device\.
  + `reported` — The state properties and values reported by the device\.
+ `clientToken` — If used, you can match the request and corresponding response by the client token\.
+ `version` — If used, the Device Shadow service processes the update only if the specified version matches the latest version it has\.

### Response state documents<a name="device-shadow-example-response-json"></a>

Response state documents have the following format depending on the response type\.

#### /accepted response state document<a name="device-shadow-example-response-json-accepted"></a>

```
{
    "state": {
        "desired": {
            "attribute1": integer2,
            "attribute2": "string2",
            ...
            "attributeN": boolean2
        }
    },
    "metadata": {
        "desired": {
            "attribute1": {
                "timestamp": timestamp
            },
            "attribute2": {
                "timestamp": timestamp
            },
            ...
            "attributeN": {
                "timestamp": timestamp
            }
        }
    },
    "timestamp": timestamp,
    "clientToken": "token",
    "version": version
}
```

#### /delta response state document<a name="device-shadow-example-response-json-delta"></a>

```
{
    "state": {
        "attribute1": integer2,
        "attribute2": "string2",
        ...
        "attributeN": boolean2
        }
    },
    "metadata": {
        "attribute1": {
            "timestamp": timestamp
        },
        "attribute2": {
            "timestamp": timestamp
        },
        ...
        "attributeN": {
            "timestamp": timestamp
        }
    },
    "timestamp": timestamp,
    "clientToken": "token",
    "version": version
}
```

#### /documents response state document<a name="device-shadow-example-response-json-documents"></a>

```
{
  "previous" : {
    "state": {
        "desired": {
            "attribute1": integer2,
            "attribute2": "string2",
            ...
            "attributeN": boolean2
        },
        "reported": {
            "attribute1": integer1,
            "attribute2": "string1",
            ...
            "attributeN": boolean1
        }
    },
    "metadata": {
        "desired": {
            "attribute1": {
                "timestamp": timestamp
            },
            "attribute2": {
                "timestamp": timestamp
            },
            ...
            "attributeN": {
                "timestamp": timestamp
            }
        },
        "reported": {
            "attribute1": {
                "timestamp": timestamp
            },
            "attribute2": {
                "timestamp": timestamp
            },
            ...
            "attributeN": {
                "timestamp": timestamp
            }
        }
    },
    "version": version-1
  },
  "current": {
    "state": {
        "desired": {
            "attribute1": integer2,
            "attribute2": "string2",
            ...
            "attributeN": boolean2
        },
        "reported": {
            "attribute1": integer2,
            "attribute2": "string2",
            ...
            "attributeN": boolean2
        }
    },
    "metadata": {
        "desired": {
            "attribute1": {
                "timestamp": timestamp
            },
            "attribute2": {
                "timestamp": timestamp
            },
            ...
            "attributeN": {
                "timestamp": timestamp
            }
        },
        "reported": {
            "attribute1": {
                "timestamp": timestamp
            },
            "attribute2": {
                "timestamp": timestamp
            },
            ...
            "attributeN": {
                "timestamp": timestamp
            }
        }
    },
    "version": version
  },
  "timestamp": timestamp,
  "clientToken": "token"
}
```

#### Response state document properties<a name="device-shadow-example-response-json-properties"></a>
+ `previous` — After a successful update, contains the `state` of the object before the update\.
+ `current` — After a successful update, contains the `state` of the object after the update\.
+ `state`
  + `reported` — Present only if a thing reported any data in the `reported` section and contains only fields that were in the request state document\.
  + `desired` — Present only if a device reported any data in the `desired` section and contains only fields that were in the request state document\.
  + `delta` — Present only if the `desired` data differs from the shadow's current `reported` data\.
+ `metadata` — Contains the timestamps for each attribute in the `desired` and `reported` sections so that you can determine when the state was updated\.
+ `timestamp` — The Epoch date and time the response was generated by AWS IoT\.
+ `clientToken` — Present only if a client token was used when publishing valid JSON to the `/update` topic\.
+ `version` — The current version of the document for the device's shadow shared in AWS IoT\. It is increased by one over the previous version of the document\.

### Error response document<a name="device-shadow-example-error-json"></a>

An error response document has the following format:

```
{
    "code": error-code,
    "message": "error-message",
    "timestamp": timestamp,
    "clientToken": "token"
}
```
+ `code` — An HTTP response code that indicates the type of error\.
+ `message` — A text message that provides additional information\.
+ `timestamp` — The date and time the response was generated by AWS IoT\. This property is not present in all error response documents\.
+ `clientToken` — Present only if a client token was used in the published message\.

For more information, see [Device Shadow error messages](device-shadow-error-messages.md)\.

### Shadow name list response document<a name="device-shadow-list-json"></a>

A shadow name list response document has the following format:

```
{
    "results": [
        "shadowName-1",
        "shadowName-2",
        "shadowName-3",
        "shadowName-n"
    ],
    "nextToken": "nextToken",
    "timestamp": timestamp
}
```
+ `results` — The array of shadow names\.
+ `nextToken` — The token value to use in paged requests to get the next page in the sequence\. This property is not present when there are no more shadow names to return\. 
+ `timestamp` — The date and time the response was generated by AWS IoT\.

## Document properties<a name="document-structure"></a>

A device's shadow document has the following properties:

`state`  <a name="state"></a>  
`desired`  <a name="desired"></a>
The desired state of the device\. Apps can write to this portion of the document to update the state of a device without having to directly connect to a it\.  
`reported`  <a name="reported"></a>
The reported state of the device\. Devices write to this portion of the document to report their new state\. Apps read this portion of the document to determine the device's last\-reported state\.

`metadata`  <a name="metadata"></a>
Information about the data stored in the `state` section of the document\. This includes timestamps, in Epoch time, for each attribute in the `state` section, which enables you to determine when they were updated\.  
Metadata do not contribute to the document size for service limits or pricing\. For more information, see [AWS IoT Service Limits](https://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html#limits_iot)\.

`timestamp`  <a name="timestamp"></a>
Indicates when the message was sent by AWS IoT\. By using the timestamp in the message and the timestamps for individual attributes in the `desired` or `reported` section, a device can determine a property's age, even if the device doesn't have an internal clock\.

`clientToken`  <a name="clientToken"></a>
A string unique to the device that enables you to associate responses with requests in an MQTT environment\.

`version`  <a name="version"></a>
The document version\. Every time the document is updated, this version number is incremented\. Used to ensure the version of the document being updated is the most recent\.

For more information, see [Shadow document examples](#device-shadow-document-syntax)\.

## Delta state<a name="delta-state"></a><a name="observing-state-changes"></a>

Delta state is a virtual type of state that contains the difference between the `desired` and `reported` states\. Fields in the `desired` section that are not in the `reported` section are included in the delta\. Fields that are in the `reported` section and not in the `desired` section are not included in the delta\. The delta contains metadata, and its values are equal to the metadata in the `desired` field\. For example:

```
{
  "state": {
    "desired": {
      "color": "RED",
      "state": "STOP"
    },
    "reported": {
      "color": "GREEN",
      "engine": "ON"
    },
    "delta": {
      "color": "RED",
      "state": "STOP"
    }
  },
  "metadata": {
    "desired": {
      "color": {
        "timestamp": 12345
      },
      "state": {
        "timestamp": 12345
      },
      "reported": {
        "color": {
          "timestamp": 12345
        },
        "engine": {
          "timestamp": 12345
        }
      },
      "delta": {
        "color": {
          "timestamp": 12345
        },
        "state": {
          "timestamp": 12345
        }
      }
    },
    "version": 17,
    "timestamp": 123456789
  }
}
```

When nested objects differ, the delta contains the path all the way to the root\.

```
{
  "state": {
    "desired": {
      "lights": {
        "color": {
          "r": 255,
          "g": 255,
          "b": 255
        }
      }
    },
    "reported": {
      "lights": {
        "color": {
          "r": 255,
          "g": 0,
          "b": 255
        }
      }
    },
    "delta": {
      "lights": {
        "color": {
          "g": 255
        }
      }
    }
  },
  "version": 18,
  "timestamp": 123456789
}
```

The Device Shadow service calculates the delta by iterating through each field in the `desired` state and comparing it to the `reported` state\.

Arrays are treated like values\. If an array in the `desired` section doesn't match the array in the `reported` section, then the entire desired array is copied into the delta\.

## Versioning shadow documents<a name="versioning"></a>

The Device Shadow service supports versioning on every update message, both request and response\. This means that with every update of a shadow, the version of the JSON document is incremented\. This ensures two things:
+ A client can receive an error if it attempts to overwrite a shadow using an older version number\. The client is informed it must resync before it can update a device's shadow\.
+ A client can decide not to act on a received message if the message has a lower version than the version stored by the client\. 

A client can bypass version matching by not including a version in the shadow document\.

## Client tokens in shadow documents<a name="client-token"></a>

You can use a client token with MQTT\-based messaging to verify the same client token is contained in a request and request response\. This ensures the response and request are associated\.

**Note**  
The client token can be no longer than 64 bytes\. A client token that is longer than 64 bytes causes a 400 \(Bad Request\) response and an *Invalid clientToken* error message\.

## Empty shadow document properties<a name="device-shadow-empty-fields"></a>

The `reported` and `desired` properties in a shadow document can be empty or omitted when they don't apply to the current shadow state\. For example, a shadow document contains a `desired` property only if it has a desired state\. The following is a valid example of a state document with no `desired` property:

```
{
    "reported" : { "temp": 55 }
}
```

The `reported` property can also be empty, such as if the shadow has not been updated by the device:

```
{
    "desired" : { "color" : "RED" }
}
```

If an update causes the `desired` or `reported` properties to become null, it is removed from the document\. The following shows how to remove the `desired` property by setting it to `null`\. You might do this when a device updates its state, for example\.

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

A shadow document can also have neither `desired` or `reported` properties, making the shadow document empty\. This is an example of an empty, yet valid shadow document\.

```
{
}
```

## Array values in shadow documents<a name="device-shadow-arrays"></a>

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