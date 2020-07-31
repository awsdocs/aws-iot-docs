# Device agent integration with AWS IoT Greengrass<a name="device-defender-DetectMetricsGreengrassIntegration"></a>

AWS IoT Device Defender can be used with AWS IoT Greengrass\. Device agent integration follows the standard AWS IoT Greengrass Lambda function deployment model, allowing you to add AWS IoT Device Defender security to your AWS IoT Greengrass core devices\. Follow these steps to integrate a device agent\.

Prerequisites:
+ Set up your AWS IoT Greengrass environment\. See, [Environment setup for AWS IoT Greengrass](https://docs.aws.amazon.com/greengrass/latest/developerguide/module1.html)\.
+ Configure and run your AWS IoT Greengrass core\. See, [Installing the AWS IoT AWS IoT Greengrass Core software](https://docs.aws.amazon.com/greengrass/latest/developerguide/module2.html)\. 
+ Ensure you can successfully deploy and run a Lambda function on your AWS IoT Greengrass core\.

In general, the process described here follows the [Create and Package a Lambda Function](https://docs.aws.amazon.com/greengrass/latest/developerguide/create-lambda.html) section in the *AWS IoT Greengrass Developer Guide*\.

**To use AWS IoT Greengrass Connector**

The AWS IoT Device Defender AWS IoT Greengrass Connector provides the most streamlined and automated means of deploying the Device Defender agent to your AWS IoT Greengrass core, and is the recommended method of using Device Defender with AWS IoT Greengrass\.

For more information about using AWS IoT Greengrass Connectors, see [Getting started with AWS IoT Greengrass connectors](https://docs.aws.amazon.com/greengrass/latest/developerguide/connectors-console.html)\. For more information about configuring the Device Defender Connector, see [ Device Defender connector](https://docs.aws.amazon.com/greengrass/latest/developerguide/device-defender-connector.html)\.

1. Create a local resource to allow your Lambda to collect metrics from the AWS IoT Greengrass Core host by following [Access local resources with Lambda functions and connectors](https://docs.aws.amazon.com/greengrass/latest/developerguide/access-local-resources.html)\. Use the following paramaters:
   + Resource Name: `Core_Proc`
   + Type: `Volume`
   + Source Path: `/proc`
   + Destination Path: `/host_proc` \(make sure the same value is configured for the `PROCFS_PATH` environment variable above\)
   + Group owner file access permission: select "Automatically add OS group permissions of the Linux group that owns the resource"
   + Associate the resource with your metrics Lambda\.

1. From the detail page of your AWS IoT Greengrass Group, select **Connectors** in the left\-hand menu\.

1. Select **Add a Connector**\.

1. In the **Select a connector** screen, select the **Device Defender** connector from the list and choose **Next**\.

1. On the **Configure parameters** screen, select the resource you created in Step 1 in the **Resource for `/proc`** box\.

1. In the **Metrics reporting interval** box, enter 300 or larger if you want to use a longer reporting interval\.

1. Select **Add**\.

**To create a Lambda package**
**Note**  
Due to platform\-specific binary extensions in the psutil package, this process should be performed on the platform where you plan to deploy your Lambda\.

1. Clone the AWS IoT Device Defender Python samples repository\.

   ```
   git clone https://github.com/aws-samples/aws-iot-device-defender-agent-sdk-python.git
   ```

1. Create and activate a virtual environment \(optional, but recommended\)\.

   ```
   pip install virtualenv
   virtualenv metrics_lambda_environment
   source metrics_lambda_environment/bin/activate
   ```

1. Install the AWS IoT Device Defender sample agent in the virtual environment\. Install from PyPi\.

   ```
   pip install AWSIoTDeviceDefenderAgentSDK
   ```

1. Install the downloaded source\.

   ```
   cd aws-iot-device-defender-agent-sdk-python
   #This must be run from the same directory as setup.py
   pip install .
   ```

1. Create an empty directory to assemble your Lambda function\. This is your Lambda directory\.

   ```
   mkdir metrics_lambda
   cd metrics_lambda
   ```

1. Complete steps 1\-4 in [Create and Package a Lambda Function](https://docs.aws.amazon.com/greengrass/latest/developerguide/create-lambda.html)\.

1. Unzip the AWS IoT Greengrass Python SDK into your Lambda directory\. 

   ```
   unzip ../aws_greengrass_core_sdk/sdk/python_sdk_1_1_0.zip
   cp -R ../aws_greengrass_core_sdk/examples/HelloWorld/greengrass_common .
   cp -R ../aws_greengrass_core_sdk/examples/HelloWorld/greengrasssdk .
   cp -R ../aws_greengrass_core_sdk/examples/HelloWorld/greengrass_ipc_python_sdk .
   ```

1. Copy the `AWSIoTDeviceDefenderAgentSDK` module to the root level of your Lambda directory\.

   ```
   cp -R ../aws-iot-device-defender-agent-sdk-python/AWSIoTDeviceDefenderAgentSDK . 
   ```

1. Copy the AWS IoT Greengrass agent to the root level of your Lambda directory\. 

   ```
   cp ../aws-iot-device-defender-agent-sdk-python/samples/greengrass/greengrass_core_metrics_agent/greengrass_defender_agent.py . 
   ```

1. Customize the AWS IoT Greengrass agent to include the name of your AWS IoT Greengrass core device and the desired metrics sample rate:
   + Replace `GREENGRASS_CORENAME` with the name of your AWS IoT Greengrass core\.
   + Set the `SAMPLE_RATE_SECONDS` to your desired metrics reporting interval\. The shortest reporting interval supported by AWS IoT Device Defender is 5 minutes \(300 seconds\)\.

1. Copy the dependencies from your virtual environment \(or your system\) into the root level of your Lambda directory\. 

   ```
   cp -R ../metrics_lambda_environment/lib/python2.7/site-packages/psutil .
   cp -R ../metrics_lambda_environment/lib/python2.7/site-packages/cbor .
   ```

1. Create your Lambda function zip file\. Perform this command in the root level of your Lambda directory\.

   ```
   rm *.zip
   zip -r greengrass_defender_metrics_lambda.zip *
   ```

**To configure and deploy your AWS IoT AWS IoT Greengrass Lambda function**

1. [Upload your lambda zip file](https://docs.aws.amazon.com/greengrass/latest/developerguide/package.html)\. 

1. Select the Python 3\.7 runtime, and in the Handler field, enter `greengrass_defender_agent.function_handler`\.

1. [Configure your Lambda function as a long\-lived Lambda function](https://docs.aws.amazon.com/greengrass/latest/developerguide/long-lived.html)\.

1. [Configure a subscription from your Lambda function to the AWS IoT cloud](https://docs.aws.amazon.com/greengrass/latest/developerguide/config-subs.html)\. For AWS IoT Device Defender, a subscription from the AWS Cloud to your Lambda function is not required\.

1. Create a local resource to allow your Lambda function to collect metrics from your AWS IoT Greengrass core host: 
   + Follow the instructions in [Access Local Resources with Lambda Functions](https://docs.aws.amazon.com/greengrass/latest/developerguide/access-local-resources.html)\. Use the following parameters:
     + Resource Name: `Core Proc`
     + Type: `Volume`
     + Source Path: `/proc`
     + Destination Path: `/host_proc`
     + Group owner file access permission: "Automatically add OS group permissions of the Linux group that owns the resource"
     + Associate the resource with your metrics Lambda function\.

1. [Deploy cloud configurations to a AWS IoT Greengrass core device](https://docs.aws.amazon.com/greengrass/latest/developerguide/configs-core.html)\.

**To review your AWS IoT Device Defender device metrics using the AWS IoT console**

1. Temporarily modify the publish topic in your AWS IoT Greengrass Lambda function to "metrics/test"\. 

1. Deploy the Lambda function\. 

1. To see the metrics your AWS IoT Greengrass core is emitting, on the **Test** page of the AWS IoT console, add a subscription to the temporary topic \("metrics/test"\) \. 