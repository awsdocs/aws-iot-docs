# Device Advisor test cases<a name="device-advisor-tests"></a>

Device Advisor provides prebuilt tests in five categories\.
+ [TLS](device-advisor-tests-tls.md)
+ [MQTT](device-advisor-tests-mqtt.md)
+ [Shadow](device-advisor-tests-shadow.md)
+ [Job execution](device-advisor-tests-job-execution.md)
+ [Permissions and policies](device-advisor-tests-permissions-policies.md)

## Device Advisor test cases to qualify for the AWS Device Qualification Program\.<a name="qualifiying-test-cases"></a>

Your device needs to pass the following tests to qualify as per [AWS Device Qualification Program](https://aws.amazon.com/partners/programs/dqp/)\.
+ [TLS Incorrect Subject Name Server Cert](device-advisor-tests-tls.md#TLS_Incorrect_Subject_Name) \("Incorrect Subject Common Name \(CN\) / Subject Alternative Name \(SAN\)"\)
+ [TLS Unsecure Server Cert](device-advisor-tests-tls.md#TLS_Unsecure_Server_Cert) \("Not Signed By Recognized CA"\)​
+ [TLS Connect](device-advisor-tests-tls.md#TLS_Connect) \("TLS Connect"\)​
+ [MQTT Connect](device-advisor-tests-mqtt.md#MQTT_Connect) \("Device send CONNECT to AWS IoT Core \(Happy case\)"\)​
+ [MQTT Subscribe](device-advisor-tests-mqtt.md#MQTT_Subscribe) \("Can Subscribe \(Happy Case\)"\)​
+ [MQTT Publish](device-advisor-tests-mqtt.md#MQTT_Publish) \("QoS0 \(Happy Case\)"\)​