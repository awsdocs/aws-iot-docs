# Create a job from AWS managed templates by using the AWS CLI<a name="job-template-manage-cli-create"></a>

Use the AWS CLI to get information about AWS managed templates and create a job by using these templates\. You can then save the job as a template and then create your own custom template\.

## List managed templates<a name="job-template-manage-cli-list"></a>

The [https://docs.aws.amazon.com/cli/latest/reference/iot/list-managed-job-templates.html](https://docs.aws.amazon.com/cli/latest/reference/iot/list-managed-job-templates.html) AWS CLI command lists all of the job templates in your AWS account\.

```
 aws iot list-managed-job-templates
```

By default, running this command displays all available AWS managed templates and their details\.

```
{
    "managedJobTemplates": [
        {
            "templateArn": "arn:aws:iot:region::jobtemplate/AWS-Reboot:1.0",
            "templateName": "AWS-Reboot",
            "description": "A managed job template for rebooting the device.",
            "environments": [
                "LINUX"
            ],
            "templateVersion": "1.0"
        },
        {
            "templateArn": "arn:aws:iot:region::jobtemplate/AWS-Remove-Application:1.0",
            "templateName": "AWS-Remove-Application",
            "description": "A managed job template for uninstalling one or more applications.",
            "environments": [
                "LINUX"
            ],
            "templateVersion": "1.0"
        },
        {
            "templateArn": "arn:aws:iot:region::jobtemplate/AWS-Stop-Application:1.0",
            "templateName": "AWS-Stop-Application",
            "description": "A managed job template for stopping one or more system services.",
            "environments": [
                "LINUX"
            ],
            "templateVersion": "1.0"
        },

        ...

        {
            "templateArn": "arn:aws:iot:us-east-1::jobtemplate/AWS-Restart-Application:1.0",
            "templateName": "AWS-Restart-Application",
            "description": "A managed job template for restarting one or more system services.",
            "environments": [
                "LINUX"
            ],
            "templateVersion": "1.0"
        }
    ]
}
```

For more information, see [ListManagedJobTemplates](https://docs.aws.amazon.com/iot/latest/apireference/API_ListManagedJobTemplates.html)\.

## Get details about a managed template<a name="job-template-manage-cli-get"></a>

The [https://docs.aws.amazon.com/cli/latest/reference/iot/describe-managed-job-template.html](https://docs.aws.amazon.com/cli/latest/reference/iot/describe-managed-job-template.html) AWS CLI command gets details about a specified job template\. Specify the job template name and an optional template version\. If the template version is not specified, the predefined, default version is returned\. The following shows an example of running the command to get details about the `AWS-Download-File` template\.

```
aws iot describe-managed-job-template \
    --template-name AWS-Download-File
```

The command displays the template details and ARN, its job document, and the `documentParameters` parameter, which is a list of key value pairs of input parameters of the template\. For information about the different templates and input parameters, see [Managed template remote actions and job documents](job-templates-managed.md#job-template-manage-actions)\.

**Note**  
The `documentParameters` object returned when you use this API must only be used when creating jobs from AWS managed templates\. The object must not be used for custom job templates\. For an example that shows how to use this parameter, see [Create a job by using managed templates](#job-template-manage-cli-create-job)\.

```
{
    "templateName": "AWS-Download-File",
    "templateArn": "arn:aws:iot:region::jobtemplate/AWS-Download-File:1.0",
    "description": "A managed job template for downloading a file.",
    "templateVersion": "1.0",
    "environments": [
        "LINUX"
    ],
    "documentParameters": [
        {
            "key": "downloadUrl",
            "description": "URL of file to download.",
            "regex": "(.*?)",
            "example": "http://www.example.com/index.html",
            "optional": false
        },
        {
            "key": "filePath",
            "description": "Path on the device where downloaded file is written.",
            "regex": "(.*?)",
            "example": "/path/to/file",
            "optional": false
        },
        {
            "key": "runAsUser",
            "description": "Execute handler as another user. If not specified, then handler is executed as the same user as device client.",
            "regex": "(.){0,256}",
            "example": "user1",
            "optional": true
        },
        {
            "key": "pathToHandler",
            "description": "Path to handler on the device. If not specified, then device client will use the current working directory.",
            "regex": "(.){0,4096}",
            "example": "/path/to/handler/script",
            "optional": true
        }
    ],
    "document": "{\"version\":\"1.0\",\"steps\":[{\"action\":{\"name\":\"Download-File\",\"type\":\"runHandler\",\"input\":{\"handler\":\"download-file.sh\",\"args\":[\"${aws:iot:parameter:downloadUrl}\",\"${aws:iot:parameter:filePath}\"],\"path\":\"${aws:iot:parameter:pathToHandler}\"},\"runAsUser\":\"${aws:iot:parameter:runAsUser}\"}}]}"
}
```

For more information, see [DescribeManagedJobTemplate](https://docs.aws.amazon.com/iot/latest/apireference/API_DescribeManagedJobTemplate.html)\. 

## Create a job by using managed templates<a name="job-template-manage-cli-create-job"></a>

The [https://docs.aws.amazon.com/cli/latest/reference/iot/create-job.html](https://docs.aws.amazon.com/cli/latest/reference/iot/create-job.html) AWS CLI command can be used to create a job from a job template\. It targets a device named `thingOne` and specifies the Amazon Resource Name \(ARN\) of the managed template to use as the basis for the job\. You can override advanced configurations, such as timeout and cancel configurations, by passing the associated parameters of the `create-job` command\.

The example shows how to create a job that uses the `AWS-Download-File` template\. It also shows how to specify the input parameters of the template by using the `document-parameters` parameter\. 

**Note**  
You must use the `document-parameters` object only with AWS managed templates\. This object must not be used with custom job templates\.

```
aws iot create-job \
   --targets arn:aws:iot:region:account-id:thing/thingOne \ 
   --job-id "new-managed-template-job" \ 
   --job-template-arn arn:aws:iot:region::jobtemplate/AWS-Download-File:1.0 \
   --document-parameters downloadUrl=https://example.com/index.html,filePath=path/to/file
```

where:
+ *region* is the AWS Region\.
+ *account\-id* is the unique AWS account number\.
+ `thingOne` is the name of the IoT thing for which the job is targeted\.
+ *AWS\-Download\-File:1\.0* is the name of the managed template\.
+ `https://example.com/index.html` is the URL to download the file from\.
+ `https://pathto/file/index` is the path on the device to store the downloaded file\.

Running this command creates a job for the template, `AWS-Download-File`, as shown below\.

```
{
    "jobArn": "arn:aws:iot:region:account-id:job/new-managed-template-job",
    "jobId": "new-managed-template-job",
    "description": "A managed job template for downloading a file."
}
```

## Create a custom job template from managed templates<a name="job-template-manage-cli-create-jobtemplate"></a>

1. Create a job using a managed template as described in the previous section\.

1. Create a custom job template by using the ARN of the job that you created\. For more information, see [Create a job template from an existing job](job-templates-cli.md#job-templates-cli-create-from-job)\.