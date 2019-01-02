# Create and Activate a Device Certificate<a name="create-device-certificate"></a>

Communication between your device and AWS IoT is protected through the use of X\.509 certificates\. AWS IoT can generate a certificate for you or you can use your own X\.509 certificate\. In this tutorial, AWS IoT generates the X\.509 certificate for you\. Certificates must be activated prior to use\.

1. Choose **Create certificate**\.  
![\[Image NOT FOUND\]](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/images/certificate-create2.png)

1. On the **Certificate created\!** page, choose **Download** for the certificate, private key, and the root CA for AWS IoT \(the public key need not be downloaded\)\. Save each of them to your computer, and then choose **Activate** to continue\.  
![\[Image NOT FOUND\]](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/images/certificate-created.png)

   Be aware that the downloaded filenames may be different than those listed on the **Certificate created\!** page\. For example: 

   + 2a540e2346\-certificate\.pem\.crt

   + 2a540e2346\-private\.pem\.key

   + 2a540e2346\-public\.pem\.key
**Note**  
Although it is unlikely, root CA certificates are subject to expiration and/or revocation\. If this should occur, you must copy new a root CA certificate onto your device\.

1.  Choose the back arrow until you have returned to the main **AWS IoT** console screen\.   
![\[Image NOT FOUND\]](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/images/main-console-things.png)