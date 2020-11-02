# UpdateAllowedIp

You can call this operation to update an IP address whitelist.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=alikafka&api=UpdateAllowedIp&type=RPC&version=2019-09-16)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Yes|UpdateAllowedIp|The operation that you want to perform. Set the value to UpdateAllowedIp. |
|RegionId|String|Yes|cn-hangzhou|The region ID of the Message Queue for Apache Kafka instance whose IP address whitelist you want to update |
|UpdateType|String|Yes|add|The type of the update. Valid value:

-   **add**: Add an IP address.
-   **delete**: Delete an IP address. |
|PortRange|String|Yes|9092/9092|The port number range corresponding to the IP address whitelist. Valid value:

-   **9092/9092**: port number range of the Virtual Private Cloud \(VPC\) type
-   **9093/9093**: port number range of the Internet type

**Note:** This parameter must correspond to AllowdedListType. |
|AllowedListType|String|Yes|vpc|The type of the whitelist. Valid value:

-   **vpc**: IP address whitelist of the VPC type
-   **internet**: IP address whitelist of the Internet type |
|AllowedListIp|String|Yes|0.0.0.0/0|The IP address list, which can be a CIDR block, for example, 192.168.0.100 or 192.168.0.0/16.

**Note:**-   When the value of the UpdateType parameter is add, you can enter multiple IP addresses and separate them with commas \(,\).
-   When the value of the UpdateType parameter is delete, you can enter only one IP address.
-   Use caution when you delete IP addresses. |
|InstanceId|String|Yes|alikafka\_pre-cn-0pp1cng20\*\*\*|The ID of the Message Queue for Apache Kafka instance whose IP address whitelist you want to update. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|Code|Integer|200|The returned status code. If **"200"** is returned, the request is successful. |
|Message|String|operation success.|The returned message. |
|RequestId|String|17D425C2-4EA3-4AB8-928D-E10511ECF23B|The ID of the request. |
|Success|Boolean|true|Indicates whether the request is successful. |

## Examples

Sample requests

```
http(s)://[Endpoint]/? Action=UpdateAllowedIp
&RegionId=cn-hangzhou
&UpdateType=add
&PortRange=9092/9092
&AllowedListType=vpc
&AllowedListIp=0.0.0.0/0
&InstanceId=alikafka_pre-cn-0pp1cng20***
&<Common request parameters>
```

Sample success responses

`XML` format

```
<Message>operation success</Message>
<RequestId>17D425C2-4EA3-4AB8-928D-E10511ECF23B</RequestId>
<Success>true</Success>
<Code>200</Code>
```

`JSON` format

```
{
  "Message": "operation success",
  "RequestId": "17D425C2-4EA3-4AB8-928D-E10511ECF23B",
  "Success": true,
  "Code": 200
}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/alikafka).

