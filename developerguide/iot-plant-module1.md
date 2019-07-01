# Module 1: Setting Up AWS IoT and Sending Data with Your Development Computer<a name="iot-plant-module1"></a>

In the first part of this walkthrough \(Steps 1–5\), you set up AWS IoT to begin receiving and storing soil moisture readings coming from your development computer as a device simulator, or from a Raspberry Pi\. You then set up AWS IoT to send email alerts through Amazon Simple Notification Service \(Amazon SNS\) that are based on those readings\.

Finally, you use your development computer to simulate soil moisture readings by generating random data\. You then push those readings into AWS IoT \. When the readings get too low, Amazon SNS automatically sends an email alert\. 

In [Module 2](iot-plant-module2.md), you can generate real soil moisture readings with a Raspberry Pi and then push those real readings into AWS IoT \.

## Prerequisites for Steps 1–5<a name="iot-plant-step1-prereqs"></a>

To complete the first five steps of this tutorial, you need the following:
+ An AWS account
+ An IAM administrator user in the AWS account\. \(You can use the AWS account root user instead of an IAM administrator user\. However, we don’t recommend it\.\)
+ A desktop or laptop development computer to work with the [ AWS IoT console](https://console.aws.amazon.com/iot/home) from a web browser, and to push simulated soil moisture readings into AWS IoT \. This computer can be running a Windows, macOS, Linux, or Unix operating system\. \(This sample was tested with a laptop computer running Windows 10 Enterprise edition\.\)