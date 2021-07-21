# Device Advisor test cases<a name="device-advisor-tests"></a>

Device Advisor provides prebuilt tests in five categories\.
+ **TLS**
+ **Permissions and policies**
+ **MQTT**
+ **Shadow**
+ **Job Execution**

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
Validates if the device under test can complete TLS handshake to AWS IoT\. This test doesn't validate the MQTT implementation of the client device\.  
*API test case definition:*  
`EXECUTION_TIMEOUT` has a default value of 5 minutes\. We recommended timeout value of 2 minutes\. 

```
"tests":[
   {
      "name":"my_tls_connect_test"
      "configuration": {
         // optional:
         "EXECUTION_TIMEOUT":"300",  //in seconds
      },
      "test":{
         "id":"TLS_Connect",
         "version":"0.0.0"
      }
   }
]
```
Test case outputs:  
Pass: The device under test completed TLS handshake with AWS IoT\.  
Pass with warnings: The device under test completed TLS handshake with AWS IoT, but there were TLS warning messages from the device or AWS IoT\.  
Fail: The device under test failed to complete TLS handshake with AWS IoT due to handshake error\.

**"TLS Device Support for AWS IoT recommended Cipher Suites"**  
Validates that the cipher suites in the TLS Client Hello message from the device under test contains [AWS IoT recommended cipher suites](transport-security.md)\. It provides additional insights into cipher suites supported by the device\.  
*API test case definition:*  
`EXECUTION_TIMEOUT` has a default value of 5 minutes\. We recommend a timeout value of 2 minutes\.

```
"tests":[
   {
      "name":"my_tls_support_aws_iot_cipher_suites_test"
      "configuration": {
         // optional:
         "EXECUTION_TIMEOUT":"300",  // in seconds
      },
      "test":{
         "id":"TLS_Support_AWS_IoT_Cipher_Suites",
         "version":"0.0.0"
      }
   }
]
```
Test case outputs:  
Pass: The device under test cipher suites contain at least one AWS IoT recommended cipher suite and don't contain any unsupported cipher suites\.  
Pass with warnings: The device cipher suites contain at least one AWS IoT cipher suite but 1\) don't contain any of the recommended cipher suites, or 2\) contain cipher suites not supported by AWS IoT\. We suggest verifying that unsupported cipher suites are safe\.   
Fail: The device under test cipher suites don't contain any of the AWS IoT supported cipher suites\.Bad server certificate

**"Not Signed By Recognized CA"**  
Validates that the device under test closes the connection if it's presented with a server certificate that doesn't have a valid signature from the ATS CA\. A device should only connect to an endpoint that presents a valid certificate\.  
*API test case definition:*  
`EXECUTION_TIMEOUT` has a default value of 5 minutes\. We recommend a timeout value of 2 minutes\. 

```
"tests":[
   {
      "name":"my_tls_unsecure_server_cert_test"
      "configuration": {
         // optional:
         "EXECUTION_TIMEOUT":"300",  //in seconds
      },
      "test":{
         "id":"TLS_Unsecure_Server_Cert",
         "version":"0.0.0"
      }
   }
]
```
Test case outputs:  
Pass: The device under test closed the connection\.  
Fail: The device under test completed TLS handshake with AWS IoT\.

**"Incorrect Subject Common Name \(CN\) / Subject Alternative Name \(SAN\)"**  
Validates that the device under test closes the connection if it's presented with a server certificate for a domain name that is different than the one requested\.  
*API test case definition:*  
`EXECUTION_TIMEOUT` has a default value of 5 minutes\. We recommend a timeout value of 2 minutes\. 

```
"tests":[
   {
      "name":"my_tls_incorrect_subject_name_cert_test"
      "configuration": {
         // optional:
         "EXECUTION_TIMEOUT":"300",   // in seconds
      },
      "test":{
         "id":"TLS_Incorrect_Subject_Name_Server_Cert",
         "version":"0.0.0"
      }
   }
]
```
Test case outputs:  
Pass: The device under test closed the connection\.  
Fail: The device under test completed TLS handshake with AWS IoT\.

## Permissions and policies<a name="device-advisor-tests-permissions-policies"></a>

You use these tests to determine if the policies attached to your devices’ certificates follow the best practice\.

**"Device certificate attached policies don’t contain wildcards"**  
 Validates if the permission policies associated with a device follow best practices and do not grant the device more permissions than needed\.  
*API test case definition:*  
`EXECUTION_TIMEOUT` has a default value of 1 minute\. It is recommended to set timeout for at least 30 seconds\.

```
"tests":[
   {
        "name":"my_security_device_policies"
        "configuration": {
            // optional:
            "EXECUTION_TIMEOUT":"60"    // in seconds
        } 
        "test": {
            "id": "Security_Device_Policies",
            "version": "0.0.0"
        }
    }
]
```

## MQTT<a name="device-advisor-tests-mqtt"></a>CONNECT, DISCONNECT, and RECONNECT

**"Device send CONNECT to AWS IoT Core \(Happy case\)"**  
Validates that the device under test sends a CONNECT request\.   
*API test case definition:*  
`EXECUTION_TIMEOUT` has a default value of 5 minutes\. We recommend a timeout value of 2 minutes\. 

```
"tests":[
   {
      "name":"my_mqtt_connect_test"
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

**"Device re\-connect with jitter backoff \- No CONNACK response"**  
Validates that the device under test uses the proper jitter backoff when reconnecting with the broker for at least five times\. The broker logs the timestamp of the device under test's CONNECT request, performs packet validation, pauses without sending a CONNACK to the device under test, and waits for the device under test to resend the request\. The sixth connection attempt is allowed to pass through and CONNACK is allowed to flow back to the device under test\.  
The above process is performed again\. In total, this test case requires the device to connect at least 12 times in total\. The collected timestamps are used to validate that jitter backoff is used by the device under test\. If the device under test has strictly exponential backoff delay, this test case will pass with warnings\.   
*API test case definition:*  
`EXECUTION_TIMEOUT` has a default value of 10 minutes\. We recommend a timeout value of 4 minutes\. 

```
"tests":[
   {
      "name":"my_mqtt_jitter_backoff_retries_test"
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

**"Device can return PUBACK to an arbitrary topic for QoS1"**  
This test case will check if device\(client\) can return PUBACK message if it received publish message from broker after subscribing to a topic with QoS1\.  
*API test case definition:*  
`EXECUTION_TIMEOUT` has a default value of 5 minutes\. We recommend a timeout value of 2 minutes\. 

```
"tests":[
   {
        "name":"my_mqtt_client_puback_qos1"
        "configuration": {
            // optional:
            "TRIGGER_TOPIC": "myTopic",
            "EXECUTION_TIMEOUT":"300", // in seconds
            "PAYLOAD_FOR_PUBLISH_VALIDATION":"custom payload"
        } 
        "test": {
            "id": "MQTT_Client_Puback_Qos1",
            "version": "0.0.0"
        }
    }
]
```

**"Device re\-connect with exponential backoff \- No CONNACK response"**  
Validates that the device under test uses the proper exponential backoff when reconnecting with the broker for at least five times\. The broker logs the timestamp of the device under test's CONNECT request, performs packet validation, pauses without sending a CONNACK to the client device, and waits for the device under test to resend the request\. The collected timestamps are used to validate that exponential backoff is used by the device under test\.   
*API test case definition:*  
`EXECUTION_TIMEOUT` has a default value of 10 minutes\. We recommend a timeout value of 4 minutes\. 

```
"tests":[
   {
      "name":"my_mqtt_exponential_backoff_retries_test"
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
```Publish

**"QoS0 \(Happy Case\)"**  
Validates that the device under test publishes a message with QoS0\. You can also validate the topic of the message by specifying this topic value in the test settings\.  
*API test case definition:*  
`EXECUTION_TIMEOUT` has a default value of 5 minutes\. We recommend a timeout value of 2 minutes\. 

```
"tests":[
   {
      "name":"my_mqtt_publish_test":{
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
      "name":"my_mqtt_publish_retry_test":{
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
```Subscribe

**"Can Subscribe \(Happy Case\)"**  
Validates that the device under test subscribes to MQTT topics\. You can also validate the topic that the device under test subscribes to by specifying this topic in the test settings\.   
*API test case definition:*  
`EXECUTION_TIMEOUT` has a default value of 5 minutes\. We recommend a timeout value of 2 minutes\. 

```
"tests":[
   {
      "name":"my_mqtt_subscribe_test":{
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
Validates that the device under test retries a failed subscription to MQTT topics\. The server then waits and doesn't send a SUBACK\. If the client device doesn't retry the subscription, the test fails\. You can also validate the topic that the device under test subscribes to by specifying this topic in the test settings\.   
*API test case definition:*  
`EXECUTION_TIMEOUT` has a default value of 5 minutes\. We recommend a timeout value of 4 minutes\. 

```
"tests":[
   {
      "name":"my_mqtt_subscribe_retry_test":{
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

## Shadow<a name="device-advisor-tests-shadow"></a>

Use these test to verify your devices under test use AWS IoT Device Shadow service correctly\. See [AWS IoT Device Shadow service](iot-device-shadows.md) for more information\. If these test cases are configured in your test suite, then providing a thing is required when starting the suite run\.Publish

***"Device publishes state after it connects \(Happy case\)"***  
Validates if a device can publish its state after it connects to AWS IoT Core  
*API test case definition:*  
`EXECUTION_TIMEOUT` has a default value of 5 minutes\. We recommend a timeout value of 2 minutes\. 

```
"tests":[
   {
      "name":"my_shadow_publish_reported_state",
      "configuration": {
         // optional:
         "EXECUTION_TIMEOUT":"300", // in seconds
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
<<<<<<< HEAD
If `SHADOW_NAME` is not provided, then the test case looks for messages published to topic prefixes of the Unnamed \(classic\) shadow type by default\. Provide a shadow name if your device uses the named shadow type\. See [Using shadows in devices](https://docs.aws.amazon.com/https://docs.aws.amazon.com/iot/latest/developerguide/device-shadow-comms-device.html) for more information\.Update
=======
If `SHADOW_NAME` is not provided, then the test case looks for messages published to topic prefixes of the Unnamed \(classic\) shadow type by default\. Provide a shadow name if your device uses the named shadow type\. See [Using shadows in devices](https://docs.aws.amazon.com/iot/latest/developerguide/device-shadow-comms-device.html) for more information\.Update
>>>>>>> gausekha-repo-refresh

***"Device updates reported state to desired state \(Happy case\)"***  
Validates if your device reads all update messages received and synchronizes the device's state to match the desired state properties\. Your device should publish its latest reported state after synchronizing\. If your device already has an existing shadow before running the test, make sure the desired state configured for the test case and the existing reported state do not already match\. You can identify Shadow update messages sent by Device Advisor by looking at the ClientToken field in the Shadow document as it will be `DeviceAdvisorShadowTestCaseSetup`\.   
*API test case definition:*  
`EXECUTION_TIMEOUT` has a default value of 5 minutes\. We recommend a timeout value of 2 minutes\. 

```
"tests":[
   {
      "name":"my_shadow_update_reported_state",
      "configuration": {
         "DESIRED_STATE": {
            "STATE_ATTRIBUTE": "STATE_VALUE"
         },
         // optional:
         "EXECUTION_TIMEOUT":"300", // in seconds
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
<<<<<<< HEAD
If `SHADOW_NAME` is not provided, then the test case looks for messages published to topic prefixes of the Unnamed \(classic\) shadow type by default\. Provide a shadow name if your device uses the named shadow type\. See [Using shadows in devices](https://docs.aws.amazon.com/https://docs.aws.amazon.com/iot/latest/developerguide/device-shadow-comms-device.html) for more information\.
=======
If `SHADOW_NAME` is not provided, then the test case looks for messages published to topic prefixes of the Unnamed \(classic\) shadow type by default\. Provide a shadow name if your device uses the named shadow type\. See [Using shadows in devices](https://docs.aws.amazon.com/iot/latest/developerguide/device-shadow-comms-device.html) for more information\.
>>>>>>> gausekha-repo-refresh

## Job Execution<a name="device-advisor-test-job-execution"></a>

**"Device can complete a job execution"**  
This test case helps you validate if your device is able to receive updates using AWS IoT Jobs, and publish the status of successful update\. For more information on AWS IoT Jobs, see [ Jobs](https://docs.aws.amazon.com/iot/latest/developerguide/iot-jobs.html)\.  
*API test case definition:*  
`EXECUTION_TIMEOUT` has a default value of 5 minutes\. We recommend a timeout value of 2 minutes\. 

```
"tests":[
   {
        "name":"my_job_execution",
        "configuration": {
            // Document is a JSON formatted string
            "JOB_DOCUMENT": "{
                "operation":"reboot",
                "files" : {
                   "fileName" : "install.py",
                   "url" : "${aws:iot:s3-presigned-url:https://s3.amazonaws.com/bucket/key}"
            }",
            
            // Document_SOURCE is an S3 link to the job document
            // Document and document are optional but can't be both null.
            "JOB_DOCUMENT_SOURCE": "https://s3.amazonaws.com/bucket/key",
            // optional:
            "EXECUTION_TIMEOUT": "300"  // in seconds
        },
        "test": {
            "id": "Job_Execution",
            "version": "0.0.0"
        }
    }
]
```
For more information on creating and using job documents see [job document](https://docs.aws.amazon.com/iot/latest/developerguide/iot-jobs.html)\. 