# An example use case in FreeRTOS OTA<a name="streaming-service-example"></a>

The FreeRTOS OTA \(over\-the\-air\) agent uses the AWS IoT Streaming service to transfer FreeRTOS firmware images to FreeRTOS devices\. To send the initial data set to a device, it uses the AWS IoT Job service to schedule an OTA update job to FreeRTOS devices\.

For a reference implementation of a Streaming service client, see [FreeRTOS OTA agent codes](https://github.com/aws/amazon-freertos/tree/master/libraries/freertos_plus/aws/ota/src) in the FreeRTOS GitHub website\.