# DescribeMetricList

You can call this operation to query the monitoring data on time series metrics of cloud services in a specified period in time.

## Parameter description

-   For more information about how to assign values to the Project, Metric, Period, and Dimensions parameters for cloud services, call DescribeMetricMetaList or see [Preset metrics reference](~~28619~~).
-   The period specified by StartTime and EndTime includes the time point specified by EndTime but does not include the time point specified by StartTime. StartTime cannot be the same as or later than EndTime.
-   Cursor is a paging parameter. If this parameter is null, the current page is the last page. Otherwise, the current page is not the last page.
-   The typical values of Period is 60 \(1 minute\), 300 \(5 minutes\), and 900 \(15 minutes\). You can set this parameter based on related documents or your actual needs. For example, if you set Period to 60, 1,000 instead of 1,440 data records will be returned each day, because the maximum number of records that can be returned each day is 1,000. If you set Period to 300, 288 data records will be returned.

## Debugging

You can use [API Explorer](https://api.aliyun.com/#product=Cms&api=DescribeMetricList) to perform debugging. API Explorer allows you to perform various operations to simplify API usage. For example, you can retrieve APIs, call APIs, and dynamically generate SDK example code.

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Yes|DescribeMetricList|The operation that you want to perform. Set the value to DescribeMetricList. |
|MetricName|String|Yes|cpu\_idle|The name of the metric. |
|Namespace|String|Yes|acs\_ecs\_dashboard|The namespace of the monitored service. |
|Dimensions|String|No|\[\{"instanceId":"i-abcdefgh12\*\*\*\*"\}\]|The resources to be monitored in the key-value format. The common key-value set is "instanceId:XXXXXX." If you hope to see a key-value map expressed by JSON strings, set Dimensions to ordered strings. |
|EndTime|String|No|2019-01-30 00:10:00|The end time. It can be the time that has passed since 00:00:00, January 1, 1970 in milliseconds, or a time in the 2015-10-20 00:00:00 format. |
|Express|String|No|\{"groupby":\["userId","instanceId"\]\}|The expression for real-time computation based on the existing results, such as `{"groupby":["instanceId"]}` . |
|Length|String|No|1000|The number of records returned by each query. It is used in paged queries. Default value: 1000. |
|NextToken|String|No|xxxxxxxxxxxx|The cursor used for paging. |
|Period|String|No|60|The time interval at which monitoring data is queried. Unit: second. If this parameter is not specified, raw data is queried at the minimum report period of the corresponding metric. If this parameter is specified, raw data is queried at the specified period. |
|StartTime|String|No|2019-01-30 00:00:00|The start time. It can be the time that has passed since 00:00:00, January 1, 1970 in milliseconds, or a time in the 2015-10-20 00:00:00 format. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|3121AE7D-4AFF-4C25-8F1D-C8226EBB1F42|The request ID for troubleshooting. |
|Message|String|Success|The error message. This parameter is null when Code is 200. |
|Datapoints|String|\[\{"timestamp":1548777660000,"userId":"123","instanceId":"i-abc","Minimum":9.92,"Average":9.92,"Maximum":9.92\}\]|The list of monitoring data in the following format: `{ “timestamp”: 1490164200000, “Maximum”:100, “userId”:“123456789876****”, “Minimum”:4.55, “instanceId”:“i-bp18abl200xk9599****”, “Average”:93.84 }`. |
|NextToken|String|xxxxxxxxxxxx|The cursor used for paging. |
|Period|String|60|The time interval at which monitoring data is queried. Unit: second. |
|Code|String|200|The status code. A value of 200 indicates that the call is successful. |
|Success|String|true|Indicates whether an operation is performed successfully. If there is an exception on the server, the value "false" is returned. If no exception occurs, the value "true" is returned. |

## Examples

Sample requests

```

http(s)://[Endpoint]/? Action=DescribeMetricList
&MetricName=cpu_idle
&Namespace=acs_ecs_dashboard 
&<Common request parameters>

```

Successful response examples

`XML` format

```
<QueryMetricListResponse>
  <Period>60</Period>
  <Datapoints>
    <Datapoints>
      <timestamp>1490152860000</timestamp>
      <Maximum>100</Maximum>
      <userId> 123456789876****</userId>
      <Minimum>93.1</Minimum>
      <instanceId>i-abcdefgh12****</instanceId>
      <Average>99.52</Average>
    </Datapoints>
    <Datapoints>
      <timestamp>1490152920000</timestamp>
      <Maximum>100</Maximum>
      <userId> 123456789876**** </userId>
      <Minimum>92.59</Minimum>
      <instanceId>i-abcdefgh12****</instanceId>
      <Average>99.49</Average>
    </Datapoints>
    <Datapoints>
      <timestamp>1490152980000</timestamp>
      <Maximum>100</Maximum>
      <userId>123456789876****</userId>
      <Minimum>92.86</Minimum>
      <instanceId>i-abcdefgh12****</instanceId>
      <Average>99.44</Average>
    </Datapoints>
    <Datapoints>
      <timestamp>1490153040000</timestamp>
      <Maximum>100</Maximum>
      <userId>123456789876****</userId>
      <Minimum>91.43</Minimum>
      <instanceId>i-abcdefgh12****</instanceId>
      <Average>99.36</Average>
    </Datapoints>
    <Datapoints>
      <timestamp>1490153100000</timestamp>
      <Maximum>100</Maximum>
      <userId>123456789876****</userId>
      <Minimum>93.55</Minimum>
      <instanceId>i-abcdefgh12****</instanceId>
      <Average>99.51</Average>
    </Datapoints>
    <Datapoints>
      <timestamp>1490153160000</timestamp>
      <Maximum>100</Maximum>
      <userId>123456789876****</userId>
      <Minimum>93.1</Minimum>
      <instanceId>i-abcdefgh12****</instanceId>
      <Average>99.52</Average>
    </Datapoints>
    <Datapoints>
      <timestamp>1490153220000</timestamp>
      <Maximum>100</Maximum>
      <userId>123456789876****</userId>
      <Minimum>92.59</Minimum>
      <instanceId>i-abcdefgh12****</instanceId>
      <Average>99.42</Average>
    </Datapoints>
    <Datapoints>
      <timestamp>1490153280000</timestamp>
      <Maximum>100</Maximum>
      <userId>123456789876****</userId>
      <Minimum>91.18</Minimum>
      <instanceId>i-abcdefgh12****</instanceId>
      <Average>99.34</Average>
    </Datapoints>
    <Datapoints>
      <timestamp>1490153340000</timestamp>
      <Maximum>100</Maximum>
      <userId>123456789876****</userId>
      <Minimum>92.86</Minimum>
      <instanceId>i-abcdefgh12****</instanceId>
      <Average>99.46</Average>
    </Datapoints>
    <Datapoints>
      <timestamp>1490153400000</timestamp>
      <Maximum>100</Maximum>
      <userId>123456789876****</userId>
      <Minimum>91.18</Minimum>
      <instanceId>i-abcdefgh12****</instanceId>
      <Average>99.35</Average>
    </Datapoints>
  </Datapoints>
  <RequestId>6661EC50-8625-4161-B349-E0DD59002AB7</RequestId>
  <Success>true</Success>
  <Code>200</Code>
</QueryMetricListResponse>

```

`JSON` format

```
{
	"Period":"60",
	"Datapoints":[
		{
			"timestamp":1490152860000,
			"Maximum":100,
			"userId":"123456789876****",
			"instanceId":"i-abcdefgh12****",
			"Minimum":93.1,
			"Average":99.52
		},
		{
			"timestamp":1490152920000,
			"Maximum":100,
			"userId":"123456789876****",
			"instanceId":"i-abcdefgh12****",
			"Minimum":92.59,
			"Average":99.49
		},
		{
			"timestamp":1490152980000,
			"Maximum":100,
			"userId":"123456789876****",
			"instanceId":"i-abcdefgh12****",
			"Minimum":92.86,
			"Average":99.44
		},
		{
			"timestamp":1490153040000,
			"Maximum":100,
			"userId":"123456789876****",
			"instanceId":"i-abcdefgh12****",
			"Minimum":91.43,
			"Average":99.36
		},
		{
			"timestamp":1490153100000,
			"Maximum":100,
			"userId":"123456789876****",
			"instanceId":"i-abcdefgh12****",
			"Minimum":93.55,
			"Average":99.51
		},
		{
			"timestamp":1490153160000,
			"Maximum":100,
			"userId":"123456789876****",
			"instanceId":"i-abcdefgh12****",
			"Minimum":93.1,
			"Average":99.52
		},
		{
			"timestamp":1490153220000,
			"Maximum":100,
			"userId":"123456789876****",
			"instanceId":"i-abcdefgh12****",
			"Minimum":92.59,
			"Average":99.42
		},
		{
			"timestamp":1490153280000,
			"Maximum":100,
			"userId":"123456789876****",
			"instanceId":"i-abcdefgh12****",
			"Minimum":91.18,
			"Average":99.34
		},
		{
			"timestamp":1490153340000,
			"Maximum":100,
			"userId":"123456789876****",
			"instanceId":"i-abcdefgh12****",
			"Minimum":92.86,
			"Average":99.46
		},
		{
			"timestamp":1490153400000,
			"Maximum":100,
			"userId":"123456789876****",
			"instanceId":"i-abcdefgh12****",
			"Minimum":91.18,
			"Average":99.35
		}
	],
	"RequestId":"6A5F022D-AC7C-460E-94AE-B9E75083D027",
	"Success":true,
	"Code":"200"
}
```

## Error code

[View error codes](https://error-center.aliyun.com/status/product/Cms)

