# Metrics<a name="detect-metrics"></a>

------
#### [ aws:message\-byte\-size ]

The number of bytes in a message\.

------
#### [ More info \(1\) ]

Use this metric to specify the maximum or minimum size \(in bytes\) of each message transmitted from a device to AWS IoT\.

Source: cloud\-side

Operators: less\-than \| less\-than\-equals \| greater\-than \| greater\-than\-equals 

Value: a non\-negative integer 

Units: bytes 

**Example**  

```
{
  "name": "Max Message Size",
  "metric": "aws:message-byte-size",
  "criteria": {
    "comparisonOperator": "less-than",
    "value": {
      "count": 1024
    },
    "consecutiveDatapointsToAlarm": 3,
    "consecutiveDatapointsToClear": 3
  }
}
```

**Example using a `statisticalThreshold`**  

```
{
  "name": "Large Message Size",
  "metric": "aws:message-byte-size",
  "criteria": {
    "comparisonOperator": "less-than",
    "statisticalThreshold": {
      "statistic": "p90"
    },
    "durationSeconds": 300,
    "consecutiveDatapointsToAlarm": 3,
    "consecutiveDatapointsToClear": 3
  }
}
```

An alarm occurs for a device if, during three consecutive five\-minute periods, it transmits messages whose cumulative size is more than that measured for 90 percent of all other devices reporting for this security profile behavior\.

------

 

------
#### [ aws:num\-messages\-received/aws:num\-messages\-sent ]

The number of messages received or sent by a device during a given time period\.

------
#### [ More info \(2\) ]

Use this metric to specify the maximum or minimum number of messages that can be sent or received between AWS IoT and each device in a given period of time\.

Source: cloud\-side

Operators: less\-than \| less\-than\-equals \| greater\-than \| greater\-than\-equals 

Value: a non\-negative integer 

Units: messages 

Duration: a non\-negative integer, valid values are 300, 600, 900, 1800 or 3600 seconds

**Example**  

```
{
  "name": "Out bound message count",
  "metric": "aws:num-messages-sent",
  "criteria": {
    "comparisonOperator": "less-than",
    "value": {
      "count": 50
    },
    "durationSeconds": 300,
    "consecutiveDatapointsToAlarm": 2,
    "consecutiveDatapointsToClear": 2
  }
}
```

**Example using a `statisticalThreshold`**  

```
{
  "name": "Out bound message rate",
  "metric": "aws:num-messages-sent",
  "criteria": {
    "comparisonOperator": "less-than",
    "statisticalThreshold": {
      "statistic": "p99"
    },
    "durationSeconds": 300,
    "consecutiveDatapointsToAlarm": 2,
    "consecutiveDatapointsToClear": 2
  }
}
```

------

 

------
#### [ aws:all\-bytes\-out ]

The number of outbound bytes from a device during a given time period\.

------
#### [ More info \(3\) ]

Use this metric to specify the maximum or minimum amount of outbound traffic that a device should send, measured in bytes in a given period of time\.

Source: device\-side

Operators: less\-than \| less\-than\-equals \| greater\-than \| greater\-than\-equals 

Value: a non\-negative integer 

Units: bytes 

Duration: a non\-negative integer, valid values are 300, 600, 900, 1800 or 3600 seconds

**Example**  

```
{
  "name": "TCP outbound traffic",
  "metric": "aws:all-bytes-out",
  "criteria": {
    "comparisonOperator": "less-than",
    "value": {
      "count": 4096
    },
    "durationSeconds": 300,
    "consecutiveDatapointsToAlarm": 5,
    "consecutiveDatapointsToClear": 4
  }
}
```

**Example using a `statisticalThreshold`**  

```
{
  "name": "TCP outbound traffic",
  "metric": "aws:all-bytes-out",
  "criteria": {
    "comparisonOperator": "less-than",
    "statisticalThreshold": {
      "statistic": "p50"
    },
    "durationSeconds": 900,
    "consecutiveDatapointsToAlarm": 5,
    "consecutiveDatapointsToClear": 4
  }
}
```

------

 

------
#### [ aws:all\-bytes\-in ]

The number of inbound bytes to a device during a given time period\.

------
#### [ More info \(4\) ]

Use this metric to specify the maximum or minimum amount of inbound traffic that a device should receive, measured in bytes in a given period of time\.

Source: device\-side

Operators: less\-than \| less\-than\-equals \| greater\-than \| greater\-than\-equals 

Value: a non\-negative integer 

Units: bytes 

Duration: a non\-negative integer, valid values are 300, 600, 900, 1800 or 3600 seconds

**Example**  

```
{
  "name": "TCP inbound traffic",
  "metric": "aws:all-bytes-in",
  "criteria": {
    "comparisonOperator": "less-than",
    "value": {
      "count": 4096
    },
    "durationSeconds": 300,
    "consecutiveDatapointsToAlarm": 1,
    "consecutiveDatapointsToClear": 3
  }
}
```

**Example using a `statisticalThreshold`**  

```
{
  "name": "TCP inbound traffic",
  "metric": "aws:all-bytes-in",
  "criteria": {
    "comparisonOperator": "less-than",
    "statisticalThreshold": {
      "statistic": "p90"
    },
    "durationSeconds": 300,
    "consecutiveDatapointsToAlarm": 1,
    "consecutiveDatapointsToClear": 3
  }
}
```

------

 

------
#### [ aws:all\-packets\-out ]

The number of outbound packets from a device during a given time period\.

------
#### [ More info \(5\) ]

Use this metric to specify the maximum or minimum amount of total outbound traffic that a device should send in a given period of time\.

Source: device\-side

Operators: less\-than \| less\-than\-equals \| greater\-than \| greater\-than\-equals 

Value: a non\-negative integer 

Units: packets 

Duration: a non\-negative integer\. Valid values are 300, 600, 900, 1800 or 3600 seconds\.

**Example**  

```
{
  "name": "TCP outbound traffic",
  "metric": "aws:all-packets-out",
  "criteria": {
    "comparisonOperator": "less-than",
    "value": {
      "count": 100
    },
    "durationSeconds": 300,
    "consecutiveDatapointsToAlarm": 1,
    "consecutiveDatapointsToClear": 3
  }
}
```

**Example using a `statisticalThreshold`**  

```
{
  "name": "TCP outbound traffic",
  "metric": "aws:all-packets-out",
  "criteria": {
    "comparisonOperator": "less-than",
    "statisticalThreshold": {
      "statistic": "p90"
    },
    "durationSeconds": 300,
    "consecutiveDatapointsToAlarm": 1,
    "consecutiveDatapointsToClear": 3
  }
}
```

:

------

 

------
#### [ aws:all\-packets\-in ]

The number of inbound packets to a device during a given time period\.

------
#### [ More info \(6\) ]

Use this metric to specify the maximum or minimum amount of total inbound traffic that a device should receive in a given period of time\.

Source: device\-side

Operators: less\-than \| less\-than\-equals \| greater\-than \| greater\-than\-equals 

Value: a non\-negative integer 

Units: packets 

Duration: a non\-negative integer\. Valid values are 300, 600, 900, 1800 or 3600 seconds\.

**Example**  

```
{
  "name": "TCP inbound traffic",
  "metric": "aws:all-packets-in",
  "criteria": {
    "comparisonOperator": "less-than",
    "value": {
      "count": 100
    },
    "durationSeconds": 300,
    "consecutiveDatapointsToAlarm": 2,
    "consecutiveDatapointsToClear": 1
  }
}
```

**Example**  
Example using a `statisticalThreshold`  

```
{
  "name": "TCP inbound traffic",
  "metric": "aws:all-packets-in",
  "criteria": {
    "comparisonOperator": "less-than",
    "statisticalThreshold": {
      "statistic": "p90"
    },
    "durationSeconds": 300,
    "consecutiveDatapointsToAlarm": 2,
    "consecutiveDatapointsToClear": 1
  }
}
```

:

------

 

------
#### [ aws:num\-authorization\-failures ]

The number of authorization failures during a given time period\.

------
#### [ More info \(7\) ]

Use this metric to specify the maximum number of authorization failures allowed for each device in a given period of time\. An authorization failure occurs when a request from a device to AWS IoT is denied \(for example, if a device attempts to publish to a topic for which it does not have sufficient permissions\)\. 

Source: cloud\-side

Unit: failures 

Operators: less\-than \| less\-than\-equals \| greater\-than \| greater\-than\-equals 

Value: a non\-negative integer 

Units: failures 

Duration: a non\-negative integer\. Valid values are 300, 600, 900, 1800, or 3600 seconds\.

**Example**  

```
{
  "name": "Authorization Failures",
  "metric": "aws:num-authorization-failures",
  "criteria": {
    "comparisonOperator": "less-than",
    "value": {
      "count": 5
    },
    "durationSeconds": 300,
    "consecutiveDatapointsToAlarm": 2,
    "consecutiveDatapointsToClear": 1
  }
}
```

**Example using a `statisticalThreshold`**  

```
{
  "name": "Authorization Failures",
  "metric": "aws:num-authorization-failures",
  "criteria": {
    "comparisonOperator": "less-than",
    "statisticalThreshold": {
      "statistic": "p50"
    },
    "durationSeconds": 300,
    "consecutiveDatapointsToAlarm": 2,
    "consecutiveDatapointsToClear": 1
  }
}
```

:

------

 

------
#### [ aws:source\-ip\-address ]

The IP address from which a device has connected to AWS IoT\.

------
#### [ More info \(8\) ]

Use this metric to specify a set of allowed \(formerly referred to as whitelisted\) or denied \(formerly referred to as blacklisted\) CIDRs from which each device must or must not connect to AWS IoT\.

Source: cloud\-side

Operators: in\-cidr\-set \| not\-in\-cidr\-set 

Values: a list of CIDRs

Units: n/a

**Example**  

```
{
  "name": "Denied source IPs",
  "metric": "aws:source-ip-address",
  "criteria": {
    "comparisonOperator": "not-in-cidr-set",
    "value": {
      "cidrs": [ "12.8.0.0/16", "15.102.16.0/24" ]
    }
  }
}
```

------

 

------
#### [ aws:destination\-ip\-addresses ]

A set of IP destinations\.

------
#### [ More info \(9\) ]

Use this metric to specify a set of allowed \(formerly referred to as whitelisted\) or denied \(formerly referred to as blacklisted\) CIDRs with which each device must or must not communicate\.

Source: device\-side

Operators: in\-cidr\-set \| not\-in\-cidr\-set 

Values: a list of CIDRs

Units: n/a

Example:

**Example**  

```
{
  "name": "Denied destination IPs",
  "metric": "aws:destination-ip-addresses",
  "criteria": {
    "comparisonOperator": "not-in-cidr-set",
    "value": {
      "cidrs": [ "12.8.0.0/16", "15.102.16.0/24" ]
    }
  }
}
```

------

 

------
#### [ aws:listening\-tcp\-ports / aws:listening\-udp\-ports ]

The TCP or UDP ports that the device is listening on\.

------
#### [ More info \(10\) ]

Use this metric to specify a set of allowed \(formerly referred to as whitelisted\) or denied \(formerly referred to as blacklisted\) TCP/UDP ports that each device must or must not listen on\.

Source: device\-side

Operators: in\-port\-set \| not\-in\-port\-set 

Values: a list of ports 

Units: n/a

Example:

```
{
  "name": "Listening TCP Ports",
  "metric": "aws:listening-tcp-ports",
  "criteria": {
    "comparisonOperator": "in-port-set",
    "value": {
      "ports": [ 443, 80 ]
    }
  }
}
```

------

 

------
#### [ aws:num\-listening\-tcp\-ports / aws:num\-listening\-udp\-ports ]

The number of TCP or UDP ports the device is listening on\.

------
#### [ More info \(11\) ]

Use this metric to specify the maximum or minimum number of TCP or UDP ports that each device should listen on\.

Source: device\-side

Operators: less\-than \| less\-than\-equals \| greater\-than \| greater\-than\-equals 

Value: a non\-negative integer 

Units: ports 

**Example**  
Example:

**Example using a `statisticalThreshold`**  

```
{
  "name": "Max TCP Ports",
  "metric": "aws:num-listening-tcp-ports",
  "criteria": {
    "comparisonOperator": "less-than-equals",
    "statisticalThreshold": {
      "statistic": "p90"
    },
    "durationSeconds": 300,
    "consecutiveDatapointsToAlarm": 2,
    "consecutiveDatapointsToClear": 1
  }
}
```

:

------

 

------
#### [ aws:num\-established\-tcp\-connections ]

The number of TCP connections for a device\.

------
#### [ More info \(12\) ]

Use this metric to specify the maximum or minimum number of active TCP connections that each device should have\. \(All TCP states\) 

Source: device\-side

Operators: less\-than \| less\-than\-equals \| greater\-than \| greater\-than\-equals 

Value: a non\-negative integer 

Units: connections

**Example**  

```
{
  "name": "TCP Connection Count",
  "metric": "aws:num-established-tcp-connections",
  "criteria": {
    "comparisonOperator": "less-than",
    "value": {
      "count": 3
    },
    "consecutiveDatapointsToAlarm": 3,
    "consecutiveDatapointsToClear": 3
  }
}
```

**Example using a `statisticalThreshold`**  

```
{
  "name": "TCP Connection Count",
  "metric": "aws:num-established-tcp-connections",
  "criteria": {
    "comparisonOperator": "less-than",
    "statisticalThreshold": {
      "statistic": "p90"
    },
    "durationSeconds": 900,
    "consecutiveDatapointsToAlarm": 3,
    "consecutiveDatapointsToClear": 3
  }
}
```

------

 

------
#### [ aws:num\-connection\-attempts ]

The number of times a device attempts to make a connection in a given time period\.

------
#### [ More info \(13\) ]

Use this metric to specify the maximum or minimum number of connection attempts for each device\. Successful and unsuccessful attempts are counted\.

Source: cloud\-side

Operators: less\-than \| less\-than\-equals \| greater\-than \| greater\-than\-equals 

Value: a non\-negative integer 

Units: connection attempts

Duration: a non\-negative integer\. Valid values are 300, 600, 900, 1800, or 3600 seconds\.

**Example**  

```
{
  "name": "Connection Attempts",
  "metric": "aws:num-connection-attempts",
  "criteria": {
    "comparisonOperator": "greater-than",
    "value": {
      "count": 5
    },
    "durationSeconds": 600,
    "consecutiveDatapointsToAlarm": 1,
    "consecutiveDatapointsToClear": 2
  }
}
```

**Example using a `statisticalThreshold`**  

```
{
  "name": "Connection Attempts",
  "metric": "aws:num-connection-attempts",
  "criteria": {
    "comparisonOperator": "greater-than",
    "statisticalThreshold": {
      "statistic": "p10"
    },
    "durationSeconds": 300,
    "consecutiveDatapointsToAlarm": 1,
    "consecutiveDatapointsToClear": 2
  }
}
```

------

 

------
#### [ aws:num\-disconnects ]

The number of times a device disconnects from AWS IoT during a given time period\.

------
#### [ More info \(14\) ]

Use this metric to specify the maximum or minimum number of times a device disconnected from AWS IoT during a given time period\.

Source: cloud\-side

Operators: less\-than \| less\-than\-equals \| greater\-than \| greater\-than\-equals 

Value: a non\-negative integer 

Units: disconnects

Duration: a non\-negative integer\. Valid values are 300, 600, 900, 1800, or 3600 seconds\.

**Example**  

```
{
  "name": "Disconnections",
  "metric": "aws:num-disconnects",
  "criteria": {
    "comparisonOperator": "greater-than",
    "value": {
      "count": 5
    },
    "durationSeconds": 600,
    "consecutiveDatapointsToAlarm": 1,
    "consecutiveDatapointsToClear": 2
  }
}
```

**Example using a `statisticalThreshold`**  

```
{
  "name": "Disconnections",
  "metric": "aws:num-disconnects",
  "criteria": {
    "comparisonOperator": "greater-than",
    "statisticalThreshold": {
      "statistic": "p10"
    },
    "durationSeconds": 300,
    "consecutiveDatapointsToAlarm": 1,
    "consecutiveDatapointsToClear": 2
  }
}
```

------

## Device metrics document specification<a name="DetectMetricsMessagesSpec"></a>


**Overall structure**  

|  Long name  |  Short name  |  Required  |  Type  |  Constraints  |  Notes  | 
| --- | --- | --- | --- | --- | --- | 
|  header  |  hed  |  Y  |  Object  |   |  Complete block required for well\-formed report\.  | 
|  metrics  |  met  |  Y  |  Object  |   |  Complete block required for well\-formed report\.  | 


**Header block**  

|  Long name  |  Short name  |  Required  |  Type  |  Constraints  |  Notes  | 
| --- | --- | --- | --- | --- | --- | 
|  report\_id  |  rid  |  Y  |  Integer  |   |  Monotonically increasing value\. Epoch timestamp recommended\.  | 
|  version  |  v  |  Y  |  String  |  Major\.Minor  |  Minor increments with addition of field\. Major increments if metrics removed\.  | 

**Metrics Block:**


**TCP connections**  

|  Long name  |  Short name  |  Parent element  |  Required  |  Type  |  Constraints  |  Notes  | 
| --- | --- | --- | --- | --- | --- | --- | 
|  tcp\_connections  |  tc  |  metrics  |  N  |  Object  |   |   | 
|  established\_connections  |  ec  |  tcp\_connections  |  N  |  List<Connection>  |   |  Established TCP state  | 
|  connections  |  cs  |  established\_connections  |  N  |  List<Object>  |   |   | 
|  remote\_addr  |  rad  |  connections  |  Y  |  Number  |  ip:port  |  IP can be IPv6 or IPv4  | 
|  local\_port  |  lp  |  connections  |  N  |  Number  |  >= 0  |   | 
|  local\_interface  |  li  |  connections  |  N  |  String  |   |  Interface name  | 
|  total  |  t  |  established\_connections  |  N  |  Number  |  >= 0  |  Number of established connections  | 


**Listening TCP ports**  

|  Long name  |  Short name  |  Parent element  |  Required  |  Type  |  Constraints  |  Notes  | 
| --- | --- | --- | --- | --- | --- | --- | 
|  listening\_tcp\_ports  |  tp  |  metrics  |  N  |  Object  |   |   | 
|  ports  |  pts  |  listening\_tcp\_ports  |  N  |  List<Port>  |  > 0  |   | 
|  port  |  pt  |  ports  |  N  |  Number  |  > 0  |  ports should be numbers greater than 0  | 
|  interface  |  if  |  ports  |  N  |  String  |   |  Interface name  | 
|  total  |  t  |  listening\_tcp\_ports  |  N  |  Number  |  >= 0  |   | 


**Listening UDP ports**  

|  Long name  |  Short name  |  Parent element  |  Required  |  Type  |  Constraints  |  Notes  | 
| --- | --- | --- | --- | --- | --- | --- | 
|  listening\_udp\_ports  |  up  |  metrics  |  N  |  Object  |   |   | 
|  ports  |  pts  |  listening\_udp\_ports  |  N  |  List<Port>  |  > 0  |   | 
|  port  |  pt  |  ports  |  N  |  Number  |  > 0  |  Ports should be numbers greater than 0  | 
|  interface  |  if  |  ports  |  N  |  String  |   |  Interface name  | 
|  total  |  t  |  listening\_udp\_ports  |  N  |  Number  |  >= 0  |   | 


**Network statistics**  

|  Long name  |  Short name  |  Parent element  |  Required  |  Type  |  Constraints  |  Notes  | 
| --- | --- | --- | --- | --- | --- | --- | 
|  network\_stats  |  ns  |  metrics  |  N  |  Object  |   |   | 
|  bytes\_in  |  bi  |  network\_stats  |  N  |  Number  |  Delta Metric, >= 0  |   | 
|  bytes\_out  |  bo  |  network\_stats  |  N  |  Number  |  Delta Metric, >= 0  |   | 
|  packets\_in  |  pi  |  network\_stats  |  N  |  Number  |  Delta Metric, >= 0  |   | 
|  packets\_out  |  po  |  network\_stats  |  N  |  Number  |  Delta Metric, >= 0  |   | 

**Example**  
The following JSON structure uses long names\.  

```
{
    "header": {
        "report_id": 1530304554,
        "version": "1.0"
    },
    "metrics": {
        "listening_tcp_ports": {
            "ports": [
                {
                    "interface": "eth0",
                    "port": 24800
                },
                {
                    "interface": "eth0",
                    "port": 22
                },
                {
                    "interface": "eth0",
                    "port": 53
                }
            ],
            "total": 3
        },
        "listening_udp_ports": {
            "ports": [
                {
                    "interface": "eth0",
                    "port": 5353
                },
                {
                    "interface": "eth0",
                    "port": 67
                }
            ],
            "total": 2
        },
        "network_stats": {
            "bytes_in": 29358693495,
            "bytes_out": 26485035,
            "packets_in": 10013573555,
            "packets_out": 11382615
        },
        "tcp_connections": {
            "established_connections": {
                "connections": [
                    {
                        "local_interface": "eth0",
                        "local_port": 80,
                        "remote_addr": "192.168.0.1:8000"
                    },
                    {
                        "local_interface": "eth0",
                        "local_port": 80,
                        "remote_addr": "192.168.0.1:8000"
                    }
                ],
                "total": 2
            }
        }
    }
}
```

**Example JSON structure using short names**  

```
{
    "hed": {
        "rid": 1530305228,
        "v": "1.0"
    },
    "met": {
        "tp": {
            "pts": [
                {
                    "if": "eth0",
                    "pt": 24800
                },
                {
                    "if": "eth0",
                    "pt": 22
                },
                {
                    "if": "eth0",
                    "pt": 53
                }
            ],
            "t": 3
        },
        "up": {
            "pts": [
                {
                    "if": "eth0",
                    "pt": 5353
                },
                {
                    "if": "eth0",
                    "pt": 67
                }
            ],
            "t": 2
        },
        "ns": {
            "bi": 29359307173,
            "bo": 26490711,
            "pi": 10014614051,
            "po": 11387620
        },
        "tc": {
            "ec" : {
                "cs": [
                    {
                        "li": "eth0",
                        "lp": 80,
                        "rad": "192.168.0.1:8000"
                    },
                    {
                        "li": "eth0",
                        "lp": 80,
                        "rad": "192.168.0.1:8000"
                    }
                ],
                "t": 2
            }
        }
    }
}
```