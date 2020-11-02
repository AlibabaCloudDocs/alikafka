# UpdateAllowedIp

Updates an IP address whitelist.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=alikafka&api=UpdateAllowedIp&type=RPC&version=2019-09-16)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Yes|UpdateAllowedIp|The operation that you want to perform. Set the value to

 **UpdateAllowedIp**. |
|AllowedListIp|String|Yes|0.0.0.0/0|The IP addresses that you want to add to or delete from the whitelist. You can specify a CIDR block, such as **192.168.0.0/16** .

 -   If the value of the **UpdateType** parameter is **add**, you can enter multiple IP addresses and separate them with commas \(,\).
-   If the value of the **UpdateType** parameter is **delete**, you can enter only one IP address or CIDR block.
-   Exercise caution when you delete IP addresses. |
|AllowedListType|String|Yes|vpc|The type of the whitelist. Valid values:

 -   **vpc**: IP address whitelist for VPC access
-   **internet**: IP address whitelist for Internet access |
|InstanceId|String|Yes|alikafka\_pre-cn-0pp1cng20\*\*\*|The ID of the instance whose whitelist you want to update. |
|PortRange|String|Yes|9092/9092|The allowed port range for the IP addresses in the whitelist. Valid values:

 -   **9092/9092**: port range for a VPC whitelist
-   **9093/9093**: port range for an Internet whitelist

 This parameter must correspond to **AllowdedListType**. |
|RegionId|String|Yes|cn-hangzhou|The ID of the region where the instance is located. |
|UpdateType|String|Yes|add|The update operation. Valid values:

 -   **add**: Add an IP address.
-   **delete**: Delete an IP address. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|Code|Integer|200|The response code. The HTTP 200 code indicates that the request was successful. |
|Message|String|operation success.|The response message. |
|RequestId|String|17D425C2-4EA3-4AB8-928D-E10511ECF\*\*\*|The ID of the request. |
|Success|Boolean|true|Indicates whether the request was successful. |

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
<UpdateAllowedIpResponse>
      <RequestId>17D425C2-4EA3-4AB8-928D-E10511ECF***</RequestId>
      <Message>operation success. </Message>
      <Code>200</Code>
      <Success>true</Success>
</UpdateAllowedIpResponse>
```

`JSON` format

```
{
    "RequestId": "17D425C2-4EA3-4AB8-928D-E10511ECF***",
    "Message": "operation success.",
    "Code": 200,
    "Success": true
}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/alikafka).

