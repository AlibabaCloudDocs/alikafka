# DescribeMetricMetaList

You can call this operation to query the descriptions of time series metrics that are supported in CloudMonitor.

This operation is usually used together with DescribeMetricList and DescribeMetricLast to query monitoring data.

## Debugging

[You can call this operation in OpenAPI Explorer without the need to manually calculate the signature. After you call the operation, OpenAPI Explorer can automatically generate SDK code samples.](https://api.aliyun.com/#product=Cms&api=DescribeMetricMetaList&type=RPC&version=2019-01-01)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Namespace|String|Yes|acs\_kvstore|The namespace of the monitored service. Namespaces are used to distinguish between different services. |
|Action|String|No|DescribeMetricMetaList|The operation that you want to perform. Set this parameter to DescribeMetricMetaList. |
|Labels|String|No|\[\{"name":"productCategory","value":"kvstore\_old"\}\]|The tags of the metric. You must specify the tags in the following format: `[{"name":"tag name","value":"tag value"},{"name":"tag name","value":"tag value"}]`.

 The following are the available tag names:

 -   metricCategory: the category of the metrics.
-   alertEnable: specifies whether to enable alerts.
-   alertUnit: the unit of the metric in the alert.
-   unitFactor: the factor for metric unit conversion.
-   minAlertPeriod: the minimum time interval to raise a new alert.
-   productCategory: the category of the service. |
|MetricName|String|No|CPUUtilization|The name of the metric. The metric with the specified name is matched. |
|PageNumber|Integer|No|1|The number of the page. Default value: 1. |
|PageSize|Integer|No|30|The maximum number of records on each page. Default value: 30. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|Success|Boolean|true|Indicates whether the call is successful. |
|Code|String|200|The status code. A value of 200 indicates that the call is successful. |
|RequestId|String|0CCE0AF0-053C-4B13-A583-DC9A85785D49|The ID of the request, which can be used for troubleshooting. |
|Message|String|success|The error message. |
|Resources| | |The list of the metrics. |
|Description|String|CPUUtilization|The description of the metric. |
|Dimensions|String|instanceId|The dimensions of the metric. Multiple dimensions are separated with commas \(,\), for example, userId,instanceId. |
|Labels|String|\[\{\\"name\\":\\"alertUnit\\",\\"value\\":\\"Bytes\\"\},\{\\"name\\":\\"minAlertPeriod\\",\\"value\\":\\"60\\"\},\{\\"name\\":\\"metricCategory\\",\\"value\\":\\"instanceId\\"\},\{\\"name\\":\\"instanceType\\",\\"value\\":\\"disaster\\"\},\{\\"name\\":\\"is\_alarm\\",\\"value\\":\\"true\\"\},\{\\"name\\":\\"productCategory\\",\\"value\\":\\"kvstore\_old\\"\}\]|The tags of the metric. The value is a JSON array string. The array can include repeated tag names. Sample value: `[{"name":"tag name","value":"tag value"}]`.

 The following are the available tag names:

 -   metricCategory: the category of the metrics.
-   alertEnable: indicates whether the alert is enabled.
-   alertUnit: the unit of the metric in the alert.
-   unitFactor: the factor for metric unit conversion.
-   minAlertPeriod: the minimum time interval to raise a new alert.
-   productCategory: the category of the service. |
|MetricName|String|CPUUtilization|The name of the metric. |
|Namespace|String|acs\_kvstore|The namespace of the monitored serivce. The namespace is usually in the acs\_service name format. |
|Periods|String|60,300|The statistical period of the metric. Multiple periods are separated with commas \(,\), for example, 15,60,900. |
|Statistics|String|Average,Minimum,Maximum|The statistical method. Multiple statistical methods are separated with commas \(,\), for example, Average,Minimum,Maximum. |
|Unit|String|%|The unit of the metric. |
|TotalCount|String|12|The total number of returned records. |

## Examples

Sample request

```

http(s)://[Endpoint]/? Action=DescribeMetricMetaList
&Namespace=acs_ecs_dashboard
&<Common request parameters>

```

Sample success response

`XML` format

```
<DescribeMetricMetaListResponse>
    <TotalCount>120</TotalCount>
    <RequestId>EA94B362-973C-4D6B-BE94-6774E18915C2</RequestId>
    <Success>true</Success>
    <Code>200</Code>
    <Resources>
        <Resource>
            <Description>ECS.CPUUtilization</Description>
            <Statistics>Average,Minimum,Maximum</Statistics>
            <MetricName>CPUUtilization</MetricName>
            <Labels>[{"name":"alertUnit","value":"%"},{"name":"alertDefault","value":"80"},{"name":"minAlertPeriod","value":"60"},{"name":"metricCategory","value":"instanceId"},{"name":"is_alarm","value":"true"}]</Labels>
            <Dimensions>userId,instanceId</Dimensions>
            <Namespace>acs_ecs_dashboard</Namespace>
            <Periods>60,300</Periods>
            <Unit>%</Unit>
        </Resource>
        <Resource>
            <Description></Description>
            <Statistics>Average,Maximum,Minimum</Statistics>
            <MetricName>DiskReadBPS</MetricName>
            <Labels>[{"name":"alertUnit","value":"Mbytes/s"},{"name":"minAlertPeriod","value":"60"},{"name":"unitFactor","value":"1048576"},{"name":"metricCategory","value":"instanceId"},{"name":"is_alarm","value":"true"}]</Labels>
            <Dimensions>userId,instanceId</Dimensions>
            <Namespace>acs_ecs_dashboard</Namespace>
            <Periods>60,300</Periods>
            <Unit>bytes/s</Unit>
        </Resource>
        <Resource>
            <Description>The read IOPS of the disk</Description>
            <Statistics>Average,Minimum,Maximum</Statistics>
            <MetricName>DiskReadIOPS</MetricName>
            <Labels>[{"name":"alertUnit","value":"Count/Second"},{"name":"minAlertPeriod","value":"60"},{"name":"metricCategory","value":"instanceId"},{"name":"is_alarm","value":"true"}]</Labels>
            <Dimensions>userId,instanceId</Dimensions>
            <Namespace>acs_ecs_dashboard</Namespace>
            <Periods>60,300</Periods>
            <Unit>Count/Second</Unit>
        </Resource>
        <Resource>
            <Description></Description>
            <Statistics>Average,Maximum,Minimum</Statistics>
            <MetricName>DiskWriteBPS</MetricName>
            <Labels>[{"name":"alertUnit","value":"Mbytes/s"},{"name":"minAlertPeriod","value":"60"},{"name":"unitFactor","value":"1048576"},{"name":"metricCategory","value":"instanceId"},{"name":"is_alarm","value":"true"}]</Labels>
            <Dimensions>userId,instanceId</Dimensions>
            <Namespace>acs_ecs_dashboard</Namespace>
            <Periods>60,300</Periods>
            <Unit>bytes/s</Unit>
        </Resource>
        <Resource>
            <Description>The write IOPS of the disk</Description>
            <Statistics>Average,Minimum,Maximum</Statistics>
            <MetricName>DiskWriteIOPS</MetricName>
            <Labels>[{"name":"alertUnit","value":"Count/Second"},{"name":"minAlertPeriod","value":"60"},{"name":"metricCategory","value":"instanceId"},{"name":"is_alarm","value":"true"}]</Labels>
            <Dimensions>userId,instanceId</Dimensions>
            <Namespace>acs_ecs_dashboard</Namespace>
            <Periods>60,300</Periods>
            <Unit>Count/Second</Unit>
        </Resource>
        <Resource>
            <Description></Description>
            <Statistics>Average,Minimum,Maximum</Statistics>
            <MetricName>GroupCPUUtilization</MetricName>
            <Labels>[{"name":"alertUnit","value":"%"},{"name":"minAlertPeriod","value":"60"},{"name":"metricCategory","value":"groupId"}]</Labels>
            <Dimensions>groupId</Dimensions>
            <Namespace>acs_ecs_dashboard</Namespace>
            <Periods>60,300</Periods>
            <Unit>%</Unit>
        </Resource>
        <Resource>
            <Description></Description>
            <Statistics>Average,Minimum,Maximum</Statistics>
            <MetricName>GroupDiskReadBPS</MetricName>
            <Labels>[{"name":"alertUnit","value":"Mbytes/s"},{"name":"minAlertPeriod","value":"60"},{"name":"unitFactor","value":"1048576"},{"name":"metricCategory","value":"groupId"}]</Labels>
            <Dimensions>groupId</Dimensions>
            <Namespace>acs_ecs_dashboard</Namespace>
            <Periods>60,300</Periods>
            <Unit>bytes/s</Unit>
        </Resource>
        <Resource>
            <Description></Description>
            <Statistics>Average,Minimum,Maximum</Statistics>
            <MetricName>GroupDiskReadIOPS</MetricName>
            <Labels>[{"name":"alertUnit","value":"Count/Second"},{"name":"minAlertPeriod","value":"60"},{"name":"metricCategory","value":"groupId"}]</Labels>
            <Dimensions>groupId</Dimensions>
            <Namespace>acs_ecs_dashboard</Namespace>
            <Periods>60,300</Periods>
            <Unit>Count/Second</Unit>
        </Resource>
        <Resource>
            <Description></Description>
            <Statistics>Average,Minimum,Maximum</Statistics>
            <MetricName>GroupDiskWriteBPS</MetricName>
            <Labels>[{"name":"alertUnit","value":"Mbytes/s"},{"name":"minAlertPeriod","value":"60"},{"name":"unitFactor","value":"1048576"},{"name":"metricCategory","value":"groupId"}]</Labels>
            <Dimensions>groupId</Dimensions>
            <Namespace>acs_ecs_dashboard</Namespace>
            <Periods>60,300</Periods>
            <Unit>bytes/s</Unit>
        </Resource>
        <Resource>
            <Description></Description>
            <Statistics>Average,Minimum,Maximum</Statistics>
            <MetricName>GroupDiskWriteIOPS</MetricName>
            <Labels>[{"name":"alertUnit","value":"Count/Second"},{"name":"minAlertPeriod","value":"60"},{"name":"metricCategory","value":"groupId"}]</Labels>
            <Dimensions>groupId</Dimensions>
            <Namespace>acs_ecs_dashboard</Namespace>
            <Periods>60,300</Periods>
            <Unit>Count/Second</Unit>
        </Resource>
    </Resources>
</DescribeMetricMetaListResponse>
```

`JSON` format

```
{
	"TotalCount":1853,
	"RequestId":"CDE9EAFF-D54E-4024-BBFC-B0AAC883143B",
	"Success":true,
	"Code":200,
	"Resources":{
		"Resource":[
			{
				"Description": "The disk size",
				"MetricName":"ads.diskSize",
				"Statistics":"Average,Minimum,Maximum",
				"Labels":"[{\"name\":\"minAlertPeriod\",\"value\":\"300\"},{\"name\":\"alertDefault\",\"value\":\"\"},{\"name\":\"unitFactor\",\"value\":\"1\"},{\"name\":\"alertUnit\",\"value\":\"Mbytes\"},{\"name\":\"productCategory\",\"value\":\"ads\"},{\"name\":\"is_alarm\",\"value\":\"true\"},{\"name\":\"metricCategory\",\"value\":\"workerId\"}]",
				"Dimensions":"userId,instanceId,tableSchema,workerId",
				"Namespace":"acs_ads",
				"Periods":"300",
				"Unit":"Mbytes"
			},
			{
				"Description": "The used space of the disk",
				"MetricName":"ads.diskUsed",
				"Statistics":"Average,Minimum,Maximum",
				"Labels":"[{\"name\":\"alertUnit\",\"value\":\"Mbytes\"},{\"name\":\"minAlertPeriod\",\"value\":\"300\"},{\"name\":\"metricCategory\",\"value\":\"workerId\"},{\"name\":\"is_alarm\",\"value\":\"true\"}]",
				"Dimensions":"userId,instanceId,tableSchema,workerId",
				"Namespace":"acs_ads",
				"Periods":"300",
				"Unit":"Mbytes"
			},
			{
				"Description":"The disk utilization",
				"MetricName":"ads.diskUsedPercent",
				"Statistics":"Average,Minimum,Maximum",
				"Labels":"[{\"name\":\"alertUnit\",\"value\":\"%\"},{\"name\":\"minAlertPeriod\",\"value\":\"300\"},{\"name\":\"metricCategory\",\"value\":\"workerId\"},{\"name\":\"is_alarm\",\"value\":\"true\"}]",
				"Dimensions":"userId,instanceId,tableSchema,workerId",
				"Namespace":"acs_ads",
				"Periods":"300",
				"Unit":"%"
			},
			{
				"Description": "The number of unconsumed messages in the queue",
				"MetricName":"QueueMessageAccumulation",
				"Statistics":"Maximum",
				"Labels":"[{\"name\":\"minAlertPeriod\",\"value\":\"60\"},{\"name\":\"alertDefault\",\"value\":\"\"},{\"name\":\"unitFactor\",\"value\":\"1\"},{\"name\":\"alertUnit\",\"value\":\"count/min\"},{\"name\":\"productCategory\",\"value\":\"amqp\"},{\"name\":\"is_alarm\",\"value\":\"true\"},{\"name\":\"metricCategory\",\"value\":\"queue\"}]",
				"Dimensions":"userId,regionId,vhostName,queueName",
				"Namespace":"acs_amqp",
				"Periods":"60,300",
				"Unit":"count/min"
			},
			{
				"Description": "The number of new messages in the queue per minute",
				"MetricName":"QueueMessageInput",
				"Statistics":"Value",
				"Labels":"[{\"name\":\"minAlertPeriod\",\"value\":\"60\"},{\"name\":\"alertDefault\",\"value\":\"\"},{\"n ame\":\"unitFactor\",\"value\":\"1\"},{\"name\":\"alertUnit\",\"value\":\"count/min\"},{\"name\":\"productCategory\",\"value\":\"amqp\"},{\"name\":\"is_alarm\",\"value\":\"true\"},{\"name\":\"metricCategory\",\"value\":\"queue\"}]",
				"Dimensions":"userId,regionId,vhostName,queueName",
				"Namespace":"acs_amqp",
				"Periods":"60,300",
				"Unit":"count/min"
			},
			{
				"Description": "The number of unconsumed messages in the queue",
				"MetricName":"QueueMessageOutput",
				"Statistics":"Value",
				"Labels":"[{\"name\":\"minAlertPeriod\",\"value\":\"60\"},{\"name\":\"alertDefault\",\"value\":\"\"},{\"name\":\"unitFactor\",\"value\":\"1\"},{\"name\":\"alertUnit\",\"value\":\"count/min\"},{\"name\":\"productCategory\",\"value\":\"amqp\"},{\"name\":\"is_alarm\",\"value\":\"true\"},{\"name\":\"metricCategory\",\"value\":\"queue\"}]",
				"Dimensions":"userId,regionId,vhostName,queueName",
				"Namespace":"acs_amqp",
				"Periods":"60,300",
				"Unit":"count/min"
			},
			{
				"Description":"The number of messages generated by the instance per minute",
				"MetricName":"VhostMessageInput",
				"Statistics":"Value",
				"Labels":"[{\"name\":\"minAlertPeriod\",\"value\":\"60\"},{\"name\":\"alertDefault\",\"value\":\"\"},{\"name\":\"unitFactor\",\"value\":\"1\"},{\"name\":\"alertUnit\",\"value\":\"count/min\"},{\"name\":\"productCategory\",\"value\":\"amqp\"},{\"name\":\"is_alarm\",\"value\":\"true\"},{\"name\":\"metricCategory\",\"value\":\"vhost\"}]",
				"Dimensions":"userId,regionId,vhostName",
				"Namespace":"acs_amqp",
				"Periods":"60,300",
				"Unit":"count/min"
			},
			{
				"Description":"The number of messages consumed by the instance per minute",
				"MetricName":"VhostMessageOutput",
				"Statistics":"Value",
				"Labels":"[{\"name\":\"minAlertPeriod\",\"value\":\"60\"},{\"name\":\"alertDefault\",\"value\":\"\"},{\"name\":\"unitFactor\",\"value\":\"1\"},{\"name\":\"alertUnit\",\"value\":\"count/min\"},{\"name\":\"productCategory\",\"value\":\"amqp\"},{\"name\":\"is_alarm\",\"value\":\"true\"},{\"name\":\"metricCategory\",\"value\":\"vhost\"}]",
				"Dimensions":"userId,regionId,vhostName",
				"Namespace":"acs_amqp",
				"Periods":"60,300",
				"Unit":"count/min"
			},
			{
				"Description":"",
				"MetricName":"Latency",
				"Statistics":"Average",
				"Labels":"[{\"name\":\"alertUnit\",\"value\":\"ms\"},{\"name\":\"minAlertPeriod\",\"value\":\"60\"},{\"name\":\"metricCategory\",\"value\":\"instanceId\"},{\"name\":\"is_alarm\",\"value\":\"true\"}]",
				"Dimensions":"userId,region,apiUid",
				"Namespace":"acs_apigateway_dashboard",
				"Periods":"60,300,900",
				"Unit":"ms"
			},
			{
				"Description":"",
				"MetricName":"SumQPS",
				"Statistics":"Count",
				"Labels":"[{\"name\":\"alertUnit\",\"value\":\"Count\"},{\"name\":\"minAlertPeriod\",\"value\":\"60\"},{\"name\":\"metricCategory\",\"value\":\"instanceId\"},{\"name\":\"is_alarm\",\"value\":\"true\"}]",
				"Dimensions":"userId,region,apiUid",
				"Namespace":"acs_apigateway_dashboard",
				"Periods":"60,300,900",
				"Unit":"Count"
			},
			{
				"Description":"",
				"MetricName":"TrafficRX",
				"Statistics":"Sum",
				"Labels":"[{\"name\":\"alertUnit\",\"value\":\"KBytes\"},{\"name\":\"minAlertPeriod\",\"value\":\"60\"},{\"name\":\"metricCategory\",\"value\":\"instanceId\"},{\"name\":\"is_alarm\",\"value\":\"true\"}]",
				"Dimensions":"userId,region,apiUid",
				"Namespace":"acs_apigateway_dashboard",
				"Periods":"60,300,900",
				"Unit":"KBytes"
			},
			{
				"Description":"",
				"MetricName":"TrafficTX",
				"Statistics":"Sum",
				"Labels":"[{\"name\":\"alertUnit\",\"value\":\"KBytes\"},{\"name\":\"minAlertPeriod\",\"value\":\"60\"},{\"name\":\"metricCategory\",\"value\":\"instanceId\"},{\"name\":\"is_alarm\",\"value\":\"true\"}]",
				"Dimensions":"userId,region,apiUid",
				"Namespace":"acs_apigateway_dashboard",
				"Periods":"60,300,900",
				"Unit":"KBytes"
			},
			{
				"Description":"",
				"MetricName":"code2XX",
				"Statistics":"Value",
				"Labels":"[{\"name\":\"alertUnit\",\"value\":\"count\"},{\"name\":\"minAlertPeriod\",\"value\":\"60\"},{\"name\":\"metricCategory\",\"value\":\"instanceId\"},{\"name\":\"is_alarm\",\"value\":\"true\"}]",
				"Dimensions":"userId,region,apiUid",
				"Namespace":"acs_apigateway_dashboard",
				"Periods":"60,300",
				"Unit":"count"
			},
			{
				"Description":"",
				"MetricName":"code4XX",
				"Statistics":"Value",
				"Labels":"[{\"name\":\"alertUnit\",\"value\":\"count\"},{\"name\":\"minAlertPeriod\",\"value\":\"60\"},{\"name\":\"metricCategory\",\"value\":\"instanceId\"},{\"name\":\"is_alarm\",\"value\":\"tru\"}]",
				"Dimensions":"userId,region,apiUid",
				"Namespace":"acs_apigateway_dashboard",
				"Periods":"60,300",
				"Unit":"count"
			},
			{
				"Description":"",
				"MetricName":"code5XX",
				"Statistics":"Value",
				"Labels":"[{\"name\":\"alertUnit\",\"value\":\"count\"},{\"name\":\"minAlertPeriod\",\"value\":\"60\"},{\"name\":\"metricCategory\",\"value\":\"instanceId\"},{\"name\":\"is_alarm\",\"value\":\"true\"}]",
				"Dimensions":"userId,region,apiUid",
				"Namespace":"acs_apigateway_dashboard",
				"Periods":"60,300",
				"Unit":"count"
			},
			{
				"Description":"",
				"MetricName":"net_rx.Pkgs",
				"Statistics":"Value",
				"Labels":"[{\"name\":\"alertUnit\",\"value\":\"pps\"},{\"name\":\"minAlertPeriod\",\"value\":\"60\"},{\"name\":\"metricCategory\",\"value\":\"instanceId\"},{\"name\":\"is_alarm\",\"value\":\"true\"}]",
				"Dimensions":"userId,instanceId",
				"Namespace":"acs_bandwidth_package",
				"Periods":"60,300,600",
				"Unit":"pps"
			},
			{
				"Description":"",
				"MetricName":"net_rx.rate",
				"Statistics":"Value",
				"Labels":"[{\"name\":\"alertUnit\",\"value\":\"Mbits/s\"},{\"name\":\"minAlertPeriod\",\"value\":\"60\"},{\"name\":\"unitFactor\",\"value\":\"1048576\"},{\"name\":\"metricCategory\",\"value\":\"instanceId\"},{\"name\":\"is_alarm\",\"value\":\"true\"}]",
				"Dimensions":"userId,instanceId",
				"Namespace":"acs_bandwidth_package",
				"Periods":"60,300,600",
				"Unit":"bits/s"
			},
			{
				"Description":"",
				"MetricName":"net_tx.Pkgs",
				"Statistics":"Value",
				"Labels":"[{\"name\":\"alertUnit\",\"value\":\"pps\"},{\"name\":\"minAlertPeriod\",\"value\":\"60\"},{\"name\":\"metricCategory\",\"value\":\"instanceId\"},{\"name\":\"is_alarm\",\"value\":\"true\"}]",
				"Dimensions":"userId,instanceId",
				"Namespace":"acs_bandwidth_package",
				"Periods":"60,300,600",
				"Unit":"pps"
			},
			{
				"Description":"",
				"MetricName":"net_tx.rate",
				"Statistics":"Value",
				"Labels":"[{\"name\":\"alertUnit\",\"value\":\"Mbits/s\"},{\"name\":\"minAlertPeriod\",\"value\":\"60\"},{\"name\":\"unitFactor\",\"value\":\"1048576\"},{\"name\":\"metricCategory\",\"value\":\"instanceId\"},{\"name\":\"is_alarm\",\"value\":\"true\"}]",
				"Dimensions":"userId,instanceId",
				"Namespace":"acs_bandwidth_package",
				"Periods":"60,300,600",
				"Unit":"bits/s"
			},
			{
				"Description":"",
				"MetricName":"net_tx.ratePercent",
				"Statistics":"Value",
				"Labels":"[{\"name\":\"alertUnit\",\"value\":\"%\"},{\"name\":\"minAlertPeriod\",\"value\":\"60\"},{\"name\":\"metricCategory\",\"value\":\"instanceId\"},{\"name\":\"is_alarm\",\"value\":\"true\"}]",
				"Dimensions":"userId,instanceId",
				"Namespace":"acs_bandwidth_package",
				"Periods":"60,300,600",
				"Unit":"%"
			},
			{
				"Description":"The maximum network traffic per unit time",
				"MetricName":"BPS",
				"Statistics":"Average,Minimum,Maximum",
				"Labels":"[{\"name\":\"alertUnit\",\"value\":\"Mbits/s\"},{\"name\":\"minAlertPeriod\",\"value\":\"60\"},{\"name\":\"unitFactor\",\"value\":\"1000000\"},{\"name\":\"metricCategory\",\"value\":\"instanceId\"},{\"name\":\"unitFactor\",\"value\":\"1000\"},{\"name\":\"is_alarm\",\"value\":\"true\"}]",
				"Dimensions":"userId,instanceId",
				"Namespace":"acs_cdn",
				"Periods":"60,300",
				"Unit":"bits/s"
			},
			{
				"Description":"",
				"MetricName":"GroupBPS",
				"Statistics":"Sum",
				"Labels":"[{\"name\":\"alertUnit\",\"value\":\"Mbits/s\"},{\"name\":\"minAlertPeriod\",\"value\":\"60\"},{\"name\":\"unitFactor\",\"value\":\"1000000\"},{\"name\":\"metricCategory\",\"value\":\"groupId\"},{\"name\":\"unitFactor\",\"value\":\"1000\"}]",
				"Dimensions":"userId,groupId",
				"Namespace":"acs_cdn",
				"Periods":"60,300",
				"Unit":"bits/s"
			},
			{
				"Description":"",
				"MetricName":"GroupInternetOut",
				"Statistics":"Sum",
				"Labels":"[{\"name\":\"alertUnit\",\"value\":\"Mbytes\"},{\"name\":\"minAlertPeriod\",\"value\":\"60\"},{\"name\":\"unitFactor\",\"value\":\"1000000\"},{\"name\":\"metricCategory\",\"value\":\"groupId\"},{\"name\":\"unitFactor\",\"value\":\"1000\"}]",
				"Dimensions":"userId,groupId",
				"Namespace":"acs_cdn",
				"Periods":"60,300",
				"Unit":"bytes"
			},
			{
				"Description":"The downstream traffic",
				"MetricName":"InternetOut",
				"Statistics":"Sum",
				"Labels":"[{\"name\":\"alertUnit\",\"value\":\"Mbytes\"},{\"name\":\"minAlertPeriod\",\"value\":\"60\"},{\"name\":\"unitFactor\",\"value\":\"1000000\"},{\"name\":\"metricCategory\",\"value\":\"instanceId\"},{\"name\":\"unitFactor\",\"value\":\"1000\"},{\"name\":\"is_alarm\",\"value\":\"true\"}]",
				"Dimensions":"userId,instanceId",
				"Namespace":"acs_cdn",
				"Periods":"60,300",
				"Unit":"bytes"
			},
			{
				"Description": "The average number of accesses in the specified time range",
				"MetricName":"QPS",
				"Statistics":"Average,Minimum,Maximum",
				"Labels":"[{\"name\":\"alertUnit\",\"value\":\"Count\"},{\"name\":\"minAlertPeriod\",\"value\":\"60\"},{\"name\":\"metricCategory\",\"value\":\"instanceId\"},{\"name\":\"is_alarm\",\"value\":\"true\"}]",
				"Dimensions":"userId,instanceId",
				"Namespace":"acs_cdn",
				"Periods":"60,300",
				"Unit":"Count"
			},
			{
				"Description":"",
				"MetricName":"UserQPS",
				"Statistics":"Average",
				"Labels":"[{\"name\":\"alertUnit\",\"value\":\"Count\"},{\"name\":\"alertDefault\",\"value\":\"Count\"},{\"name\":\"minAlertPeriod\",\"value\":\"60\"},{\"name\":\"metricCategory\",\"value\":\"userId\"}]",
				"Dimensions":"userId",
				"Namespace":"acs_cdn",
				"Periods":"60,300",
				"Unit":""
			},
			{
				"Description":"",
				"MetricName":"Usercode4xx",
				"Statistics":"Average",
				"Labels":"[{\"name\":\"alertUnit\",\"value\":\"Count\"},{\"name\":\"minAlertPeriod\",\"value\":\"60\"},{\"name\":\"metricCategory\",\"value\":\"userId\"}]",
				"Dimensions":"userId",
				"Namespace":"acs_cdn",
				"Periods":"60",
				"Unit":"Count"
			},
			{
				"Description":"",
				"MetricName":"Usercode5xx",
				"Statistics":"Average",
				"Labels":"[{\"name\":\"alertUnit\",\"value\":\"Count\"},{\"name\":\"minAlertPeriod\",\"value\":\"60\"},{\"name\":\"metricCategory\",\"value\":\"userId\"}]",
				"Dimensions":"userId",
				"Namespace":"acs_cdn",
				"Periods":"60",
				"Unit":"Count"
			},
			{
				"Description":"",
				"MetricName":"UserhitRate",
				"Statistics":"Average",
				"Labels":"[{\"name\":\"alertUnit\",\"value\":\"%\"},{\"name\":\"minAlertPeriod\",\"value\":\"60\"},{\"name\":\"metricCategory\",\"value\":\"userId\"}]",
				"Dimensions":"userId",
				"Namespace":"acs_cdn",
				"Periods":"60",
				"Unit":"%"
			},
			{
				"Description": "The percentage of 4XX HTTP status codes over total status codes in a specified time range",
				"MetricName":"code4xx",
				"Statistics":"Average,Minimum,Maximum",
				"Labels":"[{\"name\":\"alertUnit\",\"value\":\"%\"},{\"name\":\"minAlertPeriod\",\"value\":\"60\"},{\"name\":\"metricCategory\",\"value\":\"instanceId\"},{\"name\":\"is_alarm\",\"value\":\"true\"}]",
				"Dimensions":"userId,instanceId",
				"Namespace":"acs_cdn",
				"Periods":"60,300",
				"Unit":"%"
			}
		]
	}
}
```

## Error codes

For more information, see [Error codes](https://error-center.alibabacloud.com/status/product/Cms).

