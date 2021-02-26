# GetAllowedIpList

Queries the IP address whitelist of a Message Queue for Apache Kafka instance.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=alikafka&api=GetAllowedIpList&type=RPC&version=2019-09-16)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Yes|GetAllowedIpList|The operation that you want to perform. Set the value to

 **GetAllowedIpList**. |
|InstanceId|String|Yes|alikafka\_post-cn-mp91inkw\*\*\*\*|The ID of the instance. |
|RegionId|String|Yes|cn-hangzhou|The region ID of the instance. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|AllowedList|Struct| |The IP address whitelist. |
|DeployType|Integer|4|The deployment mode of the instance. Valid values:

 -   **4**: Internet and VPC
-   **5**: VPC

 **Note:** This parameter is necessary only for integrator customers. |
|InternetList|Array of IPListVO| |The whitelist for Internet access. |
|AllowedIpList|List|0.0.0.0/0|The IP address whitelist. |
|PortRange|String|9093/9093|The port range. Set the value to

 **9093/9093**. |
|VpcList|Array of IPListVO| |The whitelist for VPC access. |
|AllowedIpList|List|192.XXX.X.X/XX|The IP address whitelist. |
|PortRange|String|9092/9092|The port range. Set the value to

 **9092/9092**. |
|Code|Integer|200|The response code. The HTTP 200 status code indicates that the request is successful. |
|Message|String|operation success.|The response message. |
|RequestId|String|A421CCD7-5BC5-4B32-8DD8-64668A8FCB56|The ID of the request. |
|Success|Boolean|true|Indicates whether the request is successful. |

## Examples

Sample requests

```
http(s)://[Endpoint]/? Action=GetAllowedIpList
&RegionId=cn-hangzhou
&InstanceId=alikafka_post-cn-mp91inkw****
&<Common request parameters>
```

Sample success responses

`XML` format

```
<GetAllowedIpListResponse>
        <RequestId>A421CCD7-5BC5-4B32-8DD8-64668A******</RequestId>
        <Message>operation success. </Message>
        <AllowedList>
              <DeployType>4</DeployType>
              <InternetList>
                    <PortRange>9093/9093</PortRange>
                    <AllowedIpList>0.0.0.0/0</AllowedIpList>
              </InternetList>
              <VpcList>
                    <PortRange>9094/9094</PortRange>
                    <AllowedIpList>0.0.0.0/0</AllowedIpList>
              </VpcList>
              <VpcList>
                    <PortRange>9092/9092</PortRange>
                    <AllowedIpList>0.0.0.0/0</AllowedIpList>
                    <AllowedIpList>192.XXX.X.X/XX</AllowedIpList>
              </VpcList>
        </AllowedList>
        <Code>200</Code>
        <Success>true</Success>
</GetAllowedIpListResponse>
```

`JSON` format

```
{
	"RequestId": "A421CCD7-5BC5-4B32-8DD8-64668A******",
	"Message": "operation success.",
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
				"PortRange": "9094/9094",
				"AllowedIpList": [
					"0.0.0.0/0"
				]
			},
			{
				"PortRange": "9092/9092",
				"AllowedIpList": [
					"0.0.0.0/0",
					"192.XXX.X.X/XX"
				]
			}
		]
	},
	"Code": 200,
	"Success": true
}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/alikafka).

