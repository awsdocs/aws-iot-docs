# Creating and managing jobs \(console\)<a name="manage-job-console"></a>

**To create a job**

1. Browse to the [AWS IoT console](https://console.aws.amazon.com/iot/)\.

1. In the navigation pane, choose **Manage**, and then choose **Jobs**\.

1. Choose **Create a job**\.

1. Choose **Create a custom job**\.

1. Enter an alphanumeric ID for your job and an optional description\.
**Note**  
We do not recommend using personally identifiable information in your job IDs or descriptions\.

1. Select the device or device groups that you want to update\.

1. Under **Add a job file**, choose **Select**, and then select your job document\.

1. Under **Job type**, choose the appropriate option for your update, and then choose **Next**\.

1. Specify values for any advanced configurations, and then choose **Create**\.

After you create the job, the console generates a JSON signature and places it in your job document\.

You can use the [AWS IoT console](https://console.aws.amazon.com/iot/) to view the status, cancel, or delete a job\.

1. Browse to the [AWS IoT console](https://console.aws.amazon.com/iot/)\.

1. In the navigation pane, choose **Manage**, and then choose **Jobs**\.