# Shadow<a name="device-advisor-tests-shadow"></a>

Use these tests to verify your devices under test use AWS IoT Device Shadow service correctly\. See [AWS IoT Device Shadow service](iot-device-shadows.md) for more information\. If these test cases are configured in your test suite, then providing a thing is required when starting the suite run\.

## Publish<a name="publish"></a>

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
If `SHADOW_NAME` is not provided, the test case looks for messages published to topic prefixes of the Unnamed \(classic\) shadow type by default\. Provide a shadow name if your device uses the named shadow type\. See [Using shadows in devices](https://docs.aws.amazon.com/iot/latest/developerguide/device-shadow-comms-device.html) for more information\.

## Update<a name="update"></a>

***"Device updates reported state to desired state \(Happy case\)"***  
Validates if your device reads all update messages received and synchronizes the device's state to match the desired state properties\. Your device should publish its latest reported state after synchronizing\. If your device already has an existing shadow before running the test, make sure the desired state configured for the test case and the existing reported state do not already match\. You can identify Shadow update messages sent by Device Advisor by looking at the **ClientToken** field in the Shadow document as it will be `DeviceAdvisorShadowTestCaseSetup`\.   
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
If `SHADOW_NAME` is not provided, then the test case looks for messages published to topic prefixes of the Unnamed \(classic\) shadow type by default\. Provide a shadow name if your device uses the named shadow type\. See [Using shadows in devices](https://docs.aws.amazon.com/iot/latest/developerguide/device-shadow-comms-device.html) for more information\.