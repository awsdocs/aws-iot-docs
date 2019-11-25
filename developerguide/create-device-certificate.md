# Create and Activate a Device Certificate<a name="create-device-certificate"></a>

Communication between your device and AWS IoT is protected through the use of X\.509 certificates\. AWS IoT can generate a certificate for you or you can use your own X\.509 certificate\. In this tutorial, AWS IoT generates the X\.509 certificate for you\. Certificates must be activated prior to use\.

1. Choose **Create certificate**\.  
![\[Add a certificate for your thing\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/sdk-create-cert.png)

1. On the **Certificate created\!** page, choose **Download** for the certificate, private key, and the root CA for AWS IoT \(the public key need not be downloaded\)\. Save each of them to your computer, and then choose **Activate** to continue\.
**Note**  
The root CA for AWS IoT **Download** link takes you to the [X\.509 Client Certificates](x509-client-certs.md) page where you choose a CA certificate\. Unlike the other **Download** links on the page, it doesn't directly download a file\.  
![\[Certificate created!\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/sdk-attach-policy.png)

   Be aware that the downloaded file names might be different from those listed on the **Certificate created** page\. For example: 
   + 2a540e2346\-certificate\.pem\.crt
   + 2a540e2346\-private\.pem\.key
   + 2a540e2346\-public\.pem\.key
**Note**  
Although it is unlikely, root CA certificates are subject to expiration and revocation\. If this should occur, you must copy a new root CA certificate onto your device\.

1. Choose **Done** to return to the main page of the AWS IoT console\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/main-console-things.png)

When working with a device, you must copy the private key and root CA certificate onto your device\. The instructions in this guide are written with the assumption that you are not using a device and are simply getting familiar with the AWS IoT console\.