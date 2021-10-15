# Connecting to AWS IoT FIPS endpoints<a name="iot-connect-fips"></a>

AWS IoT provides endpoints that support the [Federal Information Processing Standard \(FIPS\) 140\-2](http://aws.amazon.com/compliance/fips/)\. FIPS compliant endpoints are different from standard AWS endpoints\. To interact with AWS IoT in a FIPS\-compliant manner, you must use the endpoints described below with your FIPS compliant client\. The AWS IoT console is not FIPS compliant\.

The following sections describe how to access the FIPS compliant AWS IoT endpoints by using the REST API, an SDK, or the \.AWS CLI\.

## AWS IoT Core \- control plane endpoints<a name="iot-connect-fips-control"></a>

The FIPS compliant **AWS IoT Core \- control plane** endpoints that support the [AWS IoT](https://docs.aws.amazon.com/iot/latest/apireference/API_Operations_AWS_IoT.html) operations and their related [CLI commands](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/iot/index.html) are listed in [FIPS Endpoints by Service](http://aws.amazon.com/compliance/fips/#FIPS_Endpoints_by_Service)\. In [FIPS Endpoints by Service](http://aws.amazon.com/compliance/fips/#FIPS_Endpoints_by_Service), find the **AWS IoT Core \- control plane** service, and look up the endpoint for your AWS Region\.

To use the FIPS compliant endpoint when you access the [AWS IoT](https://docs.aws.amazon.com/iot/latest/apireference/API_Operations_AWS_IoT.html) operations, use the AWS SDK or the REST API with the endpoint that is appropriate for your AWS Region\.

To use the FIPS compliant endpoint when you run [aws iot CLI commands](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/iot/index.html), add the \-\-endpoint parameter with the appropriate endpoint for your AWS Region to the command\. 

## AWS IoT Core \- data plane endpoints<a name="iot-connect-fips-data"></a>

The FIPS compliant **AWS IoT Core \- data plane** endpoints are listed in [FIPS Endpoints by Service](http://aws.amazon.com/compliance/fips/#FIPS_Endpoints_by_Service)\. In [FIPS Endpoints by Service](http://aws.amazon.com/compliance/fips/#FIPS_Endpoints_by_Service), find the **AWS IoT Core \- data plane** service, and look up the endpoint for your AWS Region\.

You can use the FIPS compliant endpoint for your AWS Region with a FIPS compliant client by using the AWS IoT Device SDK and providing the endpoint to the SDK's connection function in place of your account's default **AWS IoT Core \- data plane** endpoint\. The connection function is specific to the AWS IoT Device SDK\. For an example of a connection function, see the [Connection function in the AWS IoT Device SDK for Python](https://aws.github.io/aws-iot-device-sdk-python-v2/awsiot/mqtt_connection_builder.html)\.

**Note**  
AWS IoT doesn't support AWS account\-specific **AWS IoT Core \- data plane** endpoints that are FIPS\-compliant\. Service features that require an AWS account\-specific endpoint in the [Server Name Indication \(SNI\)](transport-security.md) can't be used\. FIPS\-compliant **AWS IoT Core \- data plane** endpoints can't support [Multi\-Account Registration Certificates](x509-client-certs.md#multiple-account-cert), [Custom Domains](iot-custom-endpoints-configurable-custom.md), and [Custom Authorizers](custom-authentication.md)\.

## AWS IoT Device Management \- jobs data endpoints<a name="iot-connect-fips-jobs"></a>

The FIPS compliant **AWS IoT Device Management \- jobs data** endpoints are listed in [FIPS Endpoints by Service](http://aws.amazon.com/compliance/fips/#FIPS_Endpoints_by_Service)\. In [FIPS Endpoints by Service](http://aws.amazon.com/compliance/fips/#FIPS_Endpoints_by_Service), find the **AWS IoT Device Management \- jobs data** service, and look up the endpoint for your AWS Region\.

To use the FIPS compliant **AWS IoT Device Management \- jobs data** endpoint when you run [aws iot\-jobs\-data CLI commands](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/iot-jobs-data/index.html), add the \-\-endpoint parameter with the appropriate endpoint for your AWS Region to the command\. You can also use the REST API with this endpoint\.

You can use the FIPS compliant endpoint for your AWS Region with a FIPS compliant client by using the AWS IoT Device SDK and providing the endpoint to the SDK's connection function in place of your account's default **AWS IoT Device Management \- jobs data** endpoint\. The connection function is specific to the AWS IoT Device SDK\. For an example of a connection function, see the [Connection function in the AWS IoT Device SDK for Python](https://aws.github.io/aws-iot-device-sdk-python-v2/awsiot/mqtt_connection_builder.html)\.

## AWS IoT Device Management \- Fleet Hub endpoints<a name="iot-connect-fips-fleethub"></a>

The FIPS compliant **AWS IoT Device Management \- Fleet Hub** endpoints to use with [Fleet Hub for AWS IoT Device Management](https://docs.aws.amazon.com/iot/latest/fleethubuserguide/what-is-aws-iot-monitor.html) [CLI commands](https://docs.aws.amazon.com/cli/latest/reference/iotfleethub/index.html) are listed in [FIPS Endpoints by Service](http://aws.amazon.com/compliance/fips/#FIPS_Endpoints_by_Service)\. In [FIPS Endpoints by Service](http://aws.amazon.com/compliance/fips/#FIPS_Endpoints_by_Service), find the **AWS IoT Device Management \- Fleet Hub** service, and look up the endpoint for your AWS Region\.

To use the FIPS compliant **AWS IoT Device Management \- Fleet Hub** endpoint when you run [aws iotfleethub CLI commands](https://docs.aws.amazon.com/cli/latest/reference/iotfleethub/index.html), add the \-\-endpoint parameter with the appropriate endpoint for your AWS Region to the command\. You can also use the REST API with this endpoint\.

## AWS IoT Device Management \- secure tunneling endpoints<a name="iot-connect-fips-tunnel"></a>

The FIPS compliant **AWS IoT Device Management \- secure tunneling** endpoints for the [AWS IoT secure tunneling API](https://docs.aws.amazon.com/iot/latest/apireference/API_Operations_AWS_IoT_Secure_Tunneling.html) and the corresponding [CLI commands](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/iotsecuretunneling/index.html) are listed in [FIPS Endpoints by Service](http://aws.amazon.com/compliance/fips/#FIPS_Endpoints_by_Service)\. In [FIPS Endpoints by Service](http://aws.amazon.com/compliance/fips/#FIPS_Endpoints_by_Service), find the **AWS IoT Device Management \- secure tunneling** service, and look up the endpoint for your AWS Region\.

To use the FIPS compliant **AWS IoT Device Management \- secure tunneling** endpoint when you run [aws iotsecuretunneling CLI commands](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/iotsecuretunneling/index.html), add the \-\-endpoint parameter with the appropriate endpoint for your AWS Region to the command\. You can also use the REST API with this endpoint\.