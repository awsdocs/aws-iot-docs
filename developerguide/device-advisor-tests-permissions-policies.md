# Permissions and policies<a name="device-advisor-tests-permissions-policies"></a>

You can use the following tests to determine if the policies attached to your devices’ certificates follow standard best practices\.

**"Device certificate attached policies don’t contain wildcards"**  
 Validates if the permission policies associated with a device follow best practices and do not grant the device more permissions than needed\.  
*API test case definition:*  
`EXECUTION_TIMEOUT` has a default value of 1 minute\. We recommend setting a timeout of at least 30 seconds\.

```
"tests":[
   {
        "name":"my_security_device_policies",
        "configuration": {
            // optional:
            "EXECUTION_TIMEOUT":"60"    // in seconds
        },
        "test": {
            "id": "Security_Device_Policies",
            "version": "0.0.0"
        }
    }
]
```