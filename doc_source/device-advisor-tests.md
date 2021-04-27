# Device Advisor test cases<a name="device-advisor-tests"></a>


|  | 
| --- |
| Device Advisor is in preview and is subject to change\. | 

Device Advisor provides prebuilt tests in two categories\.
+ Reliable connectivity

  Device connects and stays connected with AWS IoT Core\.
+ Secure connectivity

  Validate that your devices follow recommended security best practices\.

`EXECUTION_TIMEOUT` is in milliseconds\.

**Note**  
Your device needs to pass the following tests for qualification and to be able to list your device in the AWS Partner Device Catalog:  
**TLS Incorrect Subject Name Server Cert** \("Incorrect Subject Common Name \(CN\) / Subject Alternative Name \(SAN\)"\)
**TLS Unsecure Server Cert** \("Not Signed By Recognized CA"\)​
**TLS Connect** \("TLS Connect"\)​
**MQTT Connect** \("Device send CONNECT to AWS IoT Core \(Happy case\)"\)​
**MQTT Subscribe** \("Can Subscribe \(Happy Case\)"\)​
**MQTT Publish** \("QoS0 \(Happy Case\)"\)​

## TLS<a name="device-advisor-tests-tls"></a>

You use these tests to determine if the transport layer security protocol \(TLS\) between your devices and AWS IoT is secure\.Cipher suites

**"TLS Connect"**  
Validates if the client device can complete TLS handshake to AWS IoT\. This test doesn't validate the MQTT implementation of the client device\.  
*API test case definition:*  
`EXECUTION_TIMEOUT` has a default value of 5 minutes\.

```
"tests":[
   {
      "name":"my_tls_connect_test"
      "configuration": {
         // optional:
         "EXECUTION_TIMEOUT":"300000",
      },
      "test":{
         "id":"TLS_Connect",
         "version":"0.0.0"
      }
   }
]
```
Test case outputs:  
Pass: The client device completed TLS handshake with AWS IoT\.  
Pass with warnings: The client device completed TLS handshake with AWS IoT, but there were TLS warning messages from the device or AWS IoT\.  
Fail: The client device failed to complete TLS handshake with AWS IoT due to handshake error\.

**"TLS Device Support for AWS IoT recommended Cipher Suites"**  
Validates that the cipher suites in the TLS Client Hello message from the client device contains [AWS IoT recommended cipher suites](transport-security.md)\. It provides additional insights into cipher suites supported by the device\.  
*API test case definition:*  
`EXECUTION_TIMEOUT` \(optional\) is in milliseconds\.  
`EXECUTION_TIMEOUT` has a default value of 5 minutes\.

```
"tests":[
   {
      "name":"my_tls_support_aws_iot_cipher_suites_test"
      "configuration": {
         // optional:
         "EXECUTION_TIMEOUT":"300000",
      },
      "test":{
         "id":"TLS_Support_AWS_IoT_Cipher_Suites",
         "version":"0.0.0"
      }
   }
]
```
Test case outputs:  
Pass: The client device cipher suites contain at least one AWS IoT recommended cipher suite and don't contain any unsupported cipher suites\.  
Pass with warnings: The device cipher suites contain at least one AWS IoT cipher suite but 1\) don't contain any of the recommended cipher suites, or 2\) contain cipher suites not supported by AWS IoT\. We suggest verifying that unsupported cipher suites are safe\.   
Fail: The client device cipher suites don't contain any of the AWS IoT supported cipher suites\.Bad server certificate

**"Not Signed By Recognized CA"**  
Validates that the client device closes the connection if it's presented with a server certificate that doesn't have a valid signature from the ATS CA\. A device should only connect to an endpoint that presents a valid certificate\.  
*API test case definition:*  
`EXECUTION_TIMEOUT` has a default value of 5 minutes\.

```
"tests":[
   {
      "name":"my_tls_unsecure_server_cert_test"
      "configuration": {
         // optional:
         "EXECUTION_TIMEOUT":"300000",
      },
      "test":{
         "id":"TLS_Unsecure_Server_Cert",
         "version":"0.0.0"
      }
   }
]
```
Test case outputs:  
Pass: The client device closed the connection\.  
Fail: The client device completed TLS handshake with AWS IoT\.

**"Incorrect Subject Common Name \(CN\) / Subject Alternative Name \(SAN\)"**  
Validates that the client device closes the connection if it's presented with a server certificate for a domain name that is different than the one requested\.  
*API test case definition:*  
`EXECUTION_TIMEOUT` has a default value of 5 minutes\.

```
"tests":[
   {
      "name":"my_tls_incorrect_subject_name_cert_test"
      "configuration": {
         // optional:
         "EXECUTION_TIMEOUT":"300000",
      },
      "test":{
         "id":"TLS_Incorrect_Subject_Name_Server_Cert",
         "version":"0.0.0"
      }
   }
]
```
Test case outputs:  
Pass: The client device closed the connection\.  
Fail: The client device completed TLS handshake with AWS IoT\.

## MQTT<a name="device-advisor-tests-mqtt"></a>CONNECT, DISCONNECT, and RECONNECT

**"Device send CONNECT to AWS IoT Core \(Happy case\)"**  
Validates that the client device sends a CONNECT request\.   
*API test case definition:*  
`EXECUTION_TIMEOUT` has a default value of 5 minutes\.

```
"tests":[
   {
      "name":"my_mqtt_connect_test"
      "configuration": {
         // optional:
         "EXECUTION_TIMEOUT":"300000",
      },
      "test":{
         "id":"MQTT_Connect",
         "version":"0.0.0"
      }
   }
]
```

**"Device re\-connect with exponential backoff \- No CONNACK response"**  
Validates that the client device uses the proper exponential backoff when reconnecting with the server for at least five times\. The server logs the timestamp of the client device's CONNECT request, performs packet validation, pauses without sending a CONNACK to the client device, and waits for the client device to resend the request\. The collected timestamps are used to validate that exponential backoff is used by the client device\.   
*API test case definition:*  
`EXECUTION_TIMEOUT` has a default value of 10 minutes\.

```
"tests":[
   {
      "name":"my_mqtt_exponential_backoff_retries_test"
      "configuration": {
         // optional:
         "EXECUTION_TIMEOUT":"600000",
      },
      "test":{
         "id":"MQTT_Connect_Exponential_Backoff_Retries",
         "version":"0.0.0"
      }
   }
]
```Publish

**"QoS0 \(Happy Case\)"**  
Validates that the client device publishes a message with QoS0\. You can also validate the topic of the message by specifying this topic value in the test settings\.  
*API test case definition:*  
`EXECUTION_TIMEOUT` has a default value of 5 minutes\.

```
"tests":[
   {
      "name":"my_mqtt_publish_test":{
         // optional:
         "EXECUTION_TIMEOUT":"300000",
         "TOPIC_FOR_PUBLISH_VALIDATION": "String",
         "PAYLOAD_FOR_PUBLISH_VALIDATION": "String",
      },
      "test":{
         "id":"MQTT_Publish",
         "version":"0.0.0"
      }
   }
]
```

**"QoS1 publish retry \- No PUBACK"**  
Validates that the client device republishes a message sent with QoS1, if the server doesn't send PUBACK\. You can also validate the topic of the message by specifying this topic in the test settings\. The client device must not disconnect before republishing the message\. This test also validates that the republished message has the same packet identifier as the original\.  
*API test case definition:*  
`EXECUTION_TIMEOUT` has a default value of 5 minutes\.

```
"tests":[
   {
      "name":"my_mqtt_publish_retry_test":{
         // optional:
         "EXECUTION_TIMEOUT":"300000",
         "TOPIC_FOR_PUBLISH_VALIDATION": "String",
         "PAYLOAD_FOR_PUBLISH_VALIDATION": "String",
      },
      "test":{
         "id":"MQTT_Publish_Retry_No_Puback",
         "version":"0.0.0"
      }
   }
]
```Subscribe

**"Can Subscribe \(Happy Case\)"**  
Validates that the client device subscribes to MQTT topics\. You can also validate the topic that the client device subscribes to by specifying this topic in the test settings\.   
*API test case definition:*  
`EXECUTION_TIMEOUT` has a default value of 5 minutes\.

```
"tests":[
   {
      "name":"my_mqtt_subscribe_test":{
         // optional:
         "EXECUTION_TIMEOUT":"300000",
         "TOPIC_FOR_PUBLISH_VALIDATION":["String","String"]
      },
      "test":{
         "id":"MQTT_Subscribe",
         "version":"0.0.0"
      }
   }
]
```

**"Subscribe Retry \- No SUBACK"**  
Validates that the client device retries a failed subscription to MQTT topics\. The server then waits and doesn't send a SUBACK\. If the client device doesn't retry the subscription, the test fails\. You can also validate the topic that the client device subscribes to by specifying this topic in the test settings\.   
*API test case definition:*  
`EXECUTION_TIMEOUT` has a default value of 5 minutes\.

```
"tests":[
   {
      "name":"my_mqtt_subscribe_retry_test":{
         "EXECUTION_TIMEOUT":"300000",
         // optional:
         "TOPIC_FOR_PUBLISH_VALIDATION":["String","String"]
      },
      "test":{
         "id":"MQTT_Subscribe_Retry_No_Suback",
         "version":"0.0.0"
      }
   }
]
```

## Permissions and policies<a name="device-advisor-tests-permissions-policies"></a>

You use these tests to determine if the policies attached to your devices’ certificates follow the best practice\.

**"Device certificate attached policies don’t contain wildcards"**  
 Validates if the permission policies associated with a device follow best practices and do not grant the device more permissions than needed\.  
*API test case definition:*  
`EXECUTION_TIMEOUT` has a default value of 1 minute\.

```
"tests":[
   {
        "name":"my_security_device_policies"
        "configuration": {
            // optional:
            "EXECUTION_TIMEOUT":"60000"
        } 
        "test": {
            "id": "Security_Device_Policies",
            "version": "0.0.0"
        }
    }
]
```
These test cases are currently not available through the AWS IoT console\. Please use the AWS IoT CLI or the AWS IoT IoT Core Device Advisor SDK to configure this test case\.

## Shadow<a name="device-advisor-tests-shadow"></a>

Use these test to verify your devices use AWS IoT Device Shadow service correctly\. See [IoT device shadows](https://docs.aws.amazon.com/https://docs.aws.amazon.com/iot/latest/developerguide/iot-device-shadows.html) for more information\. If these test cases are configured in your test suite, then providing a thing is required when starting the suite run\.

**Note**  
These test cases are currently not available through the AWS console\. Please use the AWS CLI or the AWS IoT Core Device Advisor SDK to configure this test case\.Publish

***"Device publishes state after it connects \(Happy case\)"***  
Validates if a device can publish its state after it connects to AWS IoT Core  
*API test case definition:*  
`EXECUTION_TIMEOUT` has a default value of 5 minute\.

```
"tests":[
   {
      "name":"my_shadow_publish_reported_state",
      "configuration": {
         // optional:
         "EXECUTION_TIMEOUT":"300000",
         "SHADOW_NAME": "SHADOW_NAME",
         "REPORTED_STATE": {
            "STATE_ATTRIBUTE": "STATE_VALUE"
         }
      },
      "test":{
         "id":"Shadow_Publish_Reported_State",
         "version":"0.0.0"
      }
   }
]
```
The `REPORTED_STATE` can be provided for additional validation on your device's exact shadow state, after it connects\. By default, this test case validates your device publishing state\.  
If `SHADOW_NAME` is not provided, then the test case looks for messages published to topic prefixes of the Unnamed \(classic\) shadow type by default\. Provide a shadow name if your device uses the named shadow type\. See [Using shadows in devices](https://docs.aws.amazon.com/https://docs.aws.amazon.com/iot/latest/developerguide/device-shadow-comms-device.html) for more information\.Update

***"Device updates reported state to desired state \(Happy case\)"***  
Validates if your device reads all update messages received and synchronizes the device's state to match the desired state properties\. Your device should publish its latest reported state after synchronizing\.   
*API test case definition:*  
`EXECUTION_TIMEOUT` has a default value of 5 minutes\.

```
"tests":[
   {
      "name":"my_shadow_update_reported_state",
      "configuration": {
         "DESIRED_STATE": {
            "STATE_ATTRIBUTE": "STATE_VALUE"
         },
         // optional:
         "EXECUTION_TIMEOUT":"300000",
         "SHADOW_NAME": "SHADOW_NAME"
      },
      "test":{
         "id":"Shadow_Update_Reported_State",
         "version":"0.0.0"
      }
   }
]
```
The `DESIRED_STATE` should have at least one attribute and associated value\.  
If `SHADOW_NAME`is not provided, then the test case looks for messages published to topic prefixes of the Unnamed \(classic\) shadow type by default\. Provide a shadow name if your device uses the named shadow type\. See [Using shadows in devices](https://docs.aws.amazon.com/https://docs.aws.amazon.com/iot/latest/developerguide/device-shadow-comms-device.html) for more information\.