# Lifecycle Events<a name="life-cycle-events"></a>

AWS IoT publishes lifecycle events on the MQTT topics discussed in the following sections\. These messages allow you to be notified of lifecycle events from the message broker\.

**Note**  
Lifecycle messages might be sent out of order\. You might receive duplicate messages\.

## Connect/Disconnect Events<a name="connect-disconnect"></a>

AWS IoT publishes a message to the following MQTT topics when a client connects or disconnects:
+ `$aws/events/presence/connected/clientId` – A client connected to the message broker\.
+ `$aws/events/presence/disconnected/clientId` – A client disconnected from the message broker\.

The following is a list of JSON elements that are contained in the connection/disconnection messages published to the `$aws/events/presence/connected/clientId` topic\.

**clientId**  
The client ID of the connecting or disconnecting client\.  
Client IDs that contain \# or \+ do not receive lifecycle events\.

**clientInitiatedDisconnect**  
True if the client initiated the disconnect\. Otherwise, false\. Found in disconnection messages only\.

**disconnectReason**  
The reason why the client is disconnecting\. Found in disconnect messages only\. The following table contains valid values\.      
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/iot/latest/developerguide/life-cycle-events.html)

**eventType**  
The type of event\. Valid values are `connected` or `disconnected`\. 

**ipAddress**  
The IP address of the connecting client\. This can be in IPv4 or IPv6 format\. Found in connection messages only\. 

**principalIdentifier**  
The credential used to authenticate\. For TLS mutual authentication certificates, this is the certificate ID\. For other connections, this is IAM credentials\.

**sessionIdentifier**  
A globally unique identifier in AWS IoT that exists for the life of the session\.

**timestamp**  
An approximation of when the event occurred, expressed in milliseconds since the Unix epoch\. The accuracy of the timestamp is \+/\- 2 minutes\.

**versionNumber**  
The version number for the lifecycle event\. This is a monotonically increasing long integer value for each client ID connection\. The version number can be used by a subscriber to infer the order of lifecycle events\.  
The connect and disconnect messages for a client connection have the same version number\.  
The version number might skip values and is not guaranteed to be consistently increasing by 1 for each event\.  
If a client is not connected for approximately one hour, the version number is reset to 0\. For persistent sessions, the version number is reset to 0 after a client has been disconnected longer than the configured time\-to\-live \(TTL\) for the persistent session\.

A connect message has the following structure\.

```
 {
    "clientId": "186b5",
    "timestamp": 1573002230757,
    "eventType": "connected",
    "sessionIdentifier": "a4666d2a7d844ae4ac5d7b38c9cb7967",
    "principalIdentifier": "12345678901234567890123456789012",
    "ipAddress": "192.0.2.0",
    "versionNumber": 0
}
```

A disconnect message has the following structure\.

```
{
    "clientId": "186b5",
    "timestamp": 1573002340451,
    "eventType": "disconnected",
    "sessionIdentifier": "a4666d2a7d844ae4ac5d7b38c9cb7967",
    "principalIdentifier": "12345678901234567890123456789012",
    "clientInitiatedDisconnect": true,
    "disconnectReason": "CLIENT_INITIATED_DISCONNECT",
    "versionNumber": 0
}
```

### Handling Client Disconnections<a name="reconnect"></a>

The best practice is to always have a wait state implemented for lifecycle events, including Last Will and Testament \(LWT\) messages\. When a disconnect message is received, your code should wait a period of time and verify a device is still offline before taking action\. One way to do this is by using [SQS Delay Queues](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-delay-queues.html)\. When a client receives a LWT or a lifecycle event, you can enqueue a message \(for example, for 5 seconds\)\. When that message becomes available and is processed \(by Lambda or another service\), you can first check if the device is still offline before taking further action\.

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
    "principalIdentifier": "000000000000/ABCDEFGHIJKLMNOPQRSTU:some-user/ABCDEFGHIJKLMNOPQRSTU:some-user",
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
