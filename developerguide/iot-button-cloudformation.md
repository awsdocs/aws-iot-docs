# Configure Your AWS IoT Button with a AWS CloudFormation Template<a name="iot-button-cloudformation"></a>

When the AWS IoT button is pressed, it sends basic information about the button to an Amazon SNS topic\. The topic then forwards that information to you in an email message\. This quickstart shows you how to use an AWS CloudFormation template to configure your AWS IoT button\.

You need an AWS account and an AWS IoT button to complete the steps in this quickstart\.

1. Use the AWS IoT console to create an AWS IoT certificate:

   1. Open the [AWS IoT console](https://console.aws.amazon.com/iot/home)\. 

   1. If a **Welcome** page appears, choose **Get started**\.

   1. In the AWS region selector, choose the AWS region where you want to create the AWS IoT certificate \(for example, US East \(N\. Virginia\)\)\. You create all supporting AWS resources \(other AWS IoT resources and an Amazon SNS resource\) in the same AWS Region\.

   1. On the **Dashboard**, in the navigation pane, choose **Security**, and then choose **Certificates**\.

   1. On the **Certificates** pane, choose **Create**\. 

   1. Choose **One\-click certificate creation** and then choose **Create certificate**\.

   1. On the **Certificate created** page, choose **Download** for the certificate, private key, and root CA for AWS IoT, save each to your computer, and then choose **Activate**\.

   1. Choose **Done**\.

   1. On the **Certificates** page, choose the certificate you just created\.

   1. In the **Details** pane, make a note of the certificate ARN value \(for example, `arn:aws:iot:region-ID:account-ID:cert/random-ID`\)\. You need it later in this procedure\.

1. Use the AWS CloudFormation console at [https://console\.aws\.amazon\.com/cloudformation/](https://console.aws.amazon.com/cloudformation/) to create the AWS IoT resources, an Amazon SNS resource, and an IAM role:

   1. Save the following AWS CloudFormation template file named `AWSIoTButtonQuickStart.template` to your computer\.
**Note**  
Check the DSN for your button\. Make sure the beginning of the `"AllowedPattern"` string matches your DSN\.

      ```
      {
        "AWSTemplateFormatVersion": "2010-09-09",
        "Description": "Creates required AWS resources to allow an AWS IoT button to send information through an Amazon Simple Notification Service (Amazon SNS) topic to an email address.",  
        "Parameters": {
          "IoTButtonDSN": {
      	  "Type": "String",
      	  "AllowedPattern": "G030[A-Z][A-Z][0=9][0-9][0-9][0-5][0-9][1-7][0-9A-HJ-NP-X][0-9A-HJ-NP-X][0-9A-HJ-NP-X][0-9A-HJ-NP-X]",
      	  "Description": "The device serial number (DSN) of the AWS IoT Button. This can be found on the back of the button. The DSN must match the pattern of 'G030[A-Z][A-Z][0=9][0-9][0-9][0-5][0-9][1-7][0-9A-HJ-NP-X][0-9A-HJ-NP-X][0-9A-HJ-NP-X][0-9A-HJ-NP-X]'."
      	},
      	"CertificateARN": {
      	  "Type": "String",
      	  "Description": "The Amazon Resource Name (ARN) of the existing AWS IoT certificate." 
      	},
      	"SNSTopicName": {
      	  "Type": "String",
      	  "Default": "aws-iot-button-sns-topic",
      	  "Description": "The name of the Amazon SNS topic for AWS CloudFormation to create." 
      	},
      	"SNSTopicRoleName": {
      	  "Type": "String",
      	  "Default": "aws-iot-button-sns-topic-role",
      	  "Description": "The name of the IAM role for AWS CloudFormation to create. This IAM role allows AWS IoT to send notifications to the Amazon SNS topic."
      	},
      	"EmailAddress": {
      	  "Type": "String",
      	  "Description": "The email address for the Amazon SNS topic to send information to."
      	}
        },
        "Resources": {
          "IoTThing": {
            "Type": "AWS::IoT::Thing",
            "Properties": {
              "ThingName": {
      		  "Fn::Join" : [ "", 
      		    [ 
      		      "iotbutton_", 
                    { "Ref": "IoTButtonDSN" }			  
      			] 
      		  ]
      		}
            }
          },
      	"IoTPolicy": {
            "Type" : "AWS::IoT::Policy",
            "Properties": {
              "PolicyDocument": {
      		  "Version": "2012-10-17",
                "Statement": [
                  {
                    "Action": "iot:Publish",
                    "Effect": "Allow",
                    "Resource": {
      			    "Fn::Join": [ "", 
      		          [ 
      		            "arn:aws:iot:",
      					{ "Ref": "AWS::Region" },
      					":",
      					{ "Ref": "AWS::AccountId" },
      					":topic/iotbutton/",
      					{ "Ref": "IoTButtonDSN" }			  
      			      ] 
      		        ]
      			  }
                  }
                ]
              }		
            }
          },
      	"IoTPolicyPrincipalAttachment": {
            "Type": "AWS::IoT::PolicyPrincipalAttachment",
            "Properties": {
              "PolicyName": {
      		  "Ref": "IoTPolicy"
      		},
      		"Principal": {
      		  "Ref": "CertificateARN"
      		}
            }
          },
      	"IoTThingPrincipalAttachment": {
            "Type" : "AWS::IoT::ThingPrincipalAttachment",
            "Properties": {
              "Principal": { 
      		  "Ref": "CertificateARN"
      		},
      		"ThingName": { 
      		  "Ref": "IoTThing"
      		}
            }
          },
      	"SNSTopic": {
            "Type": "AWS::SNS::Topic",
      	  "Properties": {
      	    "DisplayName": "AWS IoT Button Press Notification",
      	    "Subscription": [
      		  {
      		    "Endpoint": {
      			  "Ref": "EmailAddress"
      			},
      			"Protocol": "email"
      		  }
      		],
              "TopicName": {
      		  "Ref": "SNSTopicName"
      		}
            }
          },
      	"SNSTopicRole": {
      	  "Type": "AWS::IAM::Role",
      	  "Properties": {
      	    "AssumeRolePolicyDocument": {
      		  "Version": "2012-10-17",
                "Statement": [
                  {
                    "Effect": "Allow",
                    "Principal": {
                      "Service": "iot.amazonaws.com"
                    },
                    "Action": "sts:AssumeRole"
                  }
                ]
      		},
      	    "Path": "/",
      		"Policies": [
      		  {
      		    "PolicyDocument": {
      			  "Version": "2012-10-17",
                    "Statement": [
      			    {
                        "Effect": "Allow",
                        "Action": "sns:Publish",
                        "Resource": {
      				    "Fn::Join": [ "", 
      		              [ 
      		                "arn:aws:sns:",
      					    { "Ref": "AWS::Region" },
      					    ":",
      					    { "Ref": "AWS::AccountId" },
      					    ":",
      					    { "Ref": "SNSTopicName" }	
                            ]					  
      			        ] 
      		          }
      				}
                    ]
      			},
      			"PolicyName": {
      			  "Ref": "SNSTopicRoleName"
      			}
      		  }
      		]
      	  }
      	},
      	"IoTTopicRule": {
            "Type": "AWS::IoT::TopicRule",
            "Properties": {
              "RuleName": {
      		  "Fn::Join": [ "", 
      		    [ 
      		     "iotbutton_",
      		     { "Ref": "IoTButtonDSN" }			  
      			] 
      		  ]
      		},
      		"TopicRulePayload": {
      		  "Actions": [ 
      		    {
      		      "Sns": {
      			    "RoleArn": {
      			      "Fn::GetAtt": [ "SNSTopicRole", "Arn" ]
      			    },
      			    "TargetArn": {
      			      "Ref": "SNSTopic"
      			    }
      			  }
      			}
      		  ],
      		  "AwsIotSqlVersion": "2015-10-08",
                "RuleDisabled": false,
                "Sql": {
      		    "Fn::Join": [ "", 
      		      [ 
      		        "SELECT * FROM 'iotbutton/",
      				{ "Ref": "IoTButtonDSN" },
      				"'" 
      		      ]
      			]
      		  }
              }
            }
          }
        }
      }
      ```

   1. Open the AWS CloudFormation console at [https://console\.aws\.amazon\.com/cloudformation/](https://console.aws.amazon.com/cloudformation/)\.

   1. Make sure the region where you created the AWS IoT certificate \(for example, US East \(N\. Virginia\)\) is displayed on the AWS region selector\.

   1. Choose **Create Stack**\.

   1. On the **Select Template** page, choose **Upload a template to Amazon S3**, and then choose **Browse**\.

   1. Select the `AWSIoTButtonQuickStart.template` file you saved earlier, choose **Open**, and then choose **Next**\.

   1. On the **Specify Details** page, for **Stack name**, enter a name for this AWS CloudFormation stack \(for example, `MyAWSIoTButtonStack`\)\.

   1. For **CertificateARN**, enter the Amazon Resource Name \(ARN\) of the AWS IoT certificate \(the certificate ARN value\) that you noted earlier\.

   1. For **EmailAddress**, enter your email address\.

   1. For **IoTButtonDSN**, enter the device serial number \(DSN\) that appears on the back of your AWS IoT button \(for example, G030JF051234A5BC\)\.

   1. You can leave **SNSTopicName** and **SNSTopicRoleName** at their defaults, or specify a different Amazon SNS topic name and associated IAM role name\. For example, if you plan to set up more AWS IoT buttons, you might want to change these values\. Choose **Next**\.

   1. You do not need to do anything on the **Options** page\. Choose **Next**\. 

   1. On the **Review** page, select **I acknowledge that AWS CloudFormation might create IAM resources**, and then choose **Create**\.

   1. When CREATE\_COMPLETE is displayed for `MyAWSIoTButtonStack`, check your email inbox for a message with a subject line of AWS IoT Button Press Notification\. Choose the **Confirm subscription** link in the body of the email message\.

1. Using the private key and certificate you created earlier, follow the steps in [Configure Your Device](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/configure-iot.html) to set up your AWS IoT button\.

1. After you have set it up, press the button once\. You should see a blinking white light several times followed by a steady green light for a few moments\. Shortly afterward, you should receive an email message with AWS IoT Button Press Notification in the subject line\. It contains information sent by the button in the body of the email message\.

1. After you are finished experimenting, you can clean up the AWS resources created by the AWS CloudFormation template\. To do this, return to the AWS CloudFormation console and delete `MyAWSIoTButtonStack`\. Follow these steps to delete the AWS IoT certificate:

   1. Return to the AWS IoT console\.

   1. In the list of resources, select the check box inside of the box that represents the AWS IoT certificate \(the box with the handshake icon\)\.

   1. For **Actions**, choose **Deactivate**, and then confirm\.

   1. With the box that represents the AWS IoT certificate still selected, for **Actions**, choose **Delete**, and then confirm\.

   1. The private key and certificate that you downloaded earlier are no longer valid, so you can now delete them from your computer\.