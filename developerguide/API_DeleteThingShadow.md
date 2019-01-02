# DeleteThingShadow<a name="API_DeleteThingShadow"></a>

Deletes the shadow for the specified thing\.

**Request**  
The request includes the standard HTTP headers plus the following URI:

```
HTTP DELETE https://endpoint/things/thingName/shadow
```

**Response**  
Upon success, the response includes the standard HTTP headers plus the following code and body:

```
HTTP 200
BODY: Empty response state document
```

**Authorization**  
Deleting a device's shadow requires a policy that allows the caller to perform the `iot:DeleteThingShadow` action\. The Device Shadow service accepts two forms of authentication: Signature Version 4 with IAM credentials or TLS mutual authentication with a client certificate\.

The following is an example policy that allows a caller to delete a device's shadow:

```
{
    "Version": "2012-10-17",
    "Statement": [{
        "Effect": "Allow",
        "Action": "iot:DeleteThingShadow",
        "Resource": ["arn:aws:iot:region:account:thing/thing"]
    }]
}
```