# Device shadow service for AWS IoT<a name="iot-device-shadows"></a>

A device's *shadow* is a JSON document that is used to store and retrieve current state information for a device\. The Device Shadow service maintains a shadow for each device you connect to AWS IoT\. You can use the shadow to get and set the state of a device over MQTT or HTTP, regardless of whether the device is connected to the Internet\. Each device's shadow is uniquely identified by the name of the corresponding thing\.

**Topics**
+ [Device shadow service data flow](device-shadow-data-flow.md)
+ [Device shadow service documents](device-shadow-document.md)
+ [Using shadows](using-device-shadows.md)
+ [Device shadow RESTful API](device-shadow-rest-api.md)
+ [Shadow MQTT topics](device-shadow-mqtt.md)
+ [Shadow document syntax](device-shadow-document-syntax.md)
+ [Shadow error messages](device-shadow-error-messages.md)