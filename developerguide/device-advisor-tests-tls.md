# TLS<a name="device-advisor-tests-tls"></a>

You use these tests to determine if the transport layer security protocol \(TLS\) between your devices and AWS IoT is secure\.

## Happy Path<a name="happy-path"></a>

**"TLS Connect"**  <a name="TLS_Connect"></a>
Validates if the device under test can complete TLS handshake to AWS IoT\. This test doesn't validate the MQTT implementation of the client device\.  
*API test case definition:*  
`EXECUTION_TIMEOUT` has a default value of 5 minutes\. For best results, we recommend a timeout value of 2 minutes\. 

```
"tests":[
   {
      "name":"my_tls_connect_test",
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

**Example Test case outputs:**  
+ **Pass** — The device under test completed TLS handshake with AWS IoT\.
+ **Pass with warnings** — The device under test completed TLS handshake with AWS IoT, but there were TLS warning messages from the device or AWS IoT\.
+ **Fail** — The device under test failed to complete TLS handshake with AWS IoT due to handshake error\.

**"TLS Receive Maximum Size Fragments"**  
This test case helps you validate if your device can receive and process TLS maximum size fragments\. Your test device needs to subscribe to a pre\-configured topic with QoS 1 to receive a large payload\. You can customize the payload using the configuration $\{payload\}\.  
*API test case definition:*  
`EXECUTION_TIMEOUT` has a default value of 5 minutes\. For best results, we recommend a timeout value of 2 minutes\. 

```
"tests":[
   {
      "name":"TLS Receive Maximum Size Fragments",
      "configuration": {
         // optional:
         "EXECUTION_TIMEOUT":"300",  //in seconds
         "PAYLOAD_FORMAT":"{"message":"${payload}"}", // A string with a placeholder ${payload}, or leave it empty to receive a plain string.
         "TRIGGER_TOPIC": "test_1" // A topic to which a device will subscribe, and to which a test case will publish a large payload.
      },
      "test":{
         "id":"TLS_Receive_Maximum_Size_Fragments",
         "version":"0.0.0"
      }
   }
]
```

## Cipher Suites<a name="cipher-suites"></a>

**"TLS Device Support for AWS IoT recommended Cipher Suites"**  
Validates that the cipher suites in the TLS Client Hello message from the device under test contains [AWS IoT recommended cipher suites](transport-security.md)\. It provides additional insights into cipher suites supported by the device\.  
*API test case definition:*  
`EXECUTION_TIMEOUT` has a default value of 5 minutes\. We recommend a timeout value of 2 minutes\.

```
"tests":[
   {
      "name":"my_tls_support_aws_iot_cipher_suites_test",
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

**Example Test case outputs:**  
+ **Pass** — The device under test cipher suites contain at least one AWS IoT recommended cipher suite and don't contain any unsupported cipher suites\.
+ **Pass with warnings** — The device cipher suites contain at least one AWS IoT cipher suite but 1\) don't contain any of the recommended cipher suites, or 2\) contain cipher suites not supported by AWS IoT\. We suggest verifying that unsupported cipher suites are safe\. 
+ **Fail** — The device under test cipher suites does not contain any of the AWS IoT supported cipher suites\.

## Larger Size Server Certificate<a name="larger-size"></a>

**"TLS large Size Server Certificate"**  
Validates if your device can complete the TLS handshake with AWS IoT by receiving and processing a larger size server certificate\. The size of the server certificate \(in bytes\), used by this test is larger than what is currently used in the **"TLS Connect"** test case and IoT Core by 20%\. During this test case, your device’s buffer space for TLS will be tested\. If the buffer space is large enough, the TLS handshake will complete without errors\. This test does not validate the MQTT implementation of the device\. The test case terminates after the TLS handshake process completes\.  
*API test case definition:*  
`EXECUTION_TIMEOUT` has a default value of 5 minutes\. For best results, we recommend a timeout value of 2 minutes\. If this test case fails but **"TLS Connect"** test case passes, we recommend you increase your device’s buffer space limit for TLS to a higher number of bytes\. Increasing the buffer space limit will ensure that your device is capable of processing a larger size server certificate in case the size increases\.

```
"tests":[
   {
      "name":"my_tls_large_size_server_cert_test",
      "configuration": {
         // optional:
         "EXECUTION_TIMEOUT":"300",  // in seconds
      },
      "test":{
         "id":"TLS_Large_Size_Server_Cert",
         "version":"0.0.0"
      }
   }
]
```

**Example Test case outputs:**  
+ **Pass** — The device under test completed the TLS handshake with AWS IoT\.
+ **Pass with warnings** — The device under test completed the TLS handshake with AWS IoT, but there are TLS warning messages either from the device or AWS IoT\.
+ **Fail** — The device under test failed to complete the TLS handshake with AWS IoT because of an error during the handshake process\.

## TLS Unsecure Server Cert<a name="unsecure-server"></a>

**"Not Signed By Recognized CA"**  <a name="TLS_Unsecure_Server_Cert"></a>
Validates that the device under test closes the connection if it's presented with a server certificate that doesn't have a valid signature from the ATS CA\. A device should only connect to an endpoint that presents a valid certificate\.  
*API test case definition:*  
`EXECUTION_TIMEOUT` has a default value of 5 minutes\. We recommend a timeout value of 2 minutes\. 

```
"tests":[
   {
      "name":"my_tls_unsecure_server_cert_test",
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

**Example Test case outputs:**  
+ **Pass** — The device under test closed the connection\.
+ **Fail** — The device under test completed TLS handshake with AWS IoT\.

**"TLS Incorrect Subject Name Server Cert / Incorrect Subject Common Name \(CN\) / Subject Alternative Name \(SAN\)"**  <a name="TLS_Incorrect_Subject_Name"></a>
Validates that the device under test closes the connection if it's presented with a server certificate for a domain name that is different than the one requested\.  
*API test case definition:*  
`EXECUTION_TIMEOUT` has a default value of 5 minutes\. We recommend a timeout value of 2 minutes\. 

```
"tests":[
   {
      "name":"my_tls_incorrect_subject_name_cert_test",
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

**Example Test case outputs:**  
+ **Pass** — The device under test closed the connection\.
+ **Fail** — The device under test completed TLS handshake with AWS IoT\.