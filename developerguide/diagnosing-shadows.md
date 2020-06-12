# Diagnosing problems with shadows<a name="diagnosing-shadows"></a>


**Diagnosing shadows**  

| Issue | Troubleshooting guidelines | 
| --- | --- | 
| A device's shadow document is rejected with Invalid JSON document\. | If you are unfamiliar with JSON, modify the examples provided in this guide for your own use\. For more information, see [Shadow document examples](device-shadow-document.md#device-shadow-document-syntax)\. | 
| I submitted correct JSON, but none or only parts of it are stored in the device's shadow document\. | Be sure you are following the JSON formatting guidelines\. Only JSON fields in the desired and reported sections are stored\. JSON content \(even if formally correct\) outside of those sections is ignored\. | 
| I received an error that the device's shadow exceeds the allowed size\. | The device's shadow supports 8 KB of data only\. Try shortening field names inside of your JSON document or simply create more shadows by creating more things\. A device can have an unlimited number of things/shadows associated with it\. The only requirement is that each thing name must be unique in your account\. | 
| When I receive a device's shadow, it is larger than 8 KB\. How can this happen? | Upon receipt, the AWS IoT service adds metadata to the device's shadow\. The service includes this data in its response, but it does not count toward the limit of 8 KB\. Only the data for desired and reported state inside the state document sent to the device's shadow counts toward the limit\. | 
| My request has been rejected due to incorrect version\. What should I do? | Perform a GET operation to sync to the latest state document version\. When using MQTT, subscribe to the \./update/accepted topic to be notified about state changes and receive the latest version of the JSON document\. | 
| The timestamp is off by several seconds\. | The timestamp for individual fields and the whole JSON document is updated when the document is received by the AWS IoT service or when the state document is published onto the \./update/accepted and \./update/delta message\. Messages can be delayed over the network, which can cause the timestamp to be off by a few seconds\. | 
| My device can publish and subscribe on the corresponding shadow topics, but when I attempt to update the shadow document over the HTTP REST API, I get HTTP 403\. | Be sure you have created policies in IAM to allow access to these topics and for the corresponding action \(UPDATE/GET/DELETE\) for the credentials you are using\. IAM policies and certificate policies are independent\. | 
| Other issues\. | The Device Shadow service logs errors to CloudWatch Logs\. To identify device and configuration issues, enable CloudWatch Logs and view the logs for debug information\.  | 