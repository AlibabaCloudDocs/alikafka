# DescribeProjectMeta

You can call this operation to obtain information about the monitored cloud services, including service description, namespace, and tags.

## Debugging

You can use [API Explorer](https://api.aliyun.com/#product=Cms&api=DescribeProjectMeta) to perform debugging. API Explorer allows you to perform various operations to simplify API usage. For example, you can retrieve APIs, call APIs, and dynamically generate SDK example code.

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Yes|DescribeProjectMeta|The operation that you want to perform. Set the value to DescribeProjectMeta. |
|Labels|String|No|\[\{"groupFlag":true\}\]|The tags of the metric in the following format: `[{"name":"tag name","value":"tag value"},{"name":"tag name","value":"tag value"}]`.

 The following tag names are supported:

 product: indicates the cloud service. The value of this tag is a service name.

 groupFlag: indicates whether the service supports application grouping. The value of this tag is true or false. |
|PageNumber|Integer|No|1|The number of the page. Default value: 1. |
|PageSize|Integer|No|30|The number of records on each page. Default value: 30. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|Code|String|200|The status code. A value of 200 indicates that the call is successful. |
|Message|String|success|The error message. |
|PageNumber|String|1|The number of the page. |
|PageSize|String|10|The number of records on each page. |
|RequestId|String|4C2061B2-3B1B-43BF-A4A4-C53426F479C0|The request ID for troubleshooting. |
|Resources| | |The details of the metric. |
|└Description|String|CDN|The description of the metric. |
|└Labels|String|\[\{"groupFlag":true\}\]|The tags of the metric. |
|└Namespace|String|acs\_cdn|The namespace of the monitored service. It is in the acs\_service name format. |
|Success|Boolean|true|Indicates whether the call is successful. A value of true indicates that the call is successful. A value of false indicates that the call has failed. |
|Total|String|12|The total number of returned records. |

## Examples

Sample requests

```

http(s)://[Endpoint]/? Action=DescribeProjectMeta
&<Common request parameters>

```

Successful response examples

`XML` format

```
<DescribeProjectMetaResponse>
  <PageNumber>1</PageNumber> 
  <PageSize>5</PageSize>
  <RequestId>4C2061B2-3B1B-43BF-A4A4-C53426F479C0</RequestId>
  <Success>true</Success> 
  <Code>200</Code>
  <Total>62</Total>
  <Resources>
    <Resource>
      <Description>AnalyticDB</Description>
      <Labels>[{"name":"product","value":"ADS"},{"name":"productCategory","value":"ads"},{"name":"groupFlag","value":"true"}]</Labels>
      <Namespace>acs_ads</Namespace> 
    </Resource>
    <Resource>
      <Description>Artificial Intelligence Recommendation</Description>
      <Labels>[{"name":"product","value":"AIRec"},{"name":"productCategory","value":"airec"},{"name":"groupFlag","value":"true"}]</Labels>
      <Namespace>acs_airec</Namespace>
    </Resource>
    <Resource>
      <Description>API Gateway</Description>
      <Labels>[{"name":"product","value":"APIGateway"},{"name":"productCategory","value":"apigateway"},{"name":"groupFlag","value":"true"}]</Labels> 
      <Namespace>acs_apigateway_dashboard</Namespace>
    </Resource>
    <Resource>
      <Description>CDN</Description>
      <Labels>[{"name":"product","value":"CDN"},{"name":"productCategory","value":"cdn"},{"name":"groupFlag","value":"true"}]</Labels>
      <Namespace>acs_cdn</Namespace>
    </Resource>
    <Resource>
      <Description>Cloud Enterprise Network</Description>
      <Labels>[{"name":"product","value":"CEN"},{"name":"productCategory","value":"cen,cen_flow,cen_vbr"},{"name":"groupFlag","value":"true"}]</Labels>
      <Namespace>acs_cen</Namespace>
    </Resource>
  </Resources>
</DescribeProjectMetaResponse>

```

`JSON` format

```
{
	"PageNumber":1,
	"PageSize":5,
	"RequestId":"4C2061B2-3B1B-43BF-A4A4-C53426F479C0",
	"Success":true,
	"Code":200,
	"Total":62,
	"Resources":{
		"Resource":[
			{
				"Description":"AnalyticDB",
				"Labels":"[{\"name\":\"product\",\"value\":\"ADS\"},{\"name\":\"productCategory\",\"value\":\"ads\"},{\"name\":\"groupFlag\",\"value\":\"true\"}]",
				"Namespace":"acs_ads"
			},
			{
				"Description":"Artificial Intelligence Recommendation",
				"Labels":"[{\"name\":\"product\",\"value\":\"AIRec\"},{\"name\":\"productCategory\",\"value\":\"airec\"},{\"name\":\"groupFlag\",\"value\":\"true\"}]",
				"Namespace":"acs_airec"
			},
			{
				"Description":"API Gateway",
				"Labels":"[{\"name\":\"product\",\"value\":\"APIGateway\"},{\"name\":\"productCategory\",\"value\":\"apigateway\"},{\"name\":\"groupFlag\",\"value\":\"true\"}]",
				"Namespace":"acs_apigateway_dashboard"
			},
			{
				"Description":"CDN",
				"Labels":"[{\"name\":\"product\",\"value\":\"CDN\"},{\"name\":\"productCategory\",\"value\":\"cdn\"},{\"name\":\"groupFlag\",\"value\":\"true\"}]",
				"Namespace":"acs_cdn"
			},
			{
				"Description":"Cloud Enterprise Network",
				"Labels":"[{\"name\":\"product\",\"value\":\"CEN\"},{\"name\":\"productCategory\",\"value\":\"cen,cen_flow,cen_vbr\"},{\"name\":\"groupFlag\",\"value\":\"true\"}]",
				"Namespace":"acs_cen"
			}
		]
	}
}
```

## Error code

[View error code](https://error-center.aliyun.com/status/product/Cms)

