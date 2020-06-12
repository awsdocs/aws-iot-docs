# Device Shadow error messages<a name="device-shadow-error-messages"></a>

The Device Shadow service publishes a message on the error topic \(over MQTT\) when an attempt to change the state document fails\. This message is only emitted as a response to a publish request on one of the reserved $aws topics\. If the client updates the document using the REST API, then it receives the HTTP error code as part of its response, and no MQTT error messages are emitted\.


****  

| HTTP error code | Error messages | 
| --- | --- | 
| 400 \(Bad Request\) |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/iot/latest/developerguide/device-shadow-error-messages.html)  | 
| 401 \(Unauthorized\) |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/iot/latest/developerguide/device-shadow-error-messages.html)  | 
| 403 \(Forbidden\) |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/iot/latest/developerguide/device-shadow-error-messages.html)  | 
| 404 \(Not Found\) |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/iot/latest/developerguide/device-shadow-error-messages.html)  | 
| 409 \(Conflict\) |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/iot/latest/developerguide/device-shadow-error-messages.html)  | 
| 413 \(Payload Too Large\) |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/iot/latest/developerguide/device-shadow-error-messages.html)  | 
| 415 \(Unsupported Media Type\) |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/iot/latest/developerguide/device-shadow-error-messages.html)  | 
| 429 \(Too Many Requests\) |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/iot/latest/developerguide/device-shadow-error-messages.html)  | 
| 500 \(Internal Server Error\) |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/iot/latest/developerguide/device-shadow-error-messages.html)  | 