# Troubleshooting<a name="aws-iot-monitor-admin-troubleshoot"></a>

This section provides troubleshooting information and possible solutions to help resolve issues as an administrator of Fleet Hub\.


| Symptom | Solution | 
| --- | --- | 
| My web application link doesn't work\. | It might take a few hours after you've created your application for the link to work\. | 
| I can't log in to my web application\. | Make sure that you have added at least one user to your application\. Make sure that your role has the appropriate trust relationship such as the following:  <pre>{"Version": "2012-10-17",<br />  "Statement": [<br />    {<br />      "Effect": "Allow",<br />      "Principal": {<br />        "Service": "iotfleethub.amazonaws.com"<br />      },<br />      "Action": "sts:AssumeRole"<br />    }<br />  ]<br />}</pre> For more information about how to edit IAM trust relationship, see [Editing the trust relationship for an existing role](https://docs.aws.amazon.com/directoryservice/latest/admin-guide/edit_trust.html)\. | 
| I can't create a web application\. | Make sure that you haven't reached your limit of total number of web applications\. | 
| I'm not seeing a custom field that I'm expecting\. | Check to make sure that you've set up fleet indexing correctly\. For more information about fleet indexing, see [Fleet indexing service](https://docs.aws.amazon.com/iot/latest/developerguide/iot-indexing.html)\. | 