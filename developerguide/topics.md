# Topics<a name="topics"></a>

Topics identify AWS IoT messages\. AWS IoT clients identify the messages they publish by giving the messages topic names\. Clients identify the messages to which they want to subscribe \(receive\) by registering a topic filter with AWS IoT Core\. The AWS IoT message broker uses topic names and topic filters to route messages from publishing clients to subscribing clients\. 

While AWS IoT supports some [reserved system topics](reserved-topics.md), most MQTT topics are created and managed by you, the system designer\. AWS IoT uses topics to identify messages received from publishing clients and select messages to send to subscribing clients, as described in the following sections\. Before you create a topic namespace for your system, review the characteristics of MQTT topics to create the hierarchy of topic names that works best for your IoT system\.

## Topic names<a name="topicnames"></a>

Topic names and topic filters are UTF\-8 encoded strings\. They can represent a hierarchy of information by using the forward slash \(/\) character to separate the levels of the hierarchy\. For example, this topic name could refer to a temperature sensor in room 1:
+ `sensor/temperature/room1`

In this example, there might also be other types of sensors in other rooms with topic names such as:
+ `sensor/temperature/room2`
+ `sensor/humidity/room1`
+ `sensor/humidity/room2`

**Note**  
As you consider topic names for the messages in your system, keep in mind:  
Topic names and topic filters are case sensitive\.
Topic names must not contain personally identifiable information\.
Topic names that begin with a $ are [reserved topics](reserved-topics.md) to be used only by AWS IoT Core\.
AWS IoT Core cannot send or receive messages between AWS accounts or Regions\.

For more information on desigining your topic names and namespace, see our whitepaper, [Designing MQTT Topics for AWS IoT Core](https://d1.awsstatic.com/whitepapers/Designing_MQTT_Topics_for_AWS_IoT_Core.pdf)\.

For examples of how apps can publish and subscribe to messages, start with [Getting started with AWS IoT Core](iot-gs.md) and [AWS IoT device and mobile SDKs ](iot-sdks.md)\.

**Important**  
The topic namespace is limited to an AWS account and Region\. For example, the `sensor/temp/room1` topic used by an AWS account in one Region is distinct from the `sensor/temp/room1` topic used by the same AWS account in another Region or used by any other AWS account in any Region\.

## Topic filters<a name="topicfilters"></a>

Subscribing clients register topic filters with the AWS IoT message broker to specify the message topics that the message broker should send to them\. A topic filter can be a single topic name to subscribe to a single topic name or it can include wildcard characters to subscribe to multiple topic names at once\.

Publishing clients cannot use wildcard characters in the topic names they publish\. 

The following table lists the wildcard characters that can be used in a topic filter\. 


**Topic wildcards**  

| Wildcard character | Matches | Notes | 
| --- | --- | --- | 
| \# | All strings at and below its level in the topic hierarchy\. |  Must be the last character in the topic filter\.  Must be the only character in its level of the topic hierarchy\. Can be used in a topic filter that also contains the \+ wildcard character\.  | 
| \+ | Any string in the level that contains the character\. |  Must be the only character in its level of the topic hierarchy\. Can be used in multiple levels of a topic filter\.  | 

Using wildcards with the previous sensor topic name examples:
+ A subscription to `sensor/#` receives messages published to `sensor/`, `sensor/temperature`, `sensor/temperature/room1`, but not messages published to `Sensor`\. 
+ A subscription to `sensor/+/room1` receives messages published to `sensor/temperature/room1` and `sensor/humidity/room1`, but not messages sent to `sensor/temperature/room2` or `sensor/humidity/room2`\.