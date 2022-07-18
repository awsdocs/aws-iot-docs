# MQTT<a name="device-advisor-tests-mqtt"></a>

## CONNECT, DISCONNECT, and RECONNECT<a name="connect"></a>

**"Device send CONNECT to AWS IoT Core \(Happy case\)"**  <a name="MQTT_Connect"></a>
Validates that the device under test sends a CONNECT request\.   
*API test case definition:*  
`EXECUTION_TIMEOUT` has a default value of 5 minutes\. We recommend a timeout value of 2 minutes\. 

```
"tests":[
   {
      "name":"my_mqtt_connect_test",
      "configuration": {
         // optional:
         "EXECUTION_TIMEOUT":"300",   // in seconds
      },
      "test":{
         "id":"MQTT_Connect",
         "version":"0.0.0"
      }
   }
]
```

**"Device can return PUBACK to an arbitrary topic for QoS1"**  
This test case will check if device \(client\) can return a PUBACK message if it received a publish message from the broker after subscribing to a topic with QoS1\.  
*API test case definition:*  
`EXECUTION_TIMEOUT` has a default value of 5 minutes\. We recommend a timeout value of 2 minutes\. 

```
"tests":[
   {
        "name":"my_mqtt_client_puback_qos1",
        "configuration": {
            // optional:
            "TRIGGER_TOPIC": "myTopic",
            "EXECUTION_TIMEOUT":"300", // in seconds
            "PAYLOAD_FOR_PUBLISH_VALIDATION":"custom payload"
        },
        "test": {
            "id": "MQTT_Client_Puback_Qos1",
            "version": "0.0.0"
        }
    }
]
```

**"Device connect retries with jitter backoff \- No CONNACK response"**  
Validates that the device under test uses the proper jitter backoff when reconnecting with the broker for at least five times\. The broker logs the timestamp of the device under test's CONNECT request, performs packet validation, pauses without sending a CONNACK to the device under test, and waits for the device under test to resend the request\. The sixth connection attempt is allowed to pass through and CONNACK is allowed to flow back to the device under test\.  
The preceding process is performed again\. In total, this test case requires the device to connect at least 12 times in total\. The collected timestamps are used to validate that jitter backoff is used by the device under test\. If the device under test has a strictly exponential backoff delay, this test case will pass with warnings\.   
We recommend implementation of the [Exponential Backoff And Jitter](http://aws.amazon.com/blogs/architecture/exponential-backoff-and-jitter/) mechanism on the device under test to pass this test case\.  
*API test case definition:*  
`EXECUTION_TIMEOUT` has a default value of 5 minutes\. We recommend a timeout value of 4 minutes\. 

```
"tests":[
   {
      "name":"my_mqtt_jitter_backoff_retries_test",
      "configuration": {
         // optional:
         "EXECUTION_TIMEOUT":"300",    // in seconds
      },
      "test":{
         "id":"MQTT_Connect_Jitter_Backoff_Retries",
         "version":"0.0.0"
      }
   }
]
```

**"Device connect retries with exponential backoff \- No CONNACK response"**  
Validates that the device under test uses the proper exponential backoff when reconnecting with the broker for at least five times\. The broker logs the timestamp of the device under test's CONNECT request, performs packet validation, pauses without sending a CONNACK to the client device, and waits for the device under test to resend the request\. The collected timestamps are used to validate that an exponential backoff is used by the device under test\.   
We recommend implementation of the [Exponential Backoff And Jitter](http://aws.amazon.com/blogs/architecture/exponential-backoff-and-jitter/) mechanism on the device under test to pass this test case\.  
*API test case definition:*  
`EXECUTION_TIMEOUT` has a default value of 5 minutes\. We recommend a timeout value of 4 minutes\. 

```
"tests":[
   {
      "name":"my_mqtt_exponential_backoff_retries_test",
      "configuration": {
         // optional:
         "EXECUTION_TIMEOUT":"600",  // in seconds
      },
      "test":{
         "id":"MQTT_Connect_Exponential_Backoff_Retries",
         "version":"0.0.0"
      }
   }
]
```

**"Device re\-connect with at least one second delay \- After server disconnect"**  
Validates if a device under test waits for at least one second before reconnecting to the server when Device Advisor immediately disconnects the device after a successful connection\. If the device tries to reconnect in less than one second, the test will fail\.  
*API test case definition:*  
`EXECUTION_TIMEOUT` has a default value of 5 minutes\. We recommend a timeout value of 2 minutes\. 

```
"tests":[
   {  
      "name":"my_mqtt_reconnection_after_successful_connection",
      "configuration":{
        // optional:
        "EXECUTION_TIMEOUT":"300"  //in seconds
      },
      "test":{
         "id":"MQTT_ReconnectionAfterSuccessfulConnection",
         "version":"0.0.0"
      }
   }
]
```

**"Device re\-connect with jitter backoff \- After server disconnect"**  
Validates if a device under test uses necessary jitter and backoff while reconnecting after it's been disconnected from the server\. Device Advisor disconnects the device from the server for at least five times and observes the device's behavior for MQTT reconnection\. Device Advisor logs the timestamp of the CONNECT request for the device under test, performs packet validation, pauses without sending a CONNACK to the client device, and waits for the device under test to resend the request\. The collected timestamps are used to validate that the device under test uses jitter and backoff while reconnecting\. If the device under test has a strictly exponential backoff or doesn't implement a proper jitter backoff mechanism, this test case will pass with warnings\. If the device under test has implemented either a linear backoff or a constant backoff mechanism, the test will fail\.  
To pass this test case, we recommend implementing the [Exponential Backoff And Jitter](http://aws.amazon.com/blogs/architecture/exponential-backoff-and-jitter/) mechanism on the device under test\.  
*API test case definition:*  
`EXECUTION_TIMEOUT` has a default value of 5 minutes\. We recommend a timeout value of 4 minutes\.

```
"tests":[
   {
      "name":"my_mqtt_reconnect_backoff_retries_on_server_disconnect",
      "configuration":{
         // optional:
         "EXECUTION_TIMEOUT":"300",  // in seconds
         "RECONNECTION_ATTEMPTS": 5
      },
      "test":{
         "id":"MQTT_Reconnect_Backoff_Retries_On_Server_Disconnect",
         "version":"0.0.0"
      }
   }
]
```
The number of reconnection attempts to validate for backoff can be changed by specifying the `RECONNECTION_ATTEMPTS`\. The number must be between five and ten\. The default value is five\.

## Publish<a name="publish"></a>

**"QoS0 \(Happy Case\)"**  <a name="MQTT_Publish"></a>
Validates that the device under test publishes a message with QoS0\. You can also validate the topic of the message by specifying this topic value in the test settings\.  
*API test case definition:*  
`EXECUTION_TIMEOUT` has a default value of 5 minutes\. We recommend a timeout value of 2 minutes\. 

```
"tests":[
   {
      "name":"my_mqtt_publish_test",
      "configuration":{
         // optional:
         "EXECUTION_TIMEOUT":"300",  // in seconds
         "TOPIC_FOR_PUBLISH_VALIDATION": "my_TOPIC_FOR_PUBLISH_VALIDATION",
         "PAYLOAD_FOR_PUBLISH_VALIDATION": "my_PAYLOAD_FOR_PUBLISH_VALIDATION",
      },
      "test":{
         "id":"MQTT_Publish",
         "version":"0.0.0"
      }
   }
]
```

**"QoS1 publish retry \- No PUBACK"**  
Validates that the device under test republishes a message sent with QoS1, if the broker doesn't send PUBACK\. You can also validate the topic of the message by specifying this topic in the test settings\. The client device must not disconnect before republishing the message\. This test also validates that the republished message has the same packet identifier as the original\.  
*API test case definition:*  
`EXECUTION_TIMEOUT` has a default value of 5 minutes\. It is recommended for at least 4 minutes\.

```
"tests":[
   {
      "name":"my_mqtt_publish_retry_test",
      "configuration":{
         // optional:
         "EXECUTION_TIMEOUT":"300",  // in seconds
         "TOPIC_FOR_PUBLISH_VALIDATION": "my_TOPIC_FOR_PUBLISH_VALIDATION",
         "PAYLOAD_FOR_PUBLISH_VALIDATION": "my_PAYLOAD_FOR_PUBLISH_VALIDATION",
      },
      "test":{
         "id":"MQTT_Publish_Retry_No_Puback",
         "version":"0.0.0"
      }
   }
]
```

## Subscribe<a name="subscribe"></a>

**"Can Subscribe \(Happy Case\)"**  <a name="MQTT_Subscribe"></a>
Validates that the device under test subscribes to MQTT topics\. You can also validate the topic that the device under test subscribes to by specifying this topic in the test settings\.   
*API test case definition:*  
`EXECUTION_TIMEOUT` has a default value of 5 minutes\. We recommend a timeout value of 2 minutes\. 

```
"tests":[
   {
      "name":"my_mqtt_subscribe_test",
      "configuration":{
         // optional:
         "EXECUTION_TIMEOUT":"300",  // in seconds
         "TOPIC_LIST_FOR_SUBSCRIPTION_VALIDATION_ID":["my_TOPIC_FOR_PUBLISH_VALIDATION_a","my_TOPIC_FOR_PUBLISH_VALIDATION_b"]
      },
      "test":{
         "id":"MQTT_Subscribe",
         "version":"0.0.0"
      }
   }
]
```

**"Subscribe Retry \- No SUBACK"**  
Validates that the device under test retries a failed subscription to MQTT topics\. The server then waits and doesn't send a SUBACK\. If the client device doesn't retry the subscription, the test fails\. The client device must retry the failed subscription with the same packet Id\. You can also validate the topic that the device under test subscribes to by specifying this topic in the test settings\.  
*API test case definition:*  
`EXECUTION_TIMEOUT` has a default value of 5 minutes\. We recommend a timeout value of 4 minutes\. 

```
"tests":[
   {
      "name":"my_mqtt_subscribe_retry_test",
      "configuration":{
         "EXECUTION_TIMEOUT":"300",  // in seconds
         // optional:
         "TOPIC_LIST_FOR_SUBSCRIPTION_VALIDATION_ID":["myTOPIC_FOR_PUBLISH_VALIDATION_a","my_TOPIC_FOR_PUBLISH_VALIDATION_b"]
      },
      "test":{
         "id":"MQTT_Subscribe_Retry_No_Suback",
         "version":"0.0.0"
      }
   }
]
```

## Keep\-Alive<a name="keep-alive"></a>

**"Mqtt No Ack PingResp"**  
This test case validates if the device under test disconnects when it doesn't receive a ping response\. As part of this test case, Device Advisor blocks responses sent from AWS IoT Core for publish, subscribe, and ping requests\. It also validates if the device under test disconnects the MQTT connection\.  
*API test case definition:*  
`EXECUTION_TIMEOUT` has a default value of 5 minutes\. We recommend a timeout greater than 1\.5 times the `keepAliveTime` value\.

```
"tests":[
    {
       "name":"Mqtt No Ack PingResp",
       "configuration": 
          //optional:
          "EXECUTION_TIMEOUT":"306",   // in seconds
       },
       "test":{
          "id":"MQTT_No_Ack_PingResp",
          "version":"0.0.0"
       }
    }
]
```

## Persistent Session<a name="persistent-session"></a>

**"Persistent Session \(Happy Case\)"**  
This test case validates the device behavior when disconnected from a persistent session\. The test case checks if the device can reconnect, resume the subscriptions to its trigger topics without explicitly re\-subscribing, receive the stored messages in the topics, and work as expected during a persistent session\. When this test case passes, it indicates that the client device is able to maintain a persistent session with the AWS IoT Core broker in an expected manner\. For more information on AWS IoT Persistent Sessions, see [ Using MQTT persistent sessions ](https://docs.aws.amazon.com/iot/latest/developerguide/mqtt.html#mqtt-persistent-sessions)\.   
In this test case, the client device is expected to CONNECT with the AWS IoT Core with a clean session flag set to false, and then subscribe to a trigger topic\. After a successful subscription, the device will be disconnected by AWS IoT Core Device Advisor\. While the device is in a disconnected state, a QoS 1 message payload will be stored in that topic\. Device Advisor will then allow the client device to re\-connect with the test endpoint\. At this point, since there is a persistent session, the client device is expected to resume its topic subscriptions without sending any additional SUBSCRIBE packets and receive the QoS 1 message from the broker\. After re\-connecting, if the client device re\-subscribes to its trigger topic again by sending an additional SUBSCRIBE packet and/or if the client fails to receive the stored message from the trigger topic, the test case will fail\.  
*API test case definition:*  
`EXECUTION_TIMEOUT` has a default value of 5 minutes\. We recommend a timeout value of at least 4 minutes\. In the first connection, client device needs to explicitly subscribe to a `TRIGGER_TOPIC` which was not subscribed before\. To pass the test case, client device must successfully subscribe to `TRIGGER_TOPIC` with a QoS 1\. After re\-connecting, the client device is expected to understand that there is an active persistent session; so it should accept the stored message sent by the trigger topic and return PUBACK for that specific message\. 

```
"tests":[
   {
      "name":"my_mqtt_persistent_session_happy_case",
      "configuration":{
         //required:
         "TRIGGER_TOPIC": "myTrigger/topic",
         // optional:
         // if Payload not provided, a string will be stored in the trigger topic to be sent back to the client device
         "PAYLOAD": "The message which should be received from AWS IoT Broker after re-connecting to a persistent session from the specified trigger topic.",            
         "EXECUTION_TIMEOUT":"300" // in seconds
      },
      "test":{
         "id":"MQTT_Persistent_Session_Happy_Case",
         "version":"0.0.0"
      }
   }
]
```

**"Persistent Session \- Session Expiry"**  
This test case helps to validate device behavior when a disconnected device reconnects to an expired persistent session\. After the session expires, we expect the device to resubscribe to the topics previously subscribed to by explicitly sending a new SUBSCRIBE packet\.  
During the first connection, we expect the test device to CONNECT with the AWS IoT broker, as its `CleanSession` flag is set to false to initiate a persistent session\. The device should then subscribe to a trigger topic\. Then the device is disconnected by AWS IoT Core Device Advisor, after a successful subscription and initiation of a persistent session\. After the disconnection, AWS IoT Core Device Advisor allows the test device to re\-connect back with the test endpoint\. At this point, when the test device sends another CONNECT packet, AWS IoT Core Device Advisor sends back a CONNACK packet that indicates that the persistent session is expired\. The test device needs to interpret this packet properly, and it is expected to re\-subscribe to the same trigger topic again as the persistent session is terminated\. If the test device does not re\-subscribe to its topic trigger again, the test case fails\. For the test to pass, the device needs to understand that the persistent session is over, and send back a new SUBSCRIBE packet for the same trigger topic in the second connection\.  
If this test case passes for a test device, it indicates that the device is able to handle re\-connection on expiry of persistent session in an expected way\.  
*API test case definition:*  
`EXECUTION_TIMEOUT` has a default value of 5 minutes\. We recommend a timeout value of at least 4 minutes\. The test device needs to explicitly subscribe to a `TRIGGER_TOPIC`, to which it was not subscribed before\. To pass the test case, the test device must send a CONNECT packet with `CleanSession` flag set to false, and successfully subscribe to a trigger topic with a QoS 1\. After a successful connection, AWS IoT Core Device Advisor disconnects the device\. After the disconnection, AWS IoT Core Device Advisor allows the device to re\-connect back, and the device is expected to re\-subscribe to the same `TRIGGER_TOPIC` since AWS IoT Core Device Advisor would have terminated the persistent session\.

```
"tests":[
   {
      "name":"my_expired_persistent_session_test",
      "configuration":{
         //required:
         "TRIGGER_TOPIC": "myTrigger/topic",
         // optional:       
         "EXECUTION_TIMEOUT":"300" // in seconds
      },
      "test":{
         "id":"MQTT_Expired_Persistent_Session",
         "version":"0.0.0"
      }
   }
]
```