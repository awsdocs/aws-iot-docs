# WebSocket messages and status codes<a name="conect-iot-lorawan-network-analyer-messages-status"></a>

After you've created a presigned request, you can use the request URL with your WebSocket library, , or a library that's suited to your programming language, to make requests to the service\. For more information about how you can generate this presigned request, see [Generate a presigned request with the WebSocket library](connect-iot-lorawan-network-analyzer-generate-request.md)\.

## WebSocket messages<a name="conect-iot-lorawan-network-analyer-messages"></a>

The WebSocket protocol can be used to establish a bi\-directional connection\. Messages can be transmitted from client to server and from server to client\. However, network analyzer supports only messages that are sent from server to client\. Any message received from the client is unexpected and the server will automatically close the WebSocket connection if a message is received from client\.

When the request is received and a trace messaging session has started, the server responds with a JSON structure, which is the payload\. For more information about the payload and how you can activate trace messaging from the AWS Management Console, see [View and monitor network analyzer trace message logs in real time](connect-iot-lorawan-network-analyzer-logs.md)\.

## WebSocket status codes<a name="conect-iot-lorawan-network-analyer-status-codes"></a>

The following shows the WebSocket status codes for the communication from the server to client\. The WebSocket status codes follow the [RFC Standard of Normal closure of connections](https://datatracker.ietf.org/doc/html/rfc6455#section-7.3)\.

The following shows the supported status codes:
+ 

**1000**  
This status code indicates a normal closure, which means that the WebSocket connection has been established and the request has been fulfilled\. This status can be observed when a session is idle, causing the connection to time out\.
+ 

**1002**  
This status code indicates that the endpoint is terminating the connection because of a protocol error\.
+ 

**1003**  
This status code indicates an error status where the endpoint terminated the connection because it received data in a format that it can't accept\. The endpoint supports only text data and might display this status code if it receives a binary message or a message from the client that's using an unsupported format\.
+ 

**1008**  
This status code indicates an error status where the endpoint terminated the connection because it received a message that violates its policy\. This status is generic and is displayed when the other status codes, such as 1003 or 1009, aren't applicable\. You'll also see this status displayed if there's a need to hide the policy, or when there's an authorization failure, such as an expired signature\.
+ 

**1011**  
This status code indicates an error status where the server is terminating the connection because it encountered an unexpected condition or internal error that prevented it from fulfilling the request\.

## Next steps<a name="connect-iot-lorawan-network-analyzer-websockets-next"></a>

Now that you've learned how to generate a presigned request and how you can observe messages from the server by using the WebSocket connection, you can activate trace messaging and start receiving message logs for your wireless gateway and wireless device resources\. For more information, see [View and monitor network analyzer trace message logs in real time](connect-iot-lorawan-network-analyzer-logs.md)\.