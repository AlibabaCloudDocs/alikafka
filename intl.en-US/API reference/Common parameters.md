# Common parameters

This topic provides the common request parameters and common response parameters of Message Queue for Apache Kafka API operations.

## Common request parameters

Common request parameters must be included in all Message Queue for Apache Kafka API requests.

|Parameter|Type|Required|Description|
|---------|----|--------|-----------|
|Format|String|No|The format in which the response is returned. Valid values: JSON and XML. Default value: XML.|
|Version|String|Yes|The version number of the API in YYYY-MM-DD format. Set the value to 2019-09-16.|
|AccessKeyId|String|Yes|The AccessKey ID provided to you by Alibaba Cloud.|
|Signature|String|Yes|The signature string of the API request.|
|SignatureMethod|String|Yes|The encryption method of the signature string. Set the value to HMAC-SHA1.|
|Timestamp|String|Yes|The timestamp of the request. Specify the time in UTC in the ISO 8601 standard YYYY-MM-DDThh:mm:ssZ format. Example: 2013-01-10T12:00:00Z, which indicates 20:00:00 on January 10, 2013 \(UTC+8\).|
|SignatureVersion|String|Yes|The version of the signature encryption algorithm. Set the value to 1.0.|
|SignatureNonce|String|Yes|A unique, random number that is used to prevent replay attacks. You must use different numbers for different requests.|

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

Every response returns a unique RequestID regardless of whether the request is successful. API responses use a unified format. The HTTP `2xx`response codes indicate a successful call, and the HTTP `4xx` and `5xx` response codes indicate a failed call.

XML format

```
<? xml version="1.0" encoding="UTF-8"? >
<!-The root node of the result-->
<Interface Name+Response>
 | <!--Request ID-->
 | <RequestId>4C467B38-3910-447D-87BC-AC049166F216</RequestId>
 | <!--Response data-->
<Interface Name+Response>
```

JSON format

```
{
    "RequestId": "4C467B38-3910-447D-87BC-AC049166F216"
    /* Response data */
}
```

