# Call method

Message Queue for Apache KafkaWhen you call an API,Message Queue for Apache KafkaTo send an HTTP request and add the corresponding request parameters to the request according to the API description. After the call, the system returns the processing result. Requests and returned results are encoded using the UTF-8 character set.

## Request structure

Message Queue for Apache KafkaIs an rpc api. You can call the API by sending an HTTP request.Message Queue for Apache KafkaAPI.

The request structure is as follows:

```
http://Endpoint/? Action=xx&Parameters
```

Where:

-   Endpoint:Message Queue for Apache KafkaThe endpoint of the API. The access addresses for different regions are shown in the following table.

    |Region name|RegionId|Domain|
    |-----------|--------|------|
    |Singapore|ap-southeast-1|alikafka.ap-southeast-1.aliyuncs.com|

-   Action: The operation to perform. For example, callGetInstanceListQuery createdMessage Queue for Apache KafkaInstance.
-   Version: The version of the API. Message Queue for Apache KafkaThe API version of IS2019-09-16.
-   Parameters: Request parameters. Separate multiple parameters with ampersands \(&\). A request parameter consists of common request parameters and API-specific parameters. Common parameters include the API version, credentials, and other information. For more information, see[Common parameters](/intl.en-US/API reference/Common parameters.md).

The following is a callGetInstanceListQuery createdMessage Queue for Apache KafkaExample:

**Note:** The following examples have been formatted for ease of viewing.

```
https://alikafka.aliyuncs.com/? Action=GetInstanceList
&Format=JSON
&Version=2019-09-16
&Signature=xxxx%xxxx%%3D
&SignatureMethod=HMAC-SHA1
&SignatureNonce=87dacc12d1a92bb296d2b398b454884b
&SignatureVersion=1.0
&AccessKeyId=key-test
&Timestamp=2020-01-06T09
...
```

## API authorization

To ensure the security of your account, we recommend that you use the identity credentials of the Ram user to call the API. If you use a RAM account to callMessage Queue for Apache KafkaYou must create and attach corresponding authorization policies to the RAM account.

Message Queue for Apache KafkaFor a list of resources and APIs that can be authorized, see[RAM authorization](/intl.en-US/API reference/RAM user authorization.md).

## API Signature

For each HTTP or HTTPS request, we verify the identity of the requester based on the signature information in the request. Specifically, the AccessKeyId and AccessKeySecret of the AccessKey are used for symmetric encryption and verification. Message Queue for Apache KafkaBy using the AccessKey ID and AccessKey Secret, symmetric encryption is performed to authenticate the request sender. An AccessKey is an identity credential issued for Alibaba cloud accounts and RAM users \(similar to a logon password\). The AccessKey ID is used to identify the identity of the visitor. the AccessKey Secret is the key used to encrypt the signature string and verify the signature string on the server. It must be kept strictly confidential. For more information about the API signature method, see[Signature method](/intl.en-US/API reference/Request signatures.md).

