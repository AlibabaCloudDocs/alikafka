# UpgradeInstanceVersion

Upgrades the version of a Message Queue for Apache Kafka instance.

## **Requirements for authorization**

A RAM user must be authorized before you can use it to call the **UpgradeInstanceVersion** operation. For more information about the authorization, see [RAM policies](~~185815~~).

|API

|Action

|Resource |
|-----|--------|----------|
|UpgradeInstanceVersion

|UpdateInstance

|acs:alikafka:\*:\*:\{instanceId\} |

## **Limits on QPS**

You can send a maximum of two queries per second \(QPS\).

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=alikafka&api=UpgradeInstanceVersion&type=RPC&version=2019-09-16)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Yes|UpgradeInstanceVersion|The operation that you want to perform. Set the value to **UpgradeInstanceVersion**. |
|InstanceId|String|Yes|alikafka\_post-cn-v0h1fgs2\*\*\*\*|The ID of the Message Queue for Apache Kafka instance. |
|RegionId|String|Yes|cn-hangzhou|The region ID of the instance. |
|TargetVersion|String|Yes|0.10.2|The major version to be upgraded to. Valid values:

-   **0.10.2**
-   **2.2.0**

If the instance is in the specified major version, the minor version of the instance will be upgraded to the latest version. |

## **Description of the TargetVersion parameter**

|Major version before the upgrade

|Minor version before the upgrade

|Major version to be upgraded to

|Major version after the upgrade

|Minor version after the upgrade |
|----------------------------------|----------------------------------|---------------------------------|---------------------------------|---------------------------------|
|0.10.2

|Not the latest version

|0.10.2

|0.10.2

|Latest version |
|0.10.2

|Not the latest version

|2.2.0

|2.2.0

|Latest version |
|0.10.2

|Latest version

|2.2.0

|2.2.0

|Latest version |
|2.2.0

|Not the latest version

|2.2.0

|2.2.0

|Latest version |

For more information about how to upgrade the minor version of an instance, see [Upgrade the instance version](~~113173~~).

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|Code|Integer|200|The response code. The HTTP status code 200 indicates that the request is successful. |
|Message|String|operation success.|The response message. |
|RequestId|String|ABA4A7FD-E10F-45C7-9774-A5236015\*\*\*|The ID of the request. |
|Success|Boolean|true|Indicates whether the request is successful. |

## Examples

Sample requests

```
http(s)://[Endpoint]/? Action=UpgradeInstanceVersion
&InstanceId=alikafka_post-cn-v0h1fgs2****
&RegionId=cn-hangzhou
&TargetVersion=0.10.2
&<Common request parameters>
```

Sample success responses

`XML` format

```
<UpgradeInstanceVersionResponse>
      <Message>operation success. </Message>
      <RequestId>ABA4A7FD-E10F-45C7-9774-A5236015***</RequestId>
      <Code>200</Code>
      <Success>true</Success>
</UpgradeInstanceVersionResponse>
```

`JSON` format

```
{
    "Message": "operation success.",
    "RequestId": "ABA4A7FD-E10F-45C7-9774-A5236015***",
    "Code": 200,
    "Success": true
}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/alikafka).

