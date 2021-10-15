# Attach a thing or policy to a client certificate<a name="attach-to-cert"></a>

When you create and register a certificate separate from an AWS IoT thing, it will not have any policies that authorize any AWS IoT operations, nor will it be associated with any AWS IoT thing object\. This section describes how to add these relationships to a registered certificate\.

**Important**  
To complete these procedures, you must have already created the thing or policy that you want to attach to the certificate\.

The certificate authenticates a device with AWS IoT so that it can connect\. Attaching the certificate to a thing resource establishes the relationship between the device \(by way of the certificate\) and the thing resource\. To authorize the device to perform AWS IoT actions, such as to allow the device to connect and publish messages, an appropriate policy must be attached to the device's certificate\. 

## Attach a thing to a client certificate \(console\)<a name="attach-to-cert-thing-console"></a>

You will need the name of the thing object to complete this procedure\.

**To attach a thing object to a registered certificate**

1. Sign in to the AWS Management Console and open the [AWS IoT console](https://console.aws.amazon.com/iot/home)\.

1. In the left navigation pane, choose **Secure**, choose **Certificates**\.

1. In the list of certificates, locate the certificate to which you want to attach a policy, open the certificate's option menu by choosing the ellipsis icon, and choose **Attach thing**\.

1. In the pop\-up, locate the name of the thing you want to attach to the certificate, choose its check box, and choose **Attach**\.

The thing object should now appear in the list of things on the certificate's details page\.

## Attach a policy to a client certificate \(console\)<a name="attach-to-cert-policy-console"></a>

You will need the name of the policy object to complete this procedure\.

**To attach a policy object to a registered certificate**

1. Sign in to the AWS Management Console and open the [AWS IoT console](https://console.aws.amazon.com/iot/home)\.

1. In the left navigation pane, choose **Secure**, choose **Certificates**\.

1. In the list of certificates, locate the certificate to which you want to attach a policy, open the certificate's option menu by choosing the ellipsis icon, and choose **Attach policy**\. 

1. In the pop\-up, locate the name of the policy you want to attach to the certificate, choose its check box, and choose **Attach**\.

The policy object should now appear in the list of policies on the certificate's details page\.

## Attach a thing to a client certificate \(CLI\)<a name="attach-to-cert-thing-cli"></a>

The AWS CLI provides the [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/iot/attach-thing-principal.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/iot/attach-thing-principal.html) command to attach a thing object to a certificate\.

```
aws iot attach-thing-principal \
    --principal certificateArn \
    --thing-name thingName
```

## Attach a policy to a client certificate \(CLI\)<a name="attach-to-cert-policy-cli"></a>

The AWS CLI provides the [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/iot/attach-policy.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/iot/attach-policy.html) command to attach a policy object to a certificate\.

```
aws iot attach-policy \
    --target certificateArn \
    --policy-name policyName
```