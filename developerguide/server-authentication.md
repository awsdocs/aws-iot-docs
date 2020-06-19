# Server Authentication<a name="server-authentication"></a>

When your device or other client attempts to connect to AWS IoT Core, the AWS IoT Core server will send an X\.509 certificate that your device uses to authenticate the server\. Authentication takes place at the TLS layer through validation of the [ X\.509 certificate chain](https://docs.aws.amazon.com/iot/latest/developerguide/x509-client-certs.html) This is the same method used by your browser when you visit an HTTPS URL\.

When your devices or other clients establish a TLS connection to an AWS IoT Core endpoint, AWS IoT Core presents a certificate chain that the devices use to verify that they're communicating with AWS IoT Core and not another server impersonating AWS IoT Core\. The chain that is presented depends on a combination of the type of endpoint the device is connecting to and the [cipher suite](https://docs.aws.amazon.com/iot/latest/developerguide/transport-security.html) that the client and AWS IoT Core negotiated during the TLS handshake\.

## Endpoint Types<a name="endpoint-types"></a>

AWS IoT Core supports two different data endpoint types, `iot:Data` and `iot:Data-ATS`\. `iot:Data` endpoints present a certificate signed by the [VeriSign Class 3 Public Primary G5 root CA certificate](https://www.websecurity.digicert.com/content/dam/websitesecurity/digitalassets/desktop/pdfs/roots/VeriSign-Class%203-Public-Primary-Certification-Authority-G5.pem)\. `iot:Data-ATS` endpoints present a server certificate signed by an [Amazon Trust Services](https://www.amazontrust.com/repository/) CA\.

Certificates presented by ATS endpoints are cross signed by Starfield\. Some TLS client implementations require validation of the root of trust and require that the Starfield CA certificates are installed in the client's trust stores\.

**Warning**  
Using a method of certificate pinning that hashes the whole certificate \(including the issuer name, and so on\) is not recommended because this will cause certificate verification to fail because the ATS certificates we provide are cross signed by Starfield and have a different issuer name\.

Use `iot:Data-ATS` endpoints unless your device requires Symantec or Verisign CA certificates\. Symantec and Verisign certificates have been deprecated and are no longer supported by most web browsers\.

You can use the `describe-endpoint` command to create your ATS endpoint\.

```
aws iot describe-endpoint --endpoint-type iot:Data-ATS
```

The `describe-endpoint` command returns an endpoint in the following format\.

```
account-specific-prefix.iot.your-region.amazonaws.com
```

The first time `describe-endpoint` is called, an endpoint is created\. All subsequent calls to `describe-endpoint` return the same endpoint\.

For backward\-compatibility, AWS IoT Core still supports Symantec endpoints\. For more information, see [How AWS IoT Core is Helping Customers Navigate the Upcoming Distrust of Symantec Certificate Authorities](https://aws.amazon.com/blogs/iot/aws-iot-core-ats-endpoints)\. Devices operating on ATS endpoints are fully interoperable with devices operating on Symantec endpoints in the same account and do not require any re\-registration\.

**Note**  
To see your `iot:Data-ATS` endpoint in the AWS IoT Core console, choose **Settings**\. The console displays only the `iot:Data-ATS` endpoint\. By default, the `describe-endpoint` command displays the `iot:Data` endpoint for backward compatibility\. To see the `iot:Data-ATS` endpoint, specify the `--endpointType` parameter, as in the previous example\.

### Creating an `IotDataPlaneClient` with the AWS SDK for Java<a name="java-client"></a>

By default, the [AWS SDK for Java \- Version 2](https://github.com/aws/aws-sdk-java-v2) creates an `IotDataPlaneClient` by using an `iot:Data` endpoint\. To create a client that uses an `iot:Data-ATS` endpoint, you must do the following\. 
+ Create an `iot:Data-ATS` endpoint by using the [DescribeEndpoint](https://docs.aws.amazon.com/iot/latest/apireference/API_DescribeEndpoint.html) API\.
+ Specify that endpoint when you create the `IotDataPlaneClient`\.

The following example performs both of these operations\.

```
public void setup() throws Exception {
        IotClient client = IotClient.builder().credentialsProvider(CREDENTIALS_PROVIDER_CHAIN).region(Region.US_EAST_1).build();
        String endpoint = client.describeEndpoint(r -> r.endpointType("iot:Data-ATS")).endpointAddress();
        iot = IotDataPlaneClient.builder()
                                .credentialsProvider(CREDENTIALS_PROVIDER_CHAIN)
                                .endpointOverride(URI.create("https://" + endpoint))
                                .region(Region.US_EAST_1)
                                .build();
}
```

## CA Certificates for Server Authentication<a name="server-authentication-certs"></a>

Depending on which type of data endpoint you are using and which cipher suite you have negotiated, AWS IoT Core server authentication certificates are signed by one of the following root CA certificates:

**VeriSign Endpoints \(legacy\)**
+ RSA 2048 bit key: [VeriSign Class 3 Public Primary G5 root CA certificate](https://www.websecurity.digicert.com/content/dam/websitesecurity/digitalassets/desktop/pdfs/roots/VeriSign-Class%203-Public-Primary-Certification-Authority-G5.pem)

**Amazon Trust Services Endpoints \(preferred\)**
+ RSA 2048 bit key: [Amazon Root CA 1](https://www.amazontrust.com/repository/AmazonRootCA1.pem)\.
+ RSA 4096 bit key: Amazon Root CA 2\. Reserved for future use\.
+ ECC 256 bit key: [Amazon Root CA 3](https://www.amazontrust.com/repository/AmazonRootCA3.pem)\.
+ ECC 384 bit key: Amazon Root CA 4\. Reserved for future use\.

These certificates are all cross\-signed by the [ Starfield Root CA Certificate](https://www.amazontrust.com/repository/SFSRootCAG2.pem)\. All new AWS IoT Core regions, beginning with the May 9, 2018 launch of AWS IoT Core in the Asia Pacific \(Mumbai\) Region, serve only ATS certificates\.

## Server Authentication Guidelines<a name="server-authentication-guidelines"></a>

There are many variables that can affect a device's ability to validate the AWS IoT Core server authentication certificate\. For example, devices may be too memory constrained to hold all possible root CA certificates, or devices may implement a non\-standard method of certificate validation\. For these reasons we suggest following these guidelines:
+ We recommend that you use your ATS endpoint and install all supported Amazon Root CA certificates\.
+ If you cannot store all of these certificates on your device and if your devices do not use ECC\-based validation, you can omit the [Amazon Root CA 3](https://www.amazontrust.com/repository/AmazonRootCA3.pem) and [Amazon Root CA 4](https://www.amazontrust.com/repository/AmazonRootCA4.pem) ECC certificates\. If your devices do not implement RSA\-based certificate validation, you can omit the [Amazon Root CA 1](https://www.amazontrust.com/repository/AmazonRootCA1.pem) and [Amazon Root CA 2](https://www.amazontrust.com/repository/AmazonRootCA2.pem) RSA certificates\.
+ If you are experiencing server certificate validation issues when connecting to your ATS endpoint, try adding the relevant cross\-signed Amazon Root CA certificate to your trust store\.
  + [Cross\-signed Amazon Root CA 1](https://www.amazontrust.com/repository/G2-RootCA1.pem)
  + [Cross\-signed Amazon Root CA 2](https://www.amazontrust.com/repository/G2-RootCA2.pem) \- Reserved for future use\.
  + [Cross\-signed Amazon Root CA 3](https://www.amazontrust.com/repository/G2-RootCA3.pem)
  + [Cross\-signed Amazon Root CA 4 \- Reserved for future use\.](https://www.amazontrust.com/repository/G2-RootCA4.pem)
+ If you are experiencing server certificate validation issues, your device may need to explicitly trust the root CA\. Try adding the [Starfield Root CA Certificate](https://www.amazontrust.com/repository/SFSRootCAG2.pem) to your trust store\.
+ If you still experience issues after executing the steps above, please contact [AWS Developer Support](https://aws.amazon.com/premiumsupport/plans/developers/)\. 

**Note**  
CA certificates have an expiration date after which they cannot be used to validate a server's certificate\. CA certificates might have to be replaced before their expiration date\. Make sure that you can update the root CA certificates on all of your devices to ensure ongoing connectivity and to keep up\-to\-date with security best practices\.

**Note**  
When connecting to AWS IoT Core in your device code, pass the certificate into the API you are using to connect\. The API you use will vary by SDK\. For more information, see the [AWS IoT Core Device SDKs](https://docs.aws.amazon.com/iot/latest/developerguide/iot-sdks.html)\.