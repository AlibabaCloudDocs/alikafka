# Common parameters

This topic provides the common request parameters and common response parameters of the Message Queue for Apache Kafka API operations.

## Common request parameters

Common request parameters must be included in all Message Queue for Apache Kafka API requests.

|Parameter|Type|Required|Description|
|---------|----|--------|-----------|
|Format|String|No|The format in which to return the response. Valid values: JSON and XML. Default value: XML.|
|Version|String|Yes|The version number of the API. The value must be in the format of YYYY-MM-DD, for example, 2019-09-16.|
|AccessKeyId|String|Yes|The AccessKey ID provided to you by Alibaba Cloud.|
|Signature|String|Yes|The signature string of the current request.|
|SignatureMethod|String|Yes|The encryption method of the signature string. Set the value to HMAC-SHA1.|
|Timestamp|String|Yes|The timestamp of the request. Specify the time in the ISO 8601 standard in the yyyy-MM-ddTHH:mm:ssZ format. The time must be in UTC. For example, 20:00:00 on January 10, 2013 \(UTC+8\) is written as 2013-01-10T12:00:00Z.|
|SignatureVersion|String|Yes|The version of the signature encryption algorithm. Set the value to 1.0.|
|SignatureNonce|String|Yes|A unique, random number used to prevent replay attacks. You must use different numbers for different requests.|

Sample requests

```
https:/alikafka.cn-hangzhou.aliyuncs.com/? Action=GetInstanceList
&Format=JSON
&Version=2019-09-16
&AccessKeyId=key-test
&Signature=Pc5WB8gokVn0xfeu%2FZV%2BiNM1dgI%3D
&SignatureMethod=HMAC-SHA1
&Timestamp=2020-01-01T12:00:00Z
&SignatureNonce=15215528852396
&SignatureVersion=1.0
...            
```

## Common response parameters

Every response returns a unique RequestID regardless of whether the call is successful. Responses can be returned in a unified format. API responses use the HTTP response format where a `2xx` status code indicates a successful call and a `4xx` or `5xx` status code indicates a failed call.

XML format

```
<? xml version="1.0" encoding="UTF-8"? >
<!--Result Root Node--> <Interface Name+Response> | <!--Return Request Tag-->
 | <RequestId>4C467B38-3910-447D-87BC-AC049166F216</RequestId>
 | <!--Return Result Data-->
</Interface Name+Response>
```

JSON format

```
{    "RequestId": "4C467B38-3910-447D-87BC-AC049166F216"
    /* Return Result Data */
}
```

