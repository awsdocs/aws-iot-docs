# UpdateThingShadow<a name="API_UpdateThingShadow"></a>

Updates the shadow for the specified thing\.

Updates affect only the fields specified in the request state document\. Any field with a value of `null` is removed from the device's shadow\.

**Request**  
The request includes the standard HTTP headers plus the following URI and body:

```
HTTP POST https://endpoint/things/thingName/shadow
BODY: request state document
```

For more information, see [Example Request State Document](device-shadow-document-syntax.md#device-shadow-example-request-json)\.

**Response**  
Upon success, the response includes the standard HTTP headers plus the following code and body:

```
HTTP 200
BODY: response state document
```

For more information, see [Example Response State Document](device-shadow-document-syntax.md#device-shadow-example-response-json)\.

**Authorization**  
Updating a shadow requires a policy that allows the caller to perform the `iot:UpdateThingShadow` action\. The Device Shadow service accepts two forms of authentication: Signature Version 4 with IAM credentials or TLS mutual authentication with a client certificate\.

The following is an example policy that allows a caller to update a device's shadow:

```
{
    "Version": "2012-10-17",
    "Statement": [{
        "Effect": "Allow",
        "Action": "iot:UpdateThingShadow",
        "Resource": ["arn:aws:iot:region:account:thing/thing"]
    }]
}
```