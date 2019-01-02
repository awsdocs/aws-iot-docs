# Lifecycle Events<a name="life-cycle-events"></a>

AWS IoT publishes lifecycle events on the MQTT topics discussed in the following sections\. These messages allow you to be notified of lifecycle events from the message broker\.

**Note**  
Lifecycle messages might be sent out of order\. You might receive duplicate messages\.

## Connect/Disconnect Events<a name="connect-disconnect"></a>

AWS IoT publishes a message to the following MQTT topics when a client connects or disconnects:

```
$aws/events/presence/connected/clientId
```

 or 

```
$aws/events/presence/disconnected/clientId
```

Where `clientId` is the MQTT client ID that connects to or disconnects from the AWS IoT message broker\.

The message published to this topic has the following structure:

```
{
    "clientId": "a1b2c3d4e5f6a7b8c9d0e1f2a3b4c5d6",
    "timestamp": 1460065214626,
    "eventType": "connected",
    "sessionIdentifier": "00000000-0000-0000-0000-000000000000",
    "principalIdentifier": "000000000000/ABCDEFGHIJKLMNOPQRSTU:some-user/ABCDEFGHIJKLMNOPQRSTU:some-user",
    "clientInitiatedDisconnect": false
}
```

The following is a list of JSON elements that are contained in the connection/disconnection messages published to the `$aws/events/presence/connected/clientId` topic\.

clientId  
The client ID of the connecting or disconnecting client\.  
Client IDs that contain \# or \+ do not receive lifecycle events\.

clientInitiatedDisconnect  
Only found in disconnection messages\. True if the client initiated the disconnect, otherwise False\.

eventType  
The type of event\. Valid values are `connected` or `disconnected`\. 

principalIdentifier  
The credential used to authenticate\. For TLS mutual authentication certificates, this is the certificate ID\. For other connections, this is IAM credentials\.

sessionIdentifier  
A globally unique identifier in AWS IoT that exists for the life of the session\.

timestamp  
An approximation of when the event occurred, expressed in milliseconds since the Unix epoch\. The accuracy of the timestamp is \+/\- 2 minutes\.

## Subscribe/Unsubscribe Events<a name="subscribe-unsubscribe-events"></a>

AWS IoT publishes a message to the following MQTT topic when a client subscribes or unsubscribes to an MQTT topic:

```
$aws/events/subscriptions/subscribed/clientId
```

 or 

```
$aws/events/subscriptions/unsubscribed/clientId
```

Where `clientId` is the MQTT client ID that connects to the AWS IoT message broker\.

The message published to this topic has the following structure:

```
{
    "clientId": "186b5",
    "timestamp": 1460065214626,
    "eventType": "subscribed" | "unsubscribed",
    "sessionIdentifier": "00000000-0000-0000-0000-000000000000",
    "principalIdentifier": "000000000000/ABCDEFGHIJKLMNOPQRSTU:some-user/ABCDEFGHIJKLMNOPQRSTU:some-user"
    "topics" : ["foo/bar","device/data","dog/cat"]
}
```

The following is a list of JSON elements that are contained in the subscribed and unsubscribed messages published to the `$aws/events/subscriptions/subscribed/clientId` and `$aws/events/subscriptions/unsubscribed/clientId` topics\.

clientId  
The client ID of the subscribing or unsubscribing client\.  
Client IDs that contain \# or \+ do not receive lifecycle events\.

eventType  
The type of event\. Valid values are `subscribed` or `unsubscribed`\. 

principalIdentifier  
The credential used to authenticate\. For TLS mutual authentication certificates, this is the certificate ID\. For other connections, this is IAM credentials\.

sessionIdentifier  
A globally unique identifier in AWS IoT that exists for the life of the session\.

timestamp  
An approximation of when the event occurred, expressed in milliseconds since the Unix epoch\. The accuracy of the timestamp is \+/\- 2 minutes\.

topics  
An array of the MQTT topics to which the client has subscribed\.

**Note**  
Lifecycle messages might be sent out of order\. You might receive duplicate messages\.