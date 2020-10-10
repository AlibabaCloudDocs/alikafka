# DescribeMetricTop

You can call this operation to query the monitoring data on time series metrics of cloud services sorted by specified fields within a specified time.

## Parameters

-   For more information about how to assign values to the parameters such as Project, Metric, Period, and Dimensions for cloud services, you can call the DescribeMetricMetaList operation or see [Preset metric reference](~~28619~~).
-   The period specified by StartTime and EndTime includes the time point specified by StartTime but does not include the time point specified by EndTime. StartTime must be earlier than EndTime.
-   Cursor is the parameter for pagination. If this parameter is null, the returned page is the last page. Otherwise, the returned page is not the last page.
-   The typical values of the Period parameter are 60 \(1 minute\), 300 \(5 minutes\), and 900 \(15 minutes\). You can set this parameter based on related documents or your specific requirements. For example, if you set the Period parameter to 60, only 1,000 records will be returned, though a total of 1,440 records will be generated. This is because the maximum number of records that can be returned for each query is 1,000. If you set Period to 300, then 288 data records will be returned.

## Debugging

Alibaba Cloud provides [OpenAPI Explorer](https://api.aliyun.com/#product=Cms&api=DescribeMetricTop) to simplify API usage. You can use OpenAPI Explorer to search for APIs, call APIs, and dynamically generate SDK example code.

## Request Parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Yes|DescribeMetricTop|The operation that you want to perform. Set this parameter to DescribeMetricTop. |
|MetricName|String|Yes|cpu\_idle|The name of the metric. |
|Namespace|String|Yes|acs\_ecs\_dashboard|The namespace of the monitored service, for example, "acs\_ecs\_dashboard" and "acs\_rds\_dashboard". |
|Dimensions|String|No|\[\{"instanceId": "i-abcdefgh12\*\*\*\*"\}\]|The dimensions of the monitored metrics to filter the records. Each dimension is a key-value pair. The common key-value pair is instanceId: XXXXXX. Dimensions must be organized in a JSON array string, and follow the required order. |
|EndTime|String|No|2019-01-30 00:10:00|The end time of the records. You can set this parameter to the number of milliseconds that have elapsed since 00:00:00 January 1, 1970, or to a timestamp such as 2015-10-20 00:00:00. |
|Express|String|No|\{"groupby":\["userId","instanceId"\]\}|The expression for real-time computation on the query results, such as `{"groupby":["instanceId"]}` |
|Length|String|No|1000|The number of records returned by each query. Default value: 1000. |
|OrderDesc|String|No|False|The sorting order of records. You must specify either this parameter or the Orderby parameter. Valid values:

 -   False: sorts values in descending order.
-   True: sorts values in ascending order. |
|Orderby|String|No|Average|The field by which the records are sorted. You must specify either this parameter or the OrderDesc parameter. |
|Period|String|No|60|The time interval of the records in seconds. Typical values are 60, 300, and 900. If this parameter is not specified, the default period of the metric is used to query the raw data. If this parameter is specified, only data of the specified period is queried. |
|StartTime|String|No|2019-01-30 00:00:00|The start time of the records. You can set this parameter to the number of milliseconds that have elapsed since 00:00:00 January 1, 1970, or to a timestamp such as 2015-10-20 00:00:00. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|Code|String|200|The status code. A value of 200 indicates that the call is successful. |
|Datapoints|String|\[\{"timestamp":1548777660000,"userId":"123","instanceId":"i-abc","Minimum":9.92,"Average":9.92,"Maximum":9.92\}\]|The list of monitoring data in the following format:`{ "timestamp": 1490164200000, "Maximum": 100, "userId": "123456789876****", "Minimum": 4.55, "instanceId": "i-bp18abl200xk9599****", "Average": 93.84 }` |
|Message|String|Success|The error message. No message is returned when the status code is 200. |
|Period|String|60|The time interval of the records in seconds. Typical values are 60, 300, and 900. |
|RequestId|String|3121AE7D-4AFF-4C25-8F1D-C8226EBB1F42|The ID of the request, which can be used for troubleshooting. |

## Examples

Sample request

```

http(s)://[Endpoint]/? Action=DescribeMetricTop
&MetricName=cpu_idle
&Namespace=acs_ecs_dashboard
&<Common request parameters>

```

Sample success response

`XML` format

```
<DescribeMetricTopResponse>
  <Period>60</Period>
  <Datapoints>
    <order>1</order>
    <timestamp>1551687360000</timestamp>
    <userId>12345****</userId>
    <instanceId>i-2zeehst1****</instanceId>
    <Maximum>16.41</Maximum>
    <Minimum>4.66</Minimum>
    <Average>7.74</Average>
    <_count>1</_count>
  </Datapoints>
  <Datapoints>
    <order>2</order>
    <timestamp>1551687360000</timestamp>
    <userId>12345****</userId>
    <instanceId>i-2zefxdy2****</instanceId>
    <Maximum>15.74</Maximum>
    <Minimum>5.03</Minimum>
    <Average>7.14</Average>
    <_count>1</_count>
  </Datapoints>
  <RequestId>1F68A4E8-4488-48E7-9189-3E1F5165E64E</RequestId>
  <Code>200</Code>
</DescribeMetricTopResponse>

```

`JSON` format

```
{
	"Period":"60",
	"Datapoints":[
		{
			"timestamp":1551687360000,
			"order":1,
			"_count":1,
			"Maximum":16.41,
			"userId":"12345****",
			"Minimum":4.66,
			"instanceId":"i-2zeehst1****",
			"Average":7.74
		},
		{
			"timestamp":1551687360000,
			"order":2,
			"_count":1,
			"Maximum":15.74,
			"userId":"12345****",
			"Minimum":5.03,
			"instanceId":"i-2zefxdy2****",
			"Average":7.14
		}
	],
	"RequestId":"1F68A4E8-4488-48E7-9189-3E1F5165E64E",
	"Code":"200"
}
```

## Error codes

For more information, see [Error codes](https://error-center.alibabacloud.com/status/product/Cms).

