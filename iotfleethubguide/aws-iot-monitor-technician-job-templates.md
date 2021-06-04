# Working with jobs in Fleet Hub for AWS IoT Device Management<a name="aws-iot-monitor-technician-job-templates"></a>

**Note**  
The job templates feature is in preview and subject to change\.

A job is a remote operation that is sent to and run on one or more devices connected to AWS IoT\. For example, you can define a job that instructs a set of devices to download and install application or firmware updates, reboot, rotate certificates, or perform remote troubleshooting operations\. You can run preconfigured jobs from Fleet Hub for AWS IoT Device Management web applications\. Your organization's administrators create job templates in the AWS IoT console and attach policies that make the templates available to Fleet Hub users\. In your Fleet Hub application, you specify the devices or a device group on which the job runs\.

Administrators also create device groups that you can view in your application\. To see these groups, choose **Device groups** in the navigation pane\. When you specify a device group as a target, you can specify one of the following two types of options for how the job runs\.
+ snapshot: The job runs once\.
+ continuous: After its initial run, the job runs on any device that is added to the group\.

For more information about creating and managing job templates, see [Job templates](https://docs.aws.amazon.com/iot/latest/developerguide/job-templates.html)\. For more information about how jobs work, see [Jobs](https://docs.aws.amazon.com/iot/latest/developerguide/iot-jobs.html)\.

## Running jobs<a name="aws-iot-monitor-technician-job-templates-run"></a>

You can run a job from several locations in a Fleet Hub application, but the following steps are always the same\.

1. Select a group or one or more devices as the target\.

1. Choose **Run job**\.

1. Under **Job target selection**, choose either **continuous** or **snapshot**\.

1. Select a job template\. Verify that the text under **Job summary** describes the type of job that you want to run\.

1. Optionally, enter a name for the job\.

1. Choose **Run**\.

You can select targets and follow these steps from the following locations in your Fleet Hub application\.
+ The device list tab on the dashboard\.
+ The details page of a specific device\.
+ Device groups page\.
+ The details page of a specific device group\.

## Viewing and managing jobs<a name="aws-iot-monitor-technician-job-templates-view"></a>

You can see jobs that are running in your fleet in the following locations\.
+ The jobs list page \-\- This page displays all of the jobs running in your fleet\. To see this page, choose **Jobs** in the navigation pane\.
+ The details page for a specific device \-\- This page displays all of the jobs running on the device\.

Each job has a details page that displays summary information about the job, including target and runtime information\. This page shows the runtime status of the job on each device\. It also displays the following totals\.
+ Number of runs\.
+ Number of canceled runs\.
+ Number of successful runs\.
+ Number of failed runs\.
+ Number of rejected runs\.
+ Number of queued runs\.
+ Number of in progress runs\.
+ Number of removed runs\.
+ Number of timed out runs\.

To cancel a job, select the job and choose **Cancel**\.