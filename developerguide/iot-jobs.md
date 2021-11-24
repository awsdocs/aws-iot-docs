# Jobs<a name="iot-jobs"></a>

Use AWS IoT Jobs to define a set of remote operations that can be sent to and run on one or more devices connected to AWS IoT\. For example, you can define a job that instructs a set of devices to download and install applications, run firmware updates, reboot, rotate certificates, or perform remote troubleshooting operations\.

To create jobs, first define a *job document* that contains a list of instructions describing operations that the device must perform remotely\. To perform these operations, specify a list of *targets*, which are individual things, [thing groups](thing-groups.md), or both\. The job document and targets together constitute a *deployment*\.

Each deployment can have additional configurations:
+ **Rollout**: This configuration defines how many devices receive the job document every minute\.
+ **Abort**: If a certain number of devices don't receive the job document, use this configuration to cancel the job and avoid sending a bad update to an entire fleet\.
+ **Timeout**: If a response isn't received from your job targets within a certain duration, the job can fail\. You can keep track of the job that's running on these devices\.

AWS IoT Jobs sends a message to inform the targets that a job is available\. The target starts the *execution* of the job by downloading the job document, performing the operations it specifies, and reporting its progress to AWS IoT\. You can track the progress of a job for a specific target and for all targets of the job by running commands that are provided by AWS IoT Jobs\. When a job has started, an *In progress* status is reported\. The devices then report incremental updates while displaying this status until the job has succeeded, failed, or timed out\.