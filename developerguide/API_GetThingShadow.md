# GetThingShadow<a name="API_GetThingShadow"></a>

Gets the shadow for the specified thing\.

The response state document includes the delta between the `desired` and `reported` states\.

**Request**  
The request includes the standard HTTP headers plus the following URI:

```
HTTP GET https://endpoint/things/thingName/shadow
```

**Response**  
Upon success, the response includes the standard HTTP headers plus the following code and body:

```
HTTP 200
BODY: response state document
```

For more information, see Example Response State Document\.

**Authorization**  
Retrieving a shadow requires a policy that allows the caller to perform the `iot:GetThingShadow` action\. The Device Shadow service accepts two forms of authentication: Signature Version 4 with IAM credentials or TLS mutual authentication with a client certificate\.

The following is an example policy that allows a caller to retrieve a device's shadow:

```
{
    "Version": "2012-10-17",
    "Statement": [{
        "Effect": "Allow",
        "Action": "iot:GetThingShadow",
        "Resource": ["arn:aws:iot:region:account:thing/thing"]
    }]
}
```