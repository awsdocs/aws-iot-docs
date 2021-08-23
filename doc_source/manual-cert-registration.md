# Register a client certificate manually<a name="manual-cert-registration"></a>

You can register a client certificate manually by using the AWS IoT console and AWS CLI\.

The registration procedure to use depends on whether the certificate will be shared by shared by AWS accounts and Regions\. The registration of a client certificate in one account or Region is not automatically recognized by another\.

The procedures in this topic must be performed in each account and Region in which you want to use the client certificate\. Client certificates can be shared by AWS accounts and Regions, but only if the client certificate is signed by a certificate authority \(CA\) that is NOT registered with AWS IoT\. 

## Register a client certificate signed by a registered CA \(console\)<a name="manual-cert-registration-console"></a>

**Note**  
Before you perform this procedure, make sure that you have the client certificate's \.pem file and that the client certificate was signed by a CA that you have [registered with AWS IoT](register-CA-cert.md)\.

**To register an existing certificate with AWS IoT using the console**

1. Sign in to the AWS Management Console and open the [AWS IoT console](https://console.aws.amazon.com/iot/home)\.

1. In the left navigation pane, choose **Secure**, choose **Certificates**, and then choose **Create**\.

1. On **Create a certificate**, locate the **Use my certificate** entry, and choose **Get started**\.

1. On **Select a CA**:
   + 

**if the client certificates are signed by a CA that is registered with AWS IoT**  
Choose that CA from the list, and then choose **Next**\.
   + 

**If the client certificates are not signed by a CA that is registered with AWS IoT**  
See [Register a client certificate signed by an unregistered CA \(console\)](#manual-cert-registration-console-noca)\. 
   + 

**If the client certificates are signed by Amazon's CA**  
Don't select any CA, just choose **Next**\.

   If the client certificates are not signed by a CA that is registered with AWS IoT, see [Register a client certificate signed by an unregistered CA \(console\)](#manual-cert-registration-console-noca)\. 

1.  On **Register existing device certificates**, choose **Select certificates**, and select up to 10 certificate files to register\. 

1.  After closing the file dialog box, select whether you want to activate or revoke the client certificates when you register them\.

   If you don't activate a certificate when it is registered, [Activate a client certificate \(console\)](activate-or-deactivate-device-cert.md#activate-device-cert-console) describes how to activate it later\. 

   If a certificate is revoked when it is registered, it can't be activated later\.

   After you choose the certificate files to register, and select the actions to take after registration, select **Register certificates**\.

The client certificates that are registered successfully appear in the list of certificates\.

## Register a client certificate signed by an unregistered CA \(console\)<a name="manual-cert-registration-console-noca"></a>

**Note**  
Before you perform this procedure, make sure that you have the client certificate's \.pem file\.

**To register an existing certificate with AWS IoT using the console**

1. Sign in to the AWS Management Console and open the [AWS IoT console](https://console.aws.amazon.com/iot/home)\.

1. In the left navigation pane, choose **Secure**, choose **Certificates**, and then choose **Create**\.

1. On **Create a certificate**, locate the **Use my certificate** entry, and choose **Get started**\.

1. On **Select a CA**, choose **Next**\.

1.  On **Register existing device certificates**, choose **Select certificates**, and select up to 10 certificate files to register\. 

1.  After closing the file dialog box, select whether you want to activate or revoke the client certificates when you register them\.

   If you don't activate a certificate when it is registered, [Activate a client certificate \(console\)](activate-or-deactivate-device-cert.md#activate-device-cert-console) describes how to activate it later\. 

   If a certificate is revoked when it is registered, it can't be activated later\.

   After you choose the certificate files to register, and select the actions to take after registration, select **Register certificates**\.

The client certificates that are registered successfully appear in the list of certificates\.

## Register a client certificate signed by a registered CA \(CLI\)<a name="manual-cert-registration-cli"></a>

**Note**  
Before you perform this procedure, make sure that you have the certificate authority \(CA\) \.pem and the client certificate's \.pem file\. The client certificate must be signed by a certificate authority \(CA\) that you have [registered with AWS IoT](register-CA-cert.md)\.

Use the [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/iot/register-certificate.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/iot/register-certificate.html) command to register, but not activate, a client certificate\.

```
aws iot register-certificate \
    --certificate-pem file://device_cert_pem_filename \
    --ca-certificate-pem file://ca_cert_pem_filename
```

The client certificate is registered with AWS IoT, but it is not active yet\. See [Activate a client certificate \(CLI\)](activate-or-deactivate-device-cert.md#activate-device-cert-cli) for information on how to activate it later\.

You can also activate the client certificate when you register it by using this command\.

```
aws iot register-certificate \
    --set-as-active \
    --certificate-pem file://device_cert_pem_filename \
    --ca-certificate-pem file://ca_cert_pem_filename
```

For more information about activating the certificate so that it can be used to connect to AWS IoT, see [Activate or deactivate a client certificate](activate-or-deactivate-device-cert.md)

## Register a client certificate signed by an unregistered CA \(CLI\)<a name="manual-cert-registration-noca-cli"></a>

**Note**  
Before you perform this procedure, make sure that you have the certificate's \.pem file\.

Use the [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/iot/register-certificate-without-ca.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/iot/register-certificate-without-ca.html) command to register, but not activate, a client certificate\.

```
aws iot register-certificate-without-ca \
    --certificate-pem file://device_cert_pem_filename
```

The client certificate is registered with AWS IoT, but it is not active yet\. See [Activate a client certificate \(CLI\)](activate-or-deactivate-device-cert.md#activate-device-cert-cli) for information on how to activate it later\.

You can also activate the client certificate when you register it by using this command\.

```
aws iot register-certificate-without-ca \
    --status ACTIVE \
    --certificate-pem file://device_cert_pem_filename
```

For more information about activating the certificate so that it can be used to connect to AWS IoT, see [Activate or deactivate a client certificate](activate-or-deactivate-device-cert.md)\.