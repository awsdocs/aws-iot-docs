# Use AWS managed templates to deploy common remote operations<a name="job-templates-managed"></a>

AWS managed templates are job templates provided by AWS for frequently performed remote actions such as rebooting, downloading a file, or installing an application on your devices\. These templates have a predefined job document for each remote action so you don't have to create your own job document\.

You can choose from a set of predefined configurations and create jobs using these templates without writing any additional code\. Using managed templates, you can view the job document deployed to your fleets\. You can create a job using these templates and further create a custom job template that you can reuse for your remote operations\.

## What do managed templates contain?<a name="job-template-managed-contains"></a>

Each AWS managed template contains:
+ A job document that specifies the name of the operation and its parameters\. For example, if you use a **Download file** template, the operation name is *Download file* and the parameters can be:
  + The URL of the file you want to download to your device, which can be an internet resource, or a public or pre\-signed S3 URL\.
  + A local file path on the device to store the downloaded file\.

  For more information about the job documents and its parameters, see [Managed template remote actions and job documents](#job-template-manage-actions)\.
+ The environment to run the commands in the job document\. 

## Prerequisites<a name="job-template-managed-prereq"></a>

For your devices to run the remote actions specified by the managed template job document, you must:
+ **Install the specific software on your device**
  + 

****Use the AWS IoT Device Client****  
We recommend that you install and run the AWS IoT Device Client on your devices because it supports using all managed templates directly from the console by default\.

    The Device Client is an open\-source software written in C\+\+ that you can compile and install on your embedded Linux\-based IoT devices\. The Device Client has a *base client* and discrete *client\-side features*\. The base client establishes connectivity with AWS IoT over MQTT protocol and can connect with the different client\-side features\.

    To perform remote operations on your devices, use the *client\-side Jobs feature* of the Device Client\. This feature contains a parser to receive the job document and job handlers that implement the remote actions specified in the job document\. For more information about the Device Client and its features, see [AWS IoT Device Client\.](https://github.com/awslabs/aws-iot-device-client#readme)

    When running on devices, the Device Client receives the job document and has a platform\-specific implementation that it uses to run commands in the document\. For more information about setting up the Device Client and using the Jobs feature, see [AWS IoT tutorials](iot-tutorials.md)\.
  + 

**Use your own device software and job handlers**  
Alternatively, you can write your own code for the devices by using the AWS IoT Device SDK and its library of handlers that supports the remote operations\. To deploy and run jobs, make sure that the device agent libraries have been installed correctly and are running on these devices\.

    You can also choose to use your own handlers that can support the remote operations\. For information about how you can create these handlers, see [Sample job handlers in the AWS IoT Device Client GitHub repository](https://github.com/awslabs/aws-iot-device-client/tree/main/sample-job-handlers)\.
+ **Use a supported environment**

  For each managed template, you'll find information about the environment that you can use to run the remote actions\. We recommend that you use the template with a supported Linux environment as specified in the template\. You can use the AWS IoT Device Client to run the manage template remote actions because it supports common microprocessors and Linux environments, such as Debian and Ubuntu\.

## Managed template remote actions and job documents<a name="job-template-manage-actions"></a>

The following shows the different managed templates and the remote actions that can be performed on the devices\. For each remote action, you'll also find information about its job document and a description of the various parameters\. The device agent or the job handler in the Device Client uses the template name and input parameters to perform the remote operation, which is the device behavior\.

All templates, except for the `AWS-Reboot` template, require an input parameter such as a list of packages to install, or a URL to download files from\. You specify a value for these parameters when creating a job using the managed template\. All managed templates have the following two optional parameters in common\.

`runAsUser`  
This parameter specifies whether to run the job handler as another user\. If it's not specified during job creation, the job handler is run as the same user as the Device Client\. When you run the job handler as another user, specify a string value that's not longer than 256 characters\.

`pathToHandler`  
The path to the job handler running on the device\. If not specified during job creation, theh Device Client uses the current working directory\.

The following shows the different remote actions, their job documents, and parameters they accept\. All these templates support the Linux environment for running the remote operation on the device\.

### AWS–Reboot<a name="reboot-managed-template"></a>

**Template name**  
`AWS–Reboot`

**Template description**  
A managed template provided by AWS for rebooting your device\.

**Input parameters**  
This template has no required parameters\. You can specify the optional parameters `runAsUser` and `pathToHandler`\.

**Device behavior**  
The device reboots successfully\.

**Job document**  
The following shows the job document and its latest version\. The template shows the path to the job handler and the shell script, `reboot.sh`, that the job handler must run to reboot the device\.

```
{
  "version": "1.0",
  "steps": [
    {
      "action": {
        "name": "Reboot",
        "type": "runHandler",
        "input": {
          "handler": "reboot.sh",
          "path": "${aws:iot:parameter:pathToHandler}"
        },
        "runAsUser": "${aws:iot:parameter:runAsUser}"
      }
    }
  ]
}
```

### AWS–Download–File<a name="download-managed-template"></a>

**Template name**  
`AWS–Download–File`

**Template description**  
A managed template provided by AWS for downloading a file\.

**Input parameters**  
This template has the following required parameters\. You can also specify the optional parameters `runAsUser` and `pathToHandler`\. 

`downloadUrl`  
The URL to download the file from, which can be an internet resource, an object in Amazon S3 that can be publicly accessed, or an object in Amazon S3 that can only be accessed by your device using a presigned URL\. For more information about using presigned URLs and granting permissions, see [Presigned URLs](create-manage-jobs.md#create-manage-jobs-presigned-URLs)\.

`filePath`  
A local file path that shows the location in the device to store the downloaded file\.

**Device behavior**  
The device downloads the file from the specified location, verifies that the download is complete, and stores it locally\.

**Job document**  
The following shows the job document and its latest version\. The template shows the path to the job handler and the shell script, `download-file.sh`, that the job handler must run to download the file\. It also shows the required parameters `downloadUrl` and `filePath`\.

```
{
  "version": "1.0",
  "steps": [
    {
      "action": {
        "name": "Download-File",
        "type": "runHandler",
        "input": {
          "handler": "download-file.sh",
          "args": [
            "${aws:iot:parameter:downloadUrl}",
            "${aws:iot:parameter:filePath}"
          ],
          "path": "${aws:iot:parameter:pathToHandler}"
        },
        "runAsUser": "${aws:iot:parameter:runAsUser}"
      }
    }
  ]
}
```

### AWS–Install–Application<a name="install-managed-template"></a>

**Template name**  
`AWS–Install–Application`

**Template description**  
A managed template provided by AWS for installing one or more applications\.

**Input parameters**  
This template has the following required parameter, `packages`\. You can also specify the optional parameters `runAsUser` and `pathToHandler`\. 

`packages`

A space\-separated list of one or more applications to be installed\.

**Device behavior**  
The device installs the applications as specified in the job document\.

**Job document**  
The following shows the job document and its latest version\. The template shows the path to the job handler and the shell script, `install-packages.sh`, that the job handler must run to download the file\. It also shows the required parameter `packages`\.

```
{
  "version": "1.0",
  "steps": [
    {
      "action": {
        "name": "Install-Application",
        "type": "runHandler",
        "input": {
          "handler": "install-packages.sh",
          "args": [
            "${aws:iot:parameter:packages}"
          ],
          "path": "${aws:iot:parameter:pathToHandler}"
        },
        "runAsUser": "${aws:iot:parameter:runAsUser}"
      }
    }
  ]
}
```

### AWS–Remove–Application<a name="uninstall-managed-template"></a>

**Template name**  
`AWS–Remove–Application`

**Template description**  
A managed template provided by AWS for uninstalling one or more applications\.

**Input parameters**  
This template has the following required parameter, `packages`\. You can also specify the optional parameters `runAsUser` and `pathToHandler`\. 

`packages`

A space\-separated list of one or more applications to be uninstalled\.

**Device behavior**  
The device uninstalls the applications as specified in the job document\.

**Job document**  
The following shows the job document and its latest version\. The template shows the path to the job handler and the shell script, `remove-packages.sh`, that the job handler must run to download the file\. It also shows the required parameter `packages`\.

```
{
  "version": "1.0",
  "steps": [
    {
      "action": {
        "name": "Remove-Application",
        "type": "runHandler",
        "input": {
          "handler": "remove-packages.sh",
          "args": [
            "${aws:iot:parameter:packages}"
          ],
          "path": "${aws:iot:parameter:pathToHandler}"
        },
        "runAsUser": "${aws:iot:parameter:runAsUser}"
      }
    }
  ]
}
```

### AWS–Start–Application<a name="startsvs-managed-template"></a>

**Template name**  
`AWS-Start-Application`

**Template description**  
A managed template provided by AWS for starting one or more services\.

**Input parameters**  
This template has the following required parameter, `services`\. You can also specify the optional parameters `runAsUser` and `pathToHandler`\. 

`services`

A space\-separated list of one or more applications to be started\.

**Device behavior**  
The specified applications start running on the device\.

**Job document**  
The following shows the job document and its latest version\. The template shows the path to the job handler and the shell script, `start-services.sh`, that the job handler must run to start the system services\. It also shows the required parameter `services`\.

```
{
  "version": "1.0",
  "steps": [
    {
      "action": {
        "name": "Start-Application",
        "type": "runHandler",
        "input": {
          "handler": "start-services.sh",
          "args": [
            "${aws:iot:parameter:services}"
          ],
          "path": "${aws:iot:parameter:pathToHandler}"
        },
        "runAsUser": "${aws:iot:parameter:runAsUser}"
      }
    }
  ]
}
```

### AWS–Stop–Application<a name="stopsvs-managed-template"></a>

**Template name**  
`AWS–Stop–Application`

**Template description**  
A managed template provided by AWS for stopping one or more services\.

**Input parameters**  
This template has the following required parameter, `services`\. You can also specify the optional parameters `runAsUser` and `pathToHandler`\. 

`services`

A space\-separated list of one or more applications to be stopped\.

**Device behavior**  
The specified applications stop running on the device\.

**Job document**  
The following shows the job document and its latest version\. The template shows the path to the job handler and the shell script, `stop-services.sh`, that the job handler must run to stop the system services\. It also shows the required parameter `services`\.

```
{
  "version": "1.0",
  "steps": [
    {
      "action": {
        "name": "Stop-Application",
        "type": "runHandler",
        "input": {
          "handler": "stop-services.sh",
          "args": [
            "${aws:iot:parameter:services}"
          ],
          "path": "${aws:iot:parameter:pathToHandler}"
        },
        "runAsUser": "${aws:iot:parameter:runAsUser}"
      }
    }
  ]
}
```

### AWS–Restart–Application<a name="restartsvs-managed-template"></a>

**Template name**  
`AWS–Restart–Application`

**Template description**  
A managed template provided by AWS for stopping and restarting one or more services\.

**Input parameters**  
This template has the following required parameter, `services`\. You can also specify the optional parameters `runAsUser` and `pathToHandler`\. 

`services`

A space\-separated list of one or more applications to be restarted\.

**Device behavior**  
The specified applications are stopped and then restarted on the device\.

**Job document**  
The following shows the job document and its latest version\. The template shows the path to the job handler and the shell script, `restart-services.sh`, that the job handler must run to restart the system services\. It also shows the required parameter `services`\.

```
{
  "version": "1.0",
  "steps": [
    {
      "action": {
        "name": "Restart-Application",
        "type": "runHandler",
        "input": {
          "handler": "restart-services.sh",
          "args": [
            "${aws:iot:parameter:services}"
          ],
          "path": "${aws:iot:parameter:pathToHandler}"
        },
        "runAsUser": "${aws:iot:parameter:runAsUser}"
      }
    }
  ]
}
```

**Topics**
+ [What do managed templates contain?](#job-template-managed-contains)
+ [Prerequisites](#job-template-managed-prereq)
+ [Managed template remote actions and job documents](#job-template-manage-actions)
+ [Create a job from AWS managed templates by using the AWS Management Console](job-template-manage-console-create.md)
+ [Create a job from AWS managed templates by using the AWS CLI](job-template-manage-cli-create.md)