# Device Agent Integration with AWS IoT Greengrass<a name="device-defender-DetectMetricsGreengrassIntegration"></a>

 can be used in conjunction with AWS Greengrass\. Device agent integration follows the standard Greengrass Lambda function deployment model, allowing you to add security to your Greengrass Core devices\. To integrate a device agent, follow the steps outlined in this section\.

Prerequisites:

+ Have your Greengrass environment set up\.

+ Have your Greengrass core configured and running\.

+ Ensure you can successfully deploy and run a Lambda function on your Greengrass core\.

In general, the process described here follows the [ Create and Package a Lambda Function](https://docs.aws.amazon.com/greengrass/latest/developerguide/create-lambda.html) section of the AWS Greengrass Developers Guide\.

**Create A Lambda Package**

1. Clone the Python Samples Repository\.

   ```
   git clone https://github.com/aws-samples/aws-iot-device-defender-agent-sdk-python.git
   ```

1. Create and activate a virtual environment \(optional but recommended\)\.

   ```
   pip install virtualenv
   virtualenv metrics_lambda_environment
   source metrics_lambda_environment/bin/activate
   ```

1. Install the sample agent in the virtual environment Install from PyPi\.

   ```
   pip install AWSIoTDeviceDefenderAgentSDK
   ```

1. Install the downloaded source\.

   ```
   cd aws-iot-device-defender-agent-sdk-python
   #This must be run from the same directory as setup.py
   pip install .
   ```

1. Create an empty directory to assemble your Lambda function, we will refer to this as your “Lambda directory”\.

   ```
   mkdir metrics_lambda
   cd metrics_lambda
   ```

1. Complete steps 1\-4 from this section: [ Create and Package a Lambda Function](https://docs.aws.amazon.com/greengrass/latest/developerguide/create-lambda.html)\.

1. Unzip the Greengrass python sdk into your Lambda directory\. 

   ```
   unzip ../aws_greengrass_core_sdk/sdk/python_sdk_1_1_0.zip
   cp -R ../aws_greengrass_core_sdk/examples/HelloWorld/greengrass_common .
   cp -R ../aws_greengrass_core_sdk/examples/HelloWorld/greengrasssdk .
   cp -R ../aws_greengrass_core_sdk/examples/HelloWorld/greengrass_ipc_python_sdk .
   ```

1. Copy the AWSIoTDeviceDefenderAgentSDK module to the root level of your Lambda directory\.

   ```
   cp -R ../aws-iot-device-defender-agent-sdk-python/AWSIoTDeviceDefenderAgentSDK . 
   ```

1. Copy the Greengrass agent to the root level of your Lambda directory\. 

   ```
   cp ../aws-iot-device-defender-agent-sdk-python/samples/greengrass/greengrass_core_metrics_agent/greengrass_defender_agent.py . 
   ```

1. Customize the Greengrass agent to include the name of your Greengrass Core device, and the desired metrics sample rate:

   + Replace `GREENGRASS_CORENAME` with the name of your Greengrass Core\.

   + Set the `SAMPLE_RATE_SECONDS` to your desired metrics reporting interval\. \(Note: 5 minutes \(300 seconds\) is the shortest reporting interval supported by \.\)

1. Copy the dependencies from your virtual environment \(or your system\) into the the root level of your Lambda directory\. 

   ```
   cp -R ../metrics_lambda_environment/lib/python2.7/site-packages/psutil .
   cp -R ../metrics_lambda_environment/lib/python2.7/site-packages/cbor .
   ```

1. Create your Lambda function zipfile\. \(Note: you should perform this command in the root level of your Lambda directory\.\)

   ```
   rm *.zip
   zip -r greengrass_defender_metrics_lambda.zip *
   ```

**Configure and deploy your Greengrass Lambda function**

1. [ Upload your lambda zip file](https://docs.aws.amazon.com/greengrass/latest/developerguide/package.html)\. 

1. Select the Python 2\.7 runtime, and enter `greengrass_defender_agent.function_handler` in the Handler field\.

1. [ Configure your lambda as a long\-lived lambda](https://docs.aws.amazon.com/greengrass/latest/developerguide/long-lived.html)\.

1. [ Configure a subscription from your Lambda function to the AWS IoT Cloud](https://docs.aws.amazon.com/greengrass/latest/developerguide/config-subs.html)\. \(Note: For , a subscription from the AWS IoT cloud to your Lambda function is not required\.\)

1. Create a local resource to allow your Lambda function to collect metrics from your Greengrass Core host: 

   + Follow the instructions [here](https://docs.aws.amazon.com/greengrass/latest/developerguide/access-local-resources.html) using the following parameters:

     + Resource Name: `Core Proc`

     + Type: `Volume`

     + Source Path: `/proc`

     + Destination Path: `/host_proc`

     + Group owner file access permission: “Automatically add OS group permissions of the Linux group that owns the resource”

     + Associate the resource with your metrics Lambda function\.

1. Deploy your Lambda function to your Greengrass Group\. 

**Review your device metrics using the AWS IoT Console**

1. Temporarily modify the publish topic in your Greengrass Lambda function to "metrics/test"\. 

1. Deploy the Lambda function\. 

1. Add a subscription to the temporary topic \("metrics/test"\) in the "Test" section of the IoT console in order to see the metrics your Greengrass Core is emitting\. 