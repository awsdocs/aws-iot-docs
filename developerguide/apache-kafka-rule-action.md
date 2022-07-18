# Apache Kafka<a name="apache-kafka-rule-action"></a>

The Apache Kafka \(Kafka\) action sends messages directly to your Amazon Managed Streaming for Apache Kafka \(Amazon MSK\) or self\-managed Apache Kafka clusters for data analysis and visualization\.

**Note**  
This topic assumes familiarity with the Apache Kafka platform and related concepts\. For more information about Apache Kafka, see [Apache Kafka](https://kafka.apache.org/)\.

## Requirements<a name="apache-kafka-rule-action-requirements"></a>

This rule action has the following requirements:
+ An IAM role that AWS IoT can assume to perform the `ec2:CreateNetworkInterface`, `ec2:DescribeNetworkInterfaces`, `ec2:CreateNetworkInterfacePermission`, `ec2:DeleteNetworkInterface`, `ec2:DescribeSubnets`, `ec2:DescribeVpcs`, `ec2:DescribeVpcAttribute`, and `ec2:DescribeSecurityGroups` operations\. This role creates and manages elastic network interfaces to your Amazon Virtual Private Cloud to reach your Kafka broker\. For more information, see [Granting an AWS IoT rule the access it requires](iot-create-role.md)\.

  In the AWS IoT console, you can choose or create a role to allow AWS IoT Core to perform this rule action\. 

  For more information about network interfaces, see [Elastic network interfaces](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-eni.html) in the Amazon EC2 User Guide\.

  The policy attached to the role you specify should look like the following example\.

  ```
  {
      "Version": "2012-10-17",
      "Statement": [
      {
          "Effect": "Allow",
          "Action": [
              "ec2:CreateNetworkInterface",
              "ec2:DescribeNetworkInterfaces",
              "ec2:CreateNetworkInterfacePermission",
              "ec2:DeleteNetworkInterface",
              "ec2:DescribeSubnets",
              "ec2:DescribeVpcs",
              "ec2:DescribeVpcAttribute",
              "ec2:DescribeSecurityGroups"
              ],
              "Resource": "*"
          }
      ]
  }
  ```
+ If you use AWS Secrets Manager to store the credentials required to connect to your Kafka broker, you must create an IAM role that AWS IoT Core can assume to perform the `secretsmanager:GetSecretValue` and `secretsmanager:DescribeSecret` operations\.

  The policy attached to the role you specify should look like the following example\.

  ```
  {
      "Version": "2012-10-17",
      "Statement": [
      {
          "Effect": "Allow",
          "Action": [
              "secretsmanager:GetSecretValue",
              "secretsmanager:DescribeSecret"
              ],
              "Resource": [
                  "arn:aws:secretsmanager:region:123456789012:secret:kafka_client_truststore-*",
                  "arn:aws:secretsmanager:region:123456789012:secret:kafka_keytab-*"
              ]
          }
      ]
  }
  ```
+ You can run your Apache Kafka clusters inside Amazon Virtual Private Cloud\. You must create a virtual private cloud \(VPC\) destination and use a NAT gateway in your subnets to forward messages from AWS IoT to a public Kafka cluster\. The AWS IoT rules engine creates a network interface in each of the subnets listed in the VPC destination to route traffic directly to the VPC\. When you create a VPC destination, the AWS IoT rules engine automatically creates a VPC rule action\. For more information about VPC rule actions, see [Virtual private cloud \(VPC\) destinations](vpc-rule-action.md)\.
+ If you use a customer\-managed AWS KMS key \(KMS key\) to encrypt data at rest, the service must have permission to use the KMS key on the caller's behalf\. For more information, see [Amazon MSK encryption](https://docs.aws.amazon.com/msk/latest/developerguide/msk-encryption.html) in the *Amazon Managed Streaming for Apache Kafka Developer Guide*\.

## Parameters<a name="apache-kafka-rule-action-parameters"></a>

When you create an AWS IoT rule with this action, you must specify the following information:

destinationArn  
The Amazon Resource Name \(ARN\) of the VPC destination\. For information about creating a VPC destination, see [Virtual private cloud \(VPC\) destinations](vpc-rule-action.md)\.

topic  
The Kafka topic for messages to be sent to the Kafka broker\.  
You can substitute this field using a substitution template\. For more information, see [Substitution templates](iot-substitution-templates.md)\. 

key \(optional\)  
The Kafka message key\.  
You can substitute this field using a substitution template\. For more information, see [Substitution templates](iot-substitution-templates.md)\. 

partition \(optional\)  
The Kafka message partition\.  
You can substitute this field using a substitution template\. For more information, see [Substitution templates](iot-substitution-templates.md)\. 

clientProperties  
An object that defines the properties of the Apache Kafka producer client\.    
acks \(optional\)  
The number of acknowledgments the producer requires the server to have received before considering a request complete\.  
If you specify 0 as the value, the producer will not wait for any acknowledgment from the server\. If the server doesn't receive the message, the producer won't retry to send the message\.  
Valid values: 0, 1\. The default value is 1\.  
bootstrap\.servers  
A list of host and port pairs \(`host1:port1`, `host2:port2`, etc\.\) used to establish the initial connection to your Kafka cluster\.  
compression\.type \(optional\)  
The compression type for all data generated by the producer\.  
Valid values: `none`, `gzip`, `snappy`, `lz4`, `zstd`\. The default value is `none`\.  
security\.protocol  
The security protocol used to attach to your Kafka broker\.  
Valid values: `SSL`, `SASL_SSL`\. The default value is `SSL`\.  
key\.serializer  
Specifies how to turn the key objects you provide with the`ProducerRecord` into bytes\.  
Valid value: `StringSerializer`\.  
value\.serializer  
Specifies how to turn value objects you provide with the `ProducerRecord` into bytes\.  
Valid value: `ByteBufferSerializer`\.  
ssl\.truststore  
The truststore file in base64 format or the location of the truststore file in [AWS Secrets Manager](https://docs.aws.amazon.com/secretsmanager/latest/userguide/)\. This value isn't required if your truststore is trusted by Amazon certificate authoriies \(CA\)\.  
This field supports substitution templates\. If you use Secrets Manager to store the credentials required to connect to your Kafka broker, you can use the get\_secret SQL function to retrieve the value for this field\. For more information about substitution templates, see [Substitution templates](iot-substitution-templates.md)\. For more information about the get\_secret SQL function, see [get\_secret\(secretId, secretType, key, roleArn\)](iot-sql-functions.md#iot-sql-function-get-secret)\. If the truststore is in the form of a file, use the `SecretBinary` parameter\. If the truststore is in the form of a string, use the `SecretString` parameter\.  
The maximum size of this value is 65 KB\.  
ssl\.truststore\.password  
The password for the truststore\. This value is required only if you've created a password for the truststore\.  
ssl\.keystore  
The keystore file\. This value is required when you specify `SSL` as the value for `security.protocol`\.  
This field supports substitution templates\. You must use Secrets Manager to store the credentials required to connect to your Kafka broker\. Use the get\_secret SQL function to retrieve the value for this field\. For more information about substitution templates, see [Substitution templates](iot-substitution-templates.md)\. For more information about the get\_secret SQL function, see [get\_secret\(secretId, secretType, key, roleArn\)](iot-sql-functions.md#iot-sql-function-get-secret)\. Use the `SecretBinary` parameter\.  
ssl\.keystore\.password  
The store password for the keystore file\. This value is required if you specify a value for `ssl.keystore`\.  
The value of this field can be plain text\. This field also supports substitution templates\. You must use Secrets Manager to store the credentials required to connect to your Kafka broker\. Use the get\_secret SQL function to retrieve the value for this field\. For more information about substitution templates, see [Substitution templates](iot-substitution-templates.md)\. For more information about the get\_secret SQL function, see [get\_secret\(secretId, secretType, key, roleArn\)](iot-sql-functions.md#iot-sql-function-get-secret)\. Use the `SecretString` parameter\.  
ssl\.key\.password  
The password of the private key in your keystore file\.  
This field supports substitution templates\. You must use Secrets Manager to store the credentials required to connect to your Kafka broker\. Use the get\_secret SQL function to retrieve the value for this field\. For more information about substitution templates, see [Substitution templates](iot-substitution-templates.md)\. For more information about the get\_secret SQL function, see [get\_secret\(secretId, secretType, key, roleArn\)](iot-sql-functions.md#iot-sql-function-get-secret)\. Use the `SecretString` parameter\.  
sasl\.mechanism  
The security mechanism used to connect to your Kafka broker\. This value is required when you specify `SASL_SSL` for `security.protocol`\.  
Valid values: `PLAIN`, `SCRAM-SHA-512`, `GSSAPI`\.  
`SCRAM-SHA-512` is the only supported security mechanism in the cn\-north\-1, cn\-northwest\-1, us\-gov\-east\-1, and us\-gov\-west\-1 Regions\.  
sasl\.plain\.username  
The user name used to retrieve the secret string from Secrets Manager\. This value is required when you specify `SASL_SSL` for `security.protocol` and `PLAIN` for `sasl.mechanism`\.  
sasl\.plain\.password  
The password used to retrieve the secret string from Secrets Manager\. This value is required when you specify `SASL_SSL` for `security.protocol` and `PLAIN` for `sasl.mechanism`\.  
sasl\.scram\.username  
The user name used to retrieve the secret string from Secrets Manager\. This value is required when you specify `SASL_SSL` for `security.protocol` and `SCRAM-SHA-512` for `sasl.mechanism`\.  
sasl\.scram\.password  
The password used to retrieve the secret string from Secrets Manager\. This value is required when you specify `SASL_SSL` for `security.protocol` and `SCRAM-SHA-512` for `sasl.mechanism`\.  
sasl\.kerberos\.keytab  
The keytab file for Kerberos authentication in Secrets Manager\. This value is required when you specify `SASL_SSL` for `security.protocol` and `GSSAPI` for `sasl.mechanism`\.  
This field supports substitution templates\. You must use Secrets Manager to store the credentials required to connect to your Kafka broker\. Use the get\_secret SQL function to retrieve the value for this field\. For more information about substitution templates, see [Substitution templates](iot-substitution-templates.md)\. For more information about the get\_secret SQL function, see [get\_secret\(secretId, secretType, key, roleArn\)](iot-sql-functions.md#iot-sql-function-get-secret)\. Use the `SecretBinary` parameter\.  
sasl\.kerberos\.service\.name  
The Kerberos principal name under which Apache Kafka runs\. This value is required when you specify `SASL_SSL` for `security.protocol` and `GSSAPI` for `sasl.mechanism`\.  
sasl\.kerberos\.krb5\.kdc  
The hostname of the key distribution center \(KDC\) to which your Apache Kafka producer client connects\. This value is required when you specify `SASL_SSL` for `security.protocol` and `GSSAPI` for `sasl.mechanism`\.  
sasl\.kerberos\.krb5\.realm  
The realm to which your Apache Kafka producer client connects\. This value is required when you specify `SASL_SSL` for `security.protocol` and `GSSAPI` for `sasl.mechanism`\.  
sasl\.kerberos\.principal  
The unique Kerberos identity to which Kerberos can assign tickets to access Kerberos\-aware services\. This value is required when you specify `SASL_SSL` for `security.protocol` and `GSSAPI` for `sasl.mechanism`\.

## Examples<a name="apache-kafka-rule-action-examples"></a>

The following JSON example defines an Apache Kafka action in an AWS IoT rule\.

```
{
"topicRulePayload": {
    "sql": "SELECT * FROM 'some/topic'", 
    "ruleDisabled": false, 
    "awsIotSqlVersion": "2016-03-23",
    "actions": [
    {
        "kafka": {
            "destinationArn": "arn:aws:iot:region:123456789012:ruledestination/vpc/VPCDestinationARN",
            "topic": "TopicName",
            "clientProperties": {
                "bootstrap.servers": "kafka.com:9092",
                "security.protocol": "SASL_SSL",
                "ssl.truststore": "${get_secret('kafka_client_truststore', 'SecretBinary',
'arn:aws:iam::123456789012:role/kafka-get-secret-role-name')}",
                "ssl.truststore.password": "kafka password",
               "sasl.mechanism": "GSSAPI",
               "sasl.kerberos.service.name": "kafka",
               "sasl.kerberos.krb5.kdc": "kerberosdns.com",
               "sasl.kerberos.keytab": "${get_secret('kafka_keytab',
               'SecretBinary', 'arn:aws:iam::123456789012:role/kafka-get-secret-role-name')}",
               "sasl.kerberos.krb5.realm": "KERBEROSREALM",
               "sasl.kerberos.principal": "kafka-keytab/kafka-keytab.com"         
            }
        }
    }
]

}
```

**Important notes about your Kerberos setup**
+ Your key distribution center \(KDC\) must be resolvable through private Domain Name System \(DNS\) within your target VPC\. One possible approach is to add the KDC DNS entry to a private hosted zone\. For more information about this approach, see [Working with private hosted zones](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/hosted-zones-private.html)\.
+ Each VPC must have DNS resolution enabled\. For more information, see [Using DNS with your VPC](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-dns.html)\.
+ Network interface security groups and instance\-level security groups in the VPC destination must allow traffic from within your VPC on the following ports\.
  + TCP traffic on the bootstrap broker listener port \(often 9092, but must be within the 9000 \- 9100 range\)
  + TCP and UDP traffic on port 88 for the KDC
+ `SCRAM-SHA-512` is the only supported security mechanism in the cn\-north\-1, cn\-northwest\-1, us\-gov\-east\-1, and us\-gov\-west\-1 Regions\.