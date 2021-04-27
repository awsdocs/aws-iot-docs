# An example use case in FreeRTOS OTA<a name="mqtt-based-file-delivery-example"></a>

The FreeRTOS OTA \(over\-the\-air\) agent uses AWS IoT MQTT\-based file delivery to transfer FreeRTOS firmware images to FreeRTOS devices\. To send the initial data set to a device, it uses the AWS IoT Job service to schedule an OTA update job to FreeRTOS devices\.

For a reference implementation of an MQTT\-based file delivery client, see [FreeRTOS OTA agent codes](https://github.com/aws/amazon-freertos/tree/master/libraries/freertos_plus/aws/ota/src) in the FreeRTOS GitHub website\.