# GetTopicStatus

调用GetTopicStatus获取Topic的消息收发状态。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=alikafka&api=GetTopicStatus&type=RPC&version=2019-09-16)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|GetTopicStatus|要执行的操作。取值：

 **GetTopicStatus**。 |
|InstanceId|String|是|alikafka\_pre-cn-v0h15tjmo003|实例ID。 |
|Topic|String|是|normal\_topic\_9d034262835916103455551be06cc2dc\_6|Topic名称。 |
|RegionId|String|否|cn-hangzhou|地域ID。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|Code|Integer|200|状态码。返回200代表成功。 |
|Message|String|operation success.|返回信息。 |
|RequestId|String|E475C7E2-8C35-46EF-BE7D-5D2A9F5D\*\*\*\*|请求ID。 |
|Success|Boolean|true|调用是否成功。 |
|TopicStatus|Struct| |Topic状态。 |
|LastTimeStamp|Long|1566470063575|最后一条被消费的消息的产生时间。 |
|OffsetTable|Array| |偏移列表。 |
|OffsetTable| | | |
|LastUpdateTimestamp|Long|1566470063547|最后更新时间。 |
|MaxOffset|Long|76|消息最大位点。 |
|MinOffset|Long|0|消息最小位点。 |
|Partition|Integer|0|分区ID。 |
|Topic|String|testkafka|Topic名称。 |
|TotalCount|Long|423|消息总数。 |

## 示例

请求示例

```
http(s)://[Endpoint]/?Action=GetTopicStatus
&InstanceId=alikafka_pre-cn-v0h15tjmo003
&Topic=normal_topic_9d034262835916103455551be06cc2dc_6
&<公共请求参数>
```

正常返回示例

`XML` 格式

```
<GetTopicStatusResponse>
      <Message>operation success.</Message>
      <RequestId>D2FF4C1A-2A4A-4C24-BFCD-A2FF2DC1AAFE</RequestId>
      <TopicStatus>
            <TotalCount>0</TotalCount>
            <LastTimeStamp>0</LastTimeStamp>
            <OffsetTable>
                  <OffsetTable>
                        <Partition>0</Partition>
                        <LastUpdateTimestamp>0</LastUpdateTimestamp>
                        <Topic>demo</Topic>
                        <MaxOffset>0</MaxOffset>
                        <MinOffset>0</MinOffset>
                  </OffsetTable>
                  <OffsetTable>
                        <Partition>1</Partition>
                        <LastUpdateTimestamp>0</LastUpdateTimestamp>
                        <Topic>demo</Topic>
                        <MaxOffset>0</MaxOffset>
                        <MinOffset>0</MinOffset>
                  </OffsetTable>
                  <OffsetTable>
                        <Partition>2</Partition>
                        <LastUpdateTimestamp>0</LastUpdateTimestamp>
                        <Topic>demo</Topic>
                        <MaxOffset>0</MaxOffset>
                        <MinOffset>0</MinOffset>
                  </OffsetTable>
                  <OffsetTable>
                        <Partition>3</Partition>
                        <LastUpdateTimestamp>0</LastUpdateTimestamp>
                        <Topic>demo</Topic>
                        <MaxOffset>0</MaxOffset>
                        <MinOffset>0</MinOffset>
                  </OffsetTable>
                  <OffsetTable>
                        <Partition>4</Partition>
                        <LastUpdateTimestamp>0</LastUpdateTimestamp>
                        <Topic>demo</Topic>
                        <MaxOffset>0</MaxOffset>
                        <MinOffset>0</MinOffset>
                  </OffsetTable>
                  <OffsetTable>
                        <Partition>5</Partition>
                        <LastUpdateTimestamp>0</LastUpdateTimestamp>
                        <Topic>demo</Topic>
                        <MaxOffset>0</MaxOffset>
                        <MinOffset>0</MinOffset>
                  </OffsetTable>
                  <OffsetTable>
                        <Partition>6</Partition>
                        <LastUpdateTimestamp>0</LastUpdateTimestamp>
                        <Topic>demo</Topic>
                        <MaxOffset>0</MaxOffset>
                        <MinOffset>0</MinOffset>
                  </OffsetTable>
                  <OffsetTable>
                        <Partition>7</Partition>
                        <LastUpdateTimestamp>0</LastUpdateTimestamp>
                        <Topic>demo</Topic>
                        <MaxOffset>0</MaxOffset>
                        <MinOffset>0</MinOffset>
                  </OffsetTable>
                  <OffsetTable>
                        <Partition>8</Partition>
                        <LastUpdateTimestamp>0</LastUpdateTimestamp>
                        <Topic>demo</Topic>
                        <MaxOffset>0</MaxOffset>
                        <MinOffset>0</MinOffset>
                  </OffsetTable>
                  <OffsetTable>
                        <Partition>9</Partition>
                        <LastUpdateTimestamp>0</LastUpdateTimestamp>
                        <Topic>demo</Topic>
                        <MaxOffset>0</MaxOffset>
                        <MinOffset>0</MinOffset>
                  </OffsetTable>
                  <OffsetTable>
                        <Partition>10</Partition>
                        <LastUpdateTimestamp>0</LastUpdateTimestamp>
                        <Topic>demo</Topic>
                        <MaxOffset>0</MaxOffset>
                        <MinOffset>0</MinOffset>
                  </OffsetTable>
                  <OffsetTable>
                        <Partition>11</Partition>
                        <LastUpdateTimestamp>0</LastUpdateTimestamp>
                        <Topic>demo</Topic>
                        <MaxOffset>0</MaxOffset>
                        <MinOffset>0</MinOffset>
                  </OffsetTable>
            </OffsetTable>
      </TopicStatus>
      <Success>true</Success>
      <Code>200</Code>
</GetTopicStatusResponse>
```

`JSON` 格式

```
{
    "RequestId":"E475C7E2-8C35-46EF-BE7D-5D2A9F5D****",
    "Message":"operation success.",
    "TopicStatus":{
        "LastTimeStamp":"1566470063575",
        "TotalCount":"423",
        "OffsetTable":{
            "OffsetTable":[
                {
                    "Partition":"0",
                    "MaxOffset":"76",
                    "Topic":"testkafka",
                    "LastUpdateTimestamp":"1566470063547",
                    "MinOffset":"0"
                }
            ]
        }
    },
    "Code":"200",
    "Success":"true"
}
```

## 错误码

访问[错误中心](https://error-center.aliyun.com/status/product/alikafka)查看更多错误码。

