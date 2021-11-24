# Interacting with shadows<a name="device-shadow-data-flow"></a>

This topic describes the messages associated with each of the three methods that AWS IoT provides for working with shadows\. These methods include the following:

`UPDATE`  <a name="update"></a>
Creates a shadow if it doesn't exist, or updates the contents of an existing shadow with the state information provided in the message body\. AWS IoT records a timestamp with each update to indicate when the state was last updated\. When the shadow's state changes, AWS IoT sends `/delta` messages to all MQTT subscribers with the difference between the `desired` and the `reported` states\. Devices or apps that receive a `/delta` message can perform actions based on the difference\. For example, a device can update its state to the desired state, or an app can update its UI to reflect the device's state change\.

`GET`  <a name="get"></a>
Retrieves a current shadow document that contains the complete state of the shadow, including metadata\.

`DELETE`  <a name="delete"></a>
Deletes the device shadow and its content\.  
You can't restore a deleted device shadow document, but you can create a new device shadow with the name of a deleted device shadow document\. If you create a device shadow document that has the same name as one that was deleted within the past 48 hours, the version number of the new device shadow document will follow that of the deleted one\. If a device shadow document has been deleted for more than 48 hours, the version number of a new device shadow document with the same name will be 0\.

## Protocol support<a name="protocol-support"></a>

AWS IoT supports [MQTT](http://docs.oasis-open.org/mqtt/mqtt/v3.1.1/mqtt-v3.1.1.html) and a REST API over HTTPS protocols to interact with shadows\. AWS IoT provides a set of reserved request and response topics for MQTT publish and subscribe actions\. Devices and apps should subscribe to the response topics before publishing a request topic for information about how AWS IoT handled the request\. For more information, see [Device Shadow MQTT topics](device-shadow-mqtt.md) and [Device Shadow REST API](device-shadow-rest-api.md)\.

## Requesting and reporting state<a name="shadow-reporting-state"></a>

When designing your IoT solution using AWS IoT and shadows, you should determine the apps or devices that will request changes and those that will implement them\. Typically, a device implements and reports changes back to the shadow and apps and services respond to and request changes in the shadow\. Your solution could be different, but the examples in this topic assume that the client app or service requests changes in the shadow and the device performs the changes and reports them back to the shadow\.

## Updating a shadow<a name="update-device-shadow"></a>

Your app or service can update a shadow's state by using the [UpdateThingShadow](device-shadow-rest-api.md#API_UpdateThingShadow) API or by publishing to the [/update](device-shadow-mqtt.md#update-pub-sub-topic) topic\. Updates affect only the fields specified in the request\.

### Updating a shadow when a client requests a state change<a name="update-pub-sub-topic-client"></a>

**When a client requests a state change in a shadow by using the MQTT protocol**

1. The client should have a current shadow document so that it can identify the properties to change\. See the /get action for how to obtain the current shadow document\.

1. The client subscribes to these MQTT topics:
   + `$aws/things/thingName/shadow/name/shadowName/update/accepted`
   + `$aws/things/thingName/shadow/name/shadowName/update/rejected`
   + `$aws/things/thingName/shadow/name/shadowName/update/delta`
   + `$aws/things/thingName/shadow/name/shadowName/update/documents`

1. The client publishes a `$aws/things/thingName/shadow/name/shadowName/update` request topic with a state document that contains the desired state of the shadow\. Only the properties to change need to be included in the document\. This is an example of a document with the desired state\.

   ```
   {
     "state": {
       "desired": {
         "color": {
           "r": 10
         },
         "engine": "ON"
       }
     }
   }
   ```

1. If the update request is valid, AWS IoT updates the desired state in the shadow and publishes messages on these topics:
   + `$aws/things/thingName/shadow/name/shadowName/update/accepted`
   + `$aws/things/thingName/shadow/name/shadowName/update/delta`

   The `/update/accepted` message contains an [/accepted response state document](device-shadow-document.md#device-shadow-example-response-json-accepted) shadow document, and the `/update/delta` message contains a [/delta response state document](device-shadow-document.md#device-shadow-example-response-json-delta) shadow document\. 

1. If the update request is not valid, AWS IoT publishes a message with the `$aws/things/thingName/shadow/name/shadowName/update/rejected` topic with an [Error response document](device-shadow-document.md#device-shadow-example-error-json) shadow document that describes the error\.

**When a client requests a state change in a shadow by using the API**

1. The client calls the `UpdateThingShadow` API with a [Request state document](device-shadow-document.md#device-shadow-example-request-json) state document as its message body\.

1. If the request was valid, AWS IoT returns an HTTP success response code and an [/accepted response state document](device-shadow-document.md#device-shadow-example-response-json-accepted) shadow document as its response message body\.

   AWS IoT will also publish an MQTT message to the `$aws/things/thingName/shadow/name/shadowName/update/delta` topic with a [/delta response state document](device-shadow-document.md#device-shadow-example-response-json-delta) shadow document for any devices or clients that subscribe to it\.

1. If the request was not valid, AWS IoT returns an HTTP error response code an [Error response document](device-shadow-document.md#device-shadow-example-error-json) as its response message body\.

When the device receives the `/desired` state on the `/update/delta` topic, it makes the desired changes in the device\. It then sends a message to the `/update` topic to report its current state to the shadow\. 

### Updating a shadow when a device reports its current state<a name="update-pub-sub-topic-device"></a>

**When a device reports its current state to the shadow by using the MQTT protocol**

1. The device should subscribe to these MQTT topics before updating the shadow:
   + `$aws/things/thingName/shadow/name/shadowName/update/accepted`
   + `$aws/things/thingName/shadow/name/shadowName/update/rejected`
   + `$aws/things/thingName/shadow/name/shadowName/update/delta`
   + `$aws/things/thingName/shadow/name/shadowName/update/documents`

1. The device reports its current state by publishing a message to the `$aws/things/thingName/shadow/name/shadowName/update` topic that reports the current state, such as in this example\.

   ```
   {
       "state": {
           "reported" : {
               "color" : { "r" : 10 },
               "engine" : "ON"
           }
       }
   }
   ```

1. If AWS IoT accepts the update, it publishes a message to the `$aws/things/thingName/shadow/name/shadowName/update/accepted` topics with an [/accepted response state document](device-shadow-document.md#device-shadow-example-response-json-accepted) shadow document\.

1. If the update request is not valid, AWS IoT publishes a message with the `$aws/things/thingName/shadow/name/shadowName/update/rejected` topic with an [Error response document](device-shadow-document.md#device-shadow-example-error-json) shadow document that describes the error\.

**When a device reports its current state to the shadow by using the API**

1. The device calls the `UpdateThingShadow` API with a [Request state document](device-shadow-document.md#device-shadow-example-request-json) state document as its message body\.

1. If the request was valid, AWS IoT updates the shadow and returns an HTTP success response code with an [/accepted response state document](device-shadow-document.md#device-shadow-example-response-json-accepted) shadow document as its response message body\.

   AWS IoT will also publish an MQTT message to the `$aws/things/thingName/shadow/name/shadowName/update/delta` topic with a [/delta response state document](device-shadow-document.md#device-shadow-example-response-json-delta) shadow document for any devices or clients that subscribe to it\.

1. If the request was not valid, AWS IoT returns an HTTP error response code an [Error response document](device-shadow-document.md#device-shadow-example-error-json) as its response message body\.

### Optimistic locking<a name="optimistic-locking"></a>

You can use the state document version to ensure you are updating the most recent version of a device's shadow document\. When you supply a version with an update request, the service rejects the request with an HTTP 409 conflict response code if the current version of the state document does not match the version supplied\.

For example:

Initial document:

```
{
  "state": {
    "desired": {
      "colors": [
        "RED",
        "GREEN",
        "BLUE"
      ]
    }
  },
  "version": 10
}
```

Update: \(version doesn't match; this request will be rejected\)

```
{
  "state": {
    "desired": {
      "colors": [
        "BLUE"
      ]
    }
  },
  "version": 9
}
```

Result:

```
{
  "code": 409,
  "message": "Version conflict",
  "clientToken": "426bfd96-e720-46d3-95cd-014e3ef12bb6"
}
```

Update: \(version matches; this request will be accepted\)

```
{
  "state": {
    "desired": {
      "colors": [
        "BLUE"
      ]
    }
  },
  "version": 10
}
```

Final state:

```
{
  "state": {
    "desired": {
      "colors": [
        "BLUE"
      ]
    }
  },
  "version": 11
}
```

## Retrieving a shadow document<a name="retrieving-device-shadow"></a>

You can retrieve a shadow document by using the [GetThingShadow](device-shadow-rest-api.md#API_GetThingShadow) API or by subscribing and publishing to the [/get](device-shadow-mqtt.md#get-pub-sub-topic) topic\. This retrieves a complete shadow document, including any delta between the `desired` and `reported` states\. The procedure for this task is the same whether the device or a client is making the request\.

**To retrieve a shadow document by using the MQTT protocol**

1. The device or client should subscribe to these MQTT topics before updating the shadow:
   + `$aws/things/thingName/shadow/name/shadowName/get/accepted`
   + `$aws/things/thingName/shadow/name/shadowName/get/rejected`

1. The device or client publishes a message to the `$aws/things/thingName/shadow/name/shadowName/get` topic with an empty message body\.

1. If the request is successful, AWS IoT publishes a message to the `$aws/things/thingName/shadow/name/shadowName/get/accepted` topic with a [/accepted response state document](device-shadow-document.md#device-shadow-example-response-json-accepted) in the message body\.

1. If the request was not valid, AWS IoT publishes a message to the `$aws/things/thingName/shadow/name/shadowName/get/rejected` topic with an [Error response document](device-shadow-document.md#device-shadow-example-error-json) in the message body\.

**To retrieve a shadow document by using a REST API**

1. The device or client call the `GetThingShadow` API with an empty message body\.

1. If the request is valid, AWS IoT returns an HTTP success response code with an [/accepted response state document](device-shadow-document.md#device-shadow-example-response-json-accepted) shadow document as its response message body\.

1. If the request is not valid, AWS IoT returns an HTTP error response code an [Error response document](device-shadow-document.md#device-shadow-example-error-json) as its response message body\.

## Deleting shadow data<a name="deleting-thing-data"></a>

There are two ways to delete shadow data: you can delete specific properties in the shadow document and you can delete the shadow completely\.
+ To delete specific properties from a shadow, update the shadow; however set the value of the properties that you want to delete to `null`\. Fields with a value of `null` are removed from the shadow document\.
+ To delete the entire shadow, use the [DeleteThingShadow](device-shadow-rest-api.md#API_DeleteThingShadow) API or publish to the [/delete](device-shadow-mqtt.md#delete-pub-sub-topic) topic\.

Note that deleting a shadow does not reset its version number to 0\.

### Deleting a property from a shadow document<a name="deleting-shadow-property"></a>

**To delete a property from a shadow by using the MQTT protocol**

1. The device or client should have a current shadow document so that it can identify the properties to change\. See [Retrieving a shadow document](#retrieving-device-shadow) for information on how to obtain the current shadow document\.

1. The device or client subscribes to these MQTT topics:
   + `$aws/things/thingName/shadow/name/shadowName/update/accepted`
   + `$aws/things/thingName/shadow/name/shadowName/update/rejected`

1. The device or client publishes a `$aws/things/thingName/shadow/name/shadowName/update` request topic with a state document that assigns `null` values to the properties of the shadow to delete\. Only the properties to change need to be included in the document\. This is an example of a document that deletes the `engine` property\.

   ```
   {
     "state": {
       "desired": {
         "engine": null
       }
     }
   }
   ```

1. If the update request is valid, AWS IoT deletes the specified properties in the shadow and publishes a messages with the `$aws/things/thingName/shadow/name/shadowName/update/accepted` topic with an [/accepted response state document](device-shadow-document.md#device-shadow-example-response-json-accepted) shadow document in the message body\. 

1. If the update request is not valid, AWS IoT publishes a message with the `$aws/things/thingName/shadow/name/shadowName/update/rejected` topic with an [Error response document](device-shadow-document.md#device-shadow-example-error-json) shadow document that describes the error\.

**To delete a property from a shadow by using the REST API**

1. The device or client calls the `UpdateThingShadow` API with a [Request state document](device-shadow-document.md#device-shadow-example-request-json) that assigns `null` values to the properties of the shadow to delete\. Include only the properties that you want to delete in the document\. This is an example of a document that deletes the `engine` property\.

   ```
   {
     "state": {
       "desired": {
         "engine": null
       }
     }
   }
   ```

1. If the request was valid, AWS IoT returns an HTTP success response code and an [/accepted response state document](device-shadow-document.md#device-shadow-example-response-json-accepted) shadow document as its response message body\.

1. If the request was not valid, AWS IoT returns an HTTP error response code an [Error response document](device-shadow-document.md#device-shadow-example-error-json) as its response message body\.

### Deleting a shadow<a name="deleting-device-shadow"></a>

**Note**  
Setting the device's shadow state to `null` does not delete the shadow\. The shadow version will be incremented on the next update\.  
Deleting a device's shadow does not delete the thing object\. Deleting a thing object does not delete the corresponding device's shadow\.  
Deleting a shadow does not reset its version number to 0\.

**To delete a shadow by using the MQTT protocol**

1. The device or client subscribes to these MQTT topics:
   + `$aws/things/thingName/shadow/name/shadowName/delete/accepted`
   + `$aws/things/thingName/shadow/name/shadowName/delete/rejected`

1. The device or client publishes a `$aws/things/thingName/shadow/name/shadowName/delete` with an empty message buffer\.

1. If the delete request is valid, AWS IoT deletes the shadow and publishes a messages with the `$aws/things/thingName/shadow/name/shadowName/delete/accepted` topic and an abbreviated [/accepted response state document](device-shadow-document.md#device-shadow-example-response-json-accepted) shadow document in the message body\. This is an example of the accepted delete message:

   ```
   {
     "version": 4,
     "timestamp": 1591057529
   }
   ```

1. If the update request is not valid, AWS IoT publishes a message with the `$aws/things/thingName/shadow/name/shadowName/delete/rejected` topic with an [Error response document](device-shadow-document.md#device-shadow-example-error-json) shadow document that describes the error\.

**To delete a shadow by using the REST API**

1. The device or client calls the `DeleteThingShadow` API with an empty message buffer\.

1. If the request was valid, AWS IoT returns an HTTP success response code and an [/accepted response state document](device-shadow-document.md#device-shadow-example-response-json-accepted) and an abbreviated [/accepted response state document](device-shadow-document.md#device-shadow-example-response-json-accepted) shadow document in the message body\. This is an example of the accepted delete message:

   ```
   {
     "version": 4,
     "timestamp": 1591057529
   }
   ```

1. If the request was not valid, AWS IoT returns an HTTP error response code an [Error response document](device-shadow-document.md#device-shadow-example-error-json) as its response message body\.