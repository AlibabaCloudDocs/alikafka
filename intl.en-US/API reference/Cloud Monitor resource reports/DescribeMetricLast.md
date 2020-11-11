# DescribeMetricLast

Queries the latest monitoring data of a metric.

For more information about how to assign values to the Project, Metric, Period, and Dimensions parameters for cloud services, see [DescribeMetricMetaList](~~98846~~) or [Appendix 1: Metrics](~~163515~~).

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=Cms&api=DescribeMetricLast&type=RPC&version=2019-01-01)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Yes|DescribeMetricLast|The operation that you want to perform. Set the value to DescribeMetricLast. |
|MetricName|String|Yes|CPUUtilization|The name of the metric that you want to query. |
|Namespace|String|Yes|acs\_ecs\_dashboard|The namespace of the service.

 Specify the value in the format of acs\_Service. |
|Period|String|No|60|The time interval to query monitoring data. This value is typically the same as the interval for reporting metric data. Unit: seconds.

 **Note:** If the interval for collecting metric data is not specified in the alert rule, raw data is queried at the interval for reporting data of the metric. If the interval for collecting metric data is specified in the alert rule, statistical data is queried at the specified interval. |
|StartTime|String|No|2019-01-31 10:00:00|The beginning of the time range to query.

 The value is a UNIX timestamp representing the number of milliseconds that have elapsed since January 1, 1970, 00:00:00 UTC.

 **Note:** The time range cannot exceed 31 days. The time range must be within the last 270 days. |
|EndTime|String|No|2019-01-31 10:10:00|The end of the time range to query.

 The value is a UNIX timestamp representing the number of milliseconds that have elapsed since January 1, 1970, 00:00:00 UTC.

 **Note:** The time range must be within the last 270 days. |
|Dimensions|String|No|\[\{"instanceId":"i-abcdefgh12\*\*\*\*"\}\]|The dimensions that specify the resources for which you want to query monitoring data.

 Set the value to a collection of `key-value` pairs. A typical `key-value` pair is `i-abcdefgh12****`.

 The `key` and `value` can each be 1 to 64 bytes in length. Values that contain more than 64 characters will be truncated. The `key` and `value` can contain letters, digits, periods \(.\), hyphens \(-\), underscores \(\_\), forward slashes \(/\), and backslashes \(\\\).

 **Note:** `Dimensions` must be organized in a JSON string and follow the required order. |
|NextToken|String|No|15761432850009dd70bb64cff1f0fff6c0b08ffff073be5fb1e785e2b020f7fed9b5e137bd810a6d6cff5ae\*\*\*\*|The pagination cursor.

 -   If the number of entries that match the search criteria exceeds the maximum number allowed on a single page, a pagination cursor is returned.
-   This pagination cursor can be used as an input parameter to obtain entries on the next page. If a response does not contain a pagination cursors, all query results have been returned. |
|Length|String|No|1000|The number of entries to return on each page.

 Default value: 1000. |
|Express|String|No|\{"groupby":\["userId","instanceId"\]\}|The expression for real-time computation on the query results. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|021472A6-25E3-4094-8D00-BA4B6A5486C3|The ID of the request. |
|Code|String|200|The response code.

 **Note:** The HTTP 200 code indicates that the request was successful. |
|Success|Boolean|true|Indicates whether the request was successful. The value true indicates success. The value false indicates failure. |
|Period|String|60|The time interval at which monitoring data was queried. Unit: seconds. |
|NextToken|String|xxxxxx|The pagination cursor. |
|Datapoints|String|\[\{"timestamp":1548777660000,"userId":"123456789876\*\*\*\*","instanceId":"i-abcdefgh12\*\*\*\*","Minimum":9.92,"Average":9.92,"Maximum":9.92\}\]|The monitoring data of the metric. |
|Message|String|The Request is not authorization.|The error message. |

## Examples

Sample requests

```
http(s)://[Endpoint]/? Action=DescribeMetricLast
&MetricName=CPUUtilization
&Namespace=acs_ecs_dashboard
&<Common request parameters>
```

Sample success responses

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
    "Period": "60",
    "Datapoints": [
        {
            "timestamp": 1490152860000,
            "Maximum": 100,
            "userId": "123456789876****",
            "Minimum": 93.1,
            "instanceId": "i-abcdefgh12****",
            "Average": 99.52
        },
        {
            "timestamp": 1490152920000,
            "Maximum": 100,
            "userId": "123456789876****",
            "Minimum": 92.59,
            "instanceId": "i-abcdefgh12****",
            "Average": 99.49
        },
        {
            "timestamp": 1490152980000,
            "Maximum": 100,
            "userId": "123456789876****",
            "Minimum": 92.86,
            "instanceId": "i-abcdefgh12****",
            "Average": 99.44
        },
        {
            "timestamp": 1490153040000,
            "Maximum": 100,
            "userId": "123456789876****",
            "Minimum": 91.43,
            "instanceId": "i-abcdefgh12****",
            "Average": 99.36
        },
        {
            "timestamp": 1490153100000,
            "Maximum": 100,
            "userId": "123456789876****",
            "Minimum": 93.55,
            "instanceId": "i-abcdefgh12****",
            "Average": 99.51
        },
        {
            "timestamp": 1490153160000,
            "Maximum": 100,
            "userId": "123456789876****",
            "Minimum": 93.1,
            "instanceId": "i-abcdefgh12****",
            "Average": 99.52
        },
        {
            "timestamp": 1490153220000,
            "Maximum": 100,
            "userId": "123456789876****",
            "Minimum": 92.59,
            "instanceId": "i-abcdefgh12****",
            "Average": 99.42
        },
        {
            "timestamp": 1490153280000,
            "Maximum": 100,
            "userId": "123456789876****",
            "Minimum": 91.18,
            "instanceId": "i-abcdefgh12****",
            "Average": 99.34
        },
        {
            "timestamp": 1490153340000,
            "Maximum": 100,
            "userId": "123456789876****",
            "Minimum": 92.86,
            "instanceId": "i-abcdefgh12****",
            "Average": 99.46
        },
        {
            "timestamp": 1490153400000,
            "Maximum": 100,
            "userId": "123456789876****",
            "Minimum": 91.18,
            "instanceId": "i-abcdefgh12****",
            "Average": 99.35
        }
    ],
    "RequestId": "6A5F022D-AC7C-460E-94AE-B9E75083D027",
    "Success": true,
    "Code": "200"
}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/Cms).

