# Granting AWS IoT the Required Access<a name="iot-create-role"></a>

You use IAM roles to control the AWS resources to which each rule has access\. Before you create a rule, you must create an IAM role with a policy that allows access to the required AWS resources\. AWS IoT assumes this role when executing a rule\.

**To create an IAM role \(AWS CLI\)**

1. Save the following trust policy document, which grants AWS IoT permission to assume the role, to a file called iot\-role\-trust\.json:

   ```
   {
     "Version":"2012-10-17",
     "Statement":[{
         "Effect": "Allow",
         "Principal": {
           "Service": "iot.amazonaws.com"
         },
         "Action": "sts:AssumeRole"
     }]
   }
   ```

   Use the [create\-role](https://docs.aws.amazon.com/cli/latest/reference/iam/create-role.html) command to create an IAM role specifying the iot\-role\-trust\.json file:

   ```
   aws iam create-role --role-name my-iot-role --assume-role-policy-document file://iot-role-trust.json
   ```

   The output of this command looks like the following:

   ```
   {
     "Role": {
       "AssumeRolePolicyDocument": "url-encoded-json",
       "RoleId": "AKIAIOSFODNN7EXAMPLE",
       "CreateDate": "2015-09-30T18:43:32.821Z",
       "RoleName": "my-iot-role",
       "Path": "/",
       "Arn": "arn:aws:iam::123456789012:role/my-iot-role"
     }
   }
   ```

1. Save the following JSON into a file named iot\-policy\.json\.

   ```
   {
     "Version": "2012-10-17",
     "Statement": [{
       "Effect": "Allow",
       "Action": "dynamodb:*",
       "Resource": "*"
     }]
   }
   ```

   This JSON is an example policy document that grants AWS IoT administrator access to DynamoDB\.

   Use the [create\-policy](https://docs.aws.amazon.com/cli/latest/reference/iam/create-policy.html) command to grant AWS IoT access to your AWS resources upon assuming the role, passing in the iot\-policy\.json file:

   ```
   aws iam create-policy --policy-name my-iot-policy --policy-document file://my-iot-policy.json
   ```

   For more information about how to grant access to AWS services in policies for AWS IoT, see [Creating an AWS IoT Rule](iot-create-rule.md)\.

   The output of the [create\-policy](https://docs.aws.amazon.com/cli/latest/reference/iam/create-policy.html) command contains the ARN of the policy\. You need to attach the policy to a role\.

   ```
   {
     "Policy": {
       "PolicyName": "my-iot-policy",
       "CreateDate": "2015-09-30T19:31:18.620Z",
       "AttachmentCount": 0,
       "IsAttachable": true,
       "PolicyId": "ZXR6A36LTYANPAI7NJ5UV",
       "DefaultVersionId": "v1",
       "Path": "/",
       "Arn": "arn:aws:iam::123456789012:policy/my-iot-policy",
       "UpdateDate": "2015-09-30T19:31:18.620Z"
     }
   }
   ```

1. Use the [attach\-role\-policy](https://docs.aws.amazon.com/cli/latest/reference/iam/attach-role-policy.html) command to attach your policy to your role:

   ```
   aws iam attach-role-policy --role-name my-iot-role --policy-arn "arn:aws:iam::123456789012:policy/my-iot-policy"
   ```