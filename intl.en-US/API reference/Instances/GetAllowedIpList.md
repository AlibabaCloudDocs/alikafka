# GetAllowedIpList

Queries an IP address whitelist.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=alikafka&api=GetAllowedIpList&type=RPC&version=2019-09-16)

## Request parameters

|Parameter|Type|Required|Example|Description |
|---------|----|--------|-------|------------|
|Action|String|Yes|GetAllowedIpList|The operation that you want to perform. Set the value to

 **GetAllowedIpList**. |
|InstanceId|String|Yes|alikafka\_post-cn-mp91inkw\*\*\*\*|The ID of the instance whose whitelist you want to obtain. |
|RegionId|String|Yes|cn-hangzhou|The ID of the region where the instance is located. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|AllowedList|Struct| |The IP address whitelist. |
|DeployType|Integer|4|The deployment mode of the instance. Valid values:

 -   **4**: Internet and VPC
-   **5**: VPC

 **Note:** This parameter is necessary only for integrator customers. |
|InternetList|Array| |The whitelist for Internet access. |
|AllowedIpList|List|0.0.0.0/0|The IP addresses in the whitelist. |
|PortRange|String|9093/9093|The allowed port range for the IP addresses in the whitelist. Valid values:

 **9093/9093**. |
|VpcList|Array| |The whitelist for VPC access. |
|AllowedIpList|List|192.XXX.X.X/XX|The IP addresses in the whitelist. |
|PortRange|String|9092/9092|The allowed port range for the IP addresses in the whitelist. Valid values:

 **9092/9092**. |
|Code|Integer|200|The response code. The HTTP 200 code indicates that the request was successful. |
|Message|String|operation success.|The response message. |
|RequestId|String|A421CCD7-5BC5-4B32-8DD8-64668A8FCB56|The ID of the request. |
|Success|Boolean|true|Indicates whether the request was successful. |

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
<GetAllowedIpListResponse>
      <RequestId>A421CCD7-5BC5-4B32-8DD8-64668A8FCB56</RequestId>
      <Message>operation success. </Message>
      <AllowedList>
            <DeployType>4</DeployType>
            <InternetList>
                  <PortRange>9092/9092</PortRange>
            </InternetList>
            <InternetList>
                  <AllowedIpList>192.XXX.X.X/XX</AllowedIpList>
            </InternetList>
            <InternetList>
                  <PortRange>9093/9093</PortRange>
            </InternetList>
            <InternetList>
                  <AllowedIpList>0.0.0.0/0</AllowedIpList>
            </InternetList>
            <VpcList>
                  <PortRange>9092/9092</PortRange>
            </VpcList>
            <VpcList>
                  <AllowedIpList>192.XXX.X.X/XX</AllowedIpList>
            </VpcList>
            <VpcList>
                  <PortRange>9093/9093</PortRange>
            </VpcList>
            <VpcList>
                  <AllowedIpList>0.0.0.0/0</AllowedIpList>
            </VpcList>
      </AllowedList>
      <Code>200</Code>
      <Success>true</Success>
</GetAllowedIpListResponse>
```

`JSON` format

```
{
    "RequestId": "A421CCD7-5BC5-4B32-8DD8-64668A8FCB56",
    "Message": "operation success.",
    "AllowedList": {
        "DeployType": 4,
        "InternetList": [
            {
                "PortRange": "9092/9092"
            },
            {
                "AllowedIpList": "192.XXX.X.X/XX"
            },
            {
                "PortRange": "9093/9093"
            },
            {
                "AllowedIpList": "0.0.0.0/0"
            }
        ],
        "VpcList": [
            {
                "PortRange": "9092/9092"
            },
            {
                "AllowedIpList": "192.XXX.X.X/XX"
            },
            {
                "PortRange": "9093/9093"
            },
            {
                "AllowedIpList": "0.0.0.0/0"
            }
        ]
    },
    "Code": 200,
    "Success": true
}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/alikafka).

