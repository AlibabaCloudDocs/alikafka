# GetAllowedIpList

You can call this operation to obtain an IP address whitelist.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=alikafka&api=GetAllowedIpList&type=RPC&version=2019-09-16)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Yes|GetAllowedIpList|The operation that you want to perform. Set the value to GetAllowedIpList. |
|InstanceId|String|Yes|alikafka\_post-cn-mp91inkw\*\*\*\*|The ID of the Message Queue for Apache Kafka instance whose IP address whitelist you want to obtain. |
|RegionId|String|Yes|cn-hangzhou|The region ID of the Message Queue for Apache Kafka instance whose IP address whitelist you want to obtain. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|AllowedList|Struct| |The returned IP address whitelist. |
|DeployType|Integer|4|The deployment mode of the Message Queue for Apache Kafka instance. Valid values:

 -   **4**: deployment on the Internet and Virtual Private Cloud \(VPC\)
-   **5**: deployment on the VPC

 **Note:** This parameter is necessary only for integrator customers. |
|InternetList|Array| |The details of the returned IP address whitelist of the Internet type. |
|AllowedIpList|List|0.0.0.0/0|The returned IP address whitelist. |
|PortRange|String|9093/9093|The port number range corresponding to the IP address whitelist. Set the value to

 **9093/9093**. |
|VpcList|Array| |The details of the returned IP address whitelist of the VPC type. |
|AllowedIpList|List|192.XXX.X.X/XX|The returned IP address whitelist. |
|PortRange|String|9092/9092|The port number range corresponding to the IP address whitelist. Set the value to

 **9092/9092**. |
|Code|Integer|200|The returned status code. If **"200"** is returned, the request is successful. |
|Message|String|operation success.|The returned message. |
|RequestId|String|A421CCD7-5BC5-4B32-8DD8-64668A8FCB56|The ID of the request. |
|Success|Boolean|true|Indicates whether the request is successful. |

## Examples

Sample requests

```
http(s)://[Endpoint]/? Action=GetAllowedIpList
&InstanceId=alikafka_post-cn-mp91inkw****
&RegionId=cn-hangzhou
&<Common request parameters>
```

Sample success responses

`XML` format

```
<Message>operation success. </Message>
<RequestId>A421CCD7-5BC5-4B32-8DD8-64668A8FCB56</RequestId>
<Success>true</Success>
<Code>200</Code>
<AllowedList>
    <DeployType>4</DeployType>
    <InternetList>
        <PortRange>9093/9093</PortRange>
        <AllowedIpList>0.0.0.0/0</AllowedIpList>
    </InternetList>
    <VpcList>
        <PortRange>9092/9092</PortRange>
        <AllowedIpList>192.XXX.X.X/XX</AllowedIpList>
    </VpcList>
</AllowedList>
```

`JSON` format

```
{
  "Message": "operation success.",
  "RequestId": "A421CCD7-5BC5-4B32-8DD8-64668A8FCB56",
  "Success": true,
  "Code": 200,
  "AllowedList": {
    "DeployType": 4,
    "InternetList": [
      {
        "PortRange": "9093/9093",
        "AllowedIpList": [
          "0.0.0.0/0"
        ]
      }
    ],
    "VpcList": [
      {
        "PortRange": "9092/9092",
        "AllowedIpList": [
          "192.XXX.X.X/XX"
        ]
      }
    ]
  }
}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/alikafka).

