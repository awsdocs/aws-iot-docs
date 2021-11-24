# Generate a presigned request with the WebSocket library<a name="connect-iot-lorawan-network-analyzer-generate-request"></a>

The following shows how you to generate a presigned request so that you can use the WebSocket library to send requests to the service,\.

## Add a policy for WebSocket requests to your IAM role<a name="connect-iot-lorawan-network-analyzer-iam"></a>

To use the WebSocket protocol to call network analyzer, attach the following policy to the AWS Identity and Access Management \(IAM\) role that makes this request\. 

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "iotwireless:StartNetworkAnalyzerStream",
            "Resource": "*"
        }
    ]
}
```

## Create a presigned URL<a name="connect-iot-lorawan-network-analyzer-presigned-url"></a>

Construct a URL for your WebSocket request that contains the information needed to set up communication between your application and the network analyzer\. To verify the identity of the request, WebSocket streaming uses the Amazon Signature Version 4 process for signing requests\. For more information about Signature Version 4, see [ Signing AWS API Requests](https://docs.aws.amazon.com/general/latest/gr/signing_aws_api_requests.html) in the *Amazon Web Services General Reference*\.

To call network analyzer, use the `StartNetworkAnalyzerStream` request URL\. The request will be signed using the credentials for the IAM role mentioned previously\. The URL has the following format with line breaks added for readability\.

```
GET wss://api.iotwireless.<region>.amazonaws.com/start-network-analyzer-stream?X-Amz-Algorithm=AWS4-HMAC-SHA256
   &X-Amz-Credential=Signature Version 4 credential scope
   &X-Amz-Date=date
   &X-Amz-Expires=time in seconds until expiration
   &X-Amz-Security-Token=security-token
   &X-Amz-Signature=Signature Version 4 signature 
   &X-Amz-SignedHeaders=host
```

Use the following values for the Signature Version 4 parameters:
+ **X\-Amz\-Algorithm** – The algorithm you're using in the signing process\. The only valid value is `AWS4-HMAC-SHA256`\.
+ **X\-Amz\-Credential** – A string separated by slashes \("/"\) that is formed by concatenating your access\-key ID and your credential scope components\. Credential scope includes the date in YYYYMMDD format, the AWS Region, the service name, and a termination string \(aws4\_request\)\.
+ **X\-Amz\-Date** – The date and time that the signature was created\. Generate the date and time by following the instructions in [Handling Dates in Signature Version 4](https://docs.aws.amazon.com/general/latest/gr/sigv4-date-handling.html) in the *Amazon Web Services General Reference*\.
+ **X\-Amz\-Expires** – The length of time in seconds until the credentials expire\. The maximum value is 300 seconds \(5 minutes\)\.
+ **X\-Amz\-Security\-Token** – \(optional\) A Signature Version 4 token for temporary credentials\. If you specify this parameter, include it in the canonical request\. For more information, see [Requesting Temporary Security Credentials](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp_request.html) in the *AWS Identity and Access Management User Guide*\.
+ **X\-Amz\-Signature** – The Signature Version 4 signature that you generated for the request\.
+ **X\-Amz\-SignedHeaders** – The headers that are signed when creating the signature for the request\. The only valid value is `host`\.

## Construct the request URL and create Signature Version 4 signature<a name="connect-iot-network-analyzer-construct-url-sign"></a>

To construct the URL for the request and create the Signature Version 4 signature, use the following steps\. The examples are in pseudocode\.

### Task 1: Create a canonical request<a name="connect-iot-lorawan-network-analyzer-canonical-request"></a>

Create a string that includes information from your request in a standardized format\. This ensures that when AWS receives the request, it can calculate the same signature that you calculate in [Task 3: Calculate the signature](#connect-iot-lorawan-network-analyzer-calculate-signature)\. For more information, see [Create a Canonical Request for Signature Version 4](https://docs.aws.amazon.com/general/latest/gr/sigv4-create-canonical-request.html) in the *Amazon Web Services General Reference*\.

1. Define variables for the request in your application\.

   ```
   # HTTP verb
   method = "GET"
   # Service name
   service = "iotwireless"
   # AWS Region
   region = "AWS Region"
   # Service streaming endpoint
   endpoint = "wss://api.iotwireless.region.amazonaws.com"
   # Host
   host = "api.iotwireless.<region>.amazonaws.com"
   # Date and time of request
   amz-date = YYYYMMDD'T'HHMMSS'Z'
   # Date without time for credential scope
   datestamp = YYYYMMDD
   ```

1. Create a canonical URI \(uniform resource identifier\)\. The canonical URI is the part of the URI between the domain and the query string\.

   ```
   canonical_uri = "/start-network-analyzer-stream"
   ```

1. Create the canonical headers and signed headers\. Note the trailing `\n` in the canonical headers\.
   + Append the lowercase header name followed by a colon\.
   + Append a comma\-separated list of values for that header\. Don't sort the values in headers that have multiple values\.
   + Append a new line \(`\n`\)\.

   ```
   canonical_headers = "host:" + host + "\n"
   signed_headers = "host"
   ```

1. Match the algorithm to the hashing algorithm\. You must use SHA\-256\.

   ```
   algorithm = "AWS4-HMAC-SHA256"
   ```

1. Create the credential scope, which scopes the derived key to the date, Region, and service to which the request is made\.

   ```
   credential_scope = datestamp + "/" + region + "/" + service + "/" + "aws4_request"
   ```

1. Create the canonical query string\. Query string values must be URI\-encoded and sorted by name\.
   + Sort the parameter names by character code point in ascending order\. Parameters with duplicate names should be sorted by value\. For example, a parameter name that begins with the uppercase letter F precedes a parameter name that begins with a lowercase letter b\.
   + Do not URI\-encode any of the unreserved characters that [RFC 3986](https://datatracker.ietf.org/doc/html/rfc3986) defines: A–Z, a–z, 0–9, hyphen \( \- \), underscore \( \_ \), period \( \. \), and tilde \( \~ \)\.
   + Percent\-encode all other characters with %XY, where X and Y are hexadecimal characters \(0\-9 and uppercase A\-F\)\. For example, the space character must be encoded as %20 \(not using '\+', as some encoding schemes do\) and extended UTF\-8 characters must be in the form %XY%ZA%BC\.
   + Double\-encode any equals \( = \) characters in parameter values\.

   ```
   canonical_querystring  = "X-Amz-Algorithm=" + algorithm
   canonical_querystring += "&X-Amz-Credential="+ URI-encode(access key + "/" + credential_scope)
   canonical_querystring += "&X-Amz-Date=" + amz_date 
   canonical_querystring += "&X-Amz-Expires=300"
   canonical_querystring += "&X-Amz-Security-Token=" + token
   canonical_querystring += "&X-Amz-SignedHeaders=" + signed_headers
   canonical_querystring += "&language-code=en-US&media-encoding=pcm&sample-rate=16000"
   ```

1. Create a hash of the payload\. For a GET request, the payload is an empty string\.

   ```
   payload_hash = HashSHA256(("").Encode("utf-8")).HexDigest()
   ```

1. Combine all of the elements to create the canonical request\.

   ```
   canonical_request = method + '\n' 
      + canonical_uri + '\n' 
      + canonical_querystring + '\n' 
      + canonical_headers + '\n' 
      + signed_headers + '\n' 
      + payload_hash
   ```

### Task 2: Create the string to sign<a name="connect-iot-lorawan-network-analyzer-create-urlsign"></a>

The string to sign contains meta information about your request\. You use the string to sign in the next step when you calculate the request signature\. For more information, see [Create a String to Sign for Signature Version 4](https://docs.aws.amazon.com/general/latest/gr/sigv4-create-string-to-sign.html) in the *Amazon Web Services General Reference*\.

```
string_to_sign=algorithm + "\n"
   + amz_date + "\n"
   + credential_scope + "\n"
   + HashSHA256(canonical_request.Encode("utf-8")).HexDigest()
```

### Task 3: Calculate the signature<a name="connect-iot-lorawan-network-analyzer-calculate-signature"></a>

You derive a signing key from your AWS secret access key\. For a greater degree of protection, the derived key is specific to the date, service, and AWS Region\. You use the derived key to sign the request\. For more information, see [ Calculate the Signature for AWS Signature Version 4 ](https://docs.aws.amazon.com/general/latest/gr/sigv4-calculate-signature.html) in the *Amazon Web Services General Reference*\.

The code assumes that you have implemented the `GetSignatureKey` function to derive a signing key\. For more information and example functions, see [Examples of How to Derive a Signing Key for Signature Version 4](https://docs.aws.amazon.com/general/latest/gr/signature-v4-examples.html) in the *Amazon Web Services General Reference*\.

The function `HMAC(key, data)` represents an HMAC\-SHA256 function that returns the results in binary format\.

```
#Create the signing key
signing_key = GetSignatureKey(secret_key, datestamp, region, service)
                
# Sign the string_to_sign using the signing key
signature = HMAC.new(signing_key, (string_to_sign).Encode("utf-8"), Sha256()).HexDigest
```

### Task 4: Add signing information to request and create request URL<a name="connect-iot-lorawan-network-analyzer-sign-request"></a>

After you calculate the signature, add it to the query string\. For more information, see [Add the Signature to the Request](https://docs.aws.amazon.com/general/latest/gr/sigv4-add-signature-to-request.html) in the *Amazon Web Services General Reference*\.

```
#Add the authentication information to the query string
canonical_querystring += "&X-Amz-Signature=" + signature
                
# Sign the string_to_sign using the signing key
request_url = endpoint + canonical_uri + "?" + canonical_querystring
```

## Next steps<a name="connect-iot-lorawan-network-analyzer-request-next"></a>

You can now use the request URL with your WebSocket library to make the request to the service and observe the messages\. For more information, see [WebSocket messages and status codes](conect-iot-lorawan-network-analyer-messages-status.md)\.