# CreateTopic

You can call this operation to create a topic.

Note the following when you call this operation to create a topic:

-   Each user can send a maximum of one query per second \(QPS\).
-   The maximum number of topics that can be created in each Message Queue for Apache Kafka instance depends on the version of the Message Queue for Apache Kafka instance you purchased.

## Debugging

[You can use OpenAPI Explorer to make API calls, search for API calls, perform debugging, and generate SDK example code.](https://api.aliyun.com/#product=alikafka&api=CreateTopic&type=RPC&version=2019-09-16)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String |Yes|CreateTopic|The operation that you want to perform. Set the value to

 **CreateTopic**. |
|InstanceId|String|Yes|alikafka\_pre-cn-mp919o4v\*\*\*\*|The ID of the Message Queue for Apache Kafka instance where you want to create a topic. |
|RegionId|String|Yes|cn-hangzhou|The region ID of the Message Queue for Apache Kafka instance where you want to create a topic. |
|Remark|String|Yes|alikafka\_topic\_test|The name of the topic. The value of this parameter must meet the following requirements:

 -   The value can only contain letters, digits, hyphens \(-\), and underscores \(\_\).
-   The value must be 3 to 64 characters in length. |
|Topic|String|Yes|alikafka\_topic\_test|The name of the topic. The value of this parameter must meet the following requirements:

 -   The name can only contain letters, digits, hyphens \(-\), and underscores \(\_\).
-   The name must be 3 to 64 characters in length, and will be automatically truncated if it contains more characters.
-   The name cannot be modified after being created. |
|PartitionNum|String|No|12|The number of partitions in the topic. Valid values:

 -   1 to 48
-   We recommend that you set the number of partitions to a multiple of 6 to reduce the risk of data skew.

Note: For special requirements, submit a ticket. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|Code|Integer|200|The returned status code.

 If "200" is returned, the request is successful. |
|Message|String|operation success|The returned message. |
|RequestId|String|9C0F207C-77A6-43E5-991C-9D98510A\*\*\*\*|The ID of the request. |
|Success|Boolean|true|Indicates whether the request is successful. |

## Examples

Sample requests

```
http(s)://[Endpoint]/? Action=CreateTopic
&InstanceId=alikafka_pre-cn-mp919o4v****
&RegionId=cn-hangzhou
&Remark=alikafka_topic_test
&Topic=alikafka_topic_test
&<Common request parameters>
```

Sample success responses

`XML` format

```
<CreateTopicResponse>
	    <Message>operation success</Message>
	    <RequestId>9C0F207C-77A6-43E5-991C-9D98510A****</RequestId>
	    <Success>true</Success>
	    <Code>200</Code>
</CreateTopicResponse>
```

`JSON` format

```
{
    "CreateTopicResponse": {
        "Message": "operation success",
        "RequestId": "9C0F207C-77A6-43E5-991C-9D98510A****",
        "Success":true,
        "Code":"200"
    }
}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/alikafka).

