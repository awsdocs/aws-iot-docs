# Programming devices to work with jobs<a name="programming-devices"></a>

The examples in this section use MQTT to illustrate how a device works with the AWS IoT Jobs service\. Alternatively, you could use the corresponding API or CLI commands\. For these examples, we assume a device called `MyThing` that subscribes to the following MQTT topics:
+ `$aws/things/MyThing/jobs/notify` \(or `$aws/things/MyThing/jobs/notify-next`\)
+ `$aws/things/MyThing/jobs/get/accepted`
+ `$aws/things/MyThing/jobs/get/rejected`
+ `$aws/things/MyThing/jobs/jobId/get/accepted`
+ `$aws/things/MyThing/jobs/jobId/get/rejected`

 If you are using code signing for AWS IoT your device code must verify the signature of your code file\. The signature is in the job document in the `codesign` property\. For more information about verifying a code file signature, see [Device Agent Sample](https://github.com/aws/aws-iot-device-sdk-js#jobsAgent)\.

**Topics**
+ [Device workflow](jobs-workflow-device-online.md)
+ [Jobs workflow](jobs-workflow-jobs-online.md)
+ [Jobs notifications](jobs-comm-notifications.md)