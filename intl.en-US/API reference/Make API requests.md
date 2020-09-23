# Make API requests

You can call Message Queue for Apache Kafka API operations by using HTTP, SDKs, and OpenAPI Explorer.

## Call API operations by using HTTP requests

To send a Message Queue for Apache Kafka API request, you must send an HTTP request to the Message Queue for Apache Kafka endpoint. You must add the request parameters that correspond to the API operation being called. After you call the API operation, the system returns a response. The request and response are encoded in UTF-8. Message Queue for Apache Kafka API operations use the RPC protocol. You can call Message Queue for Apache Kafka API operations by sending HTTP requests.

The following request syntax is used:

```
http://Endpoint/?Action=xx&Parameters
```

where:

-   Endpoint: the endpoint of the Message Queue for Apache Kafka API. For more information, see [Endpoints](/intl.en-US/API reference/Endpoints.md).
-   Action: the name of the operation being performed. For example, to query all Message Queue for Apache Kafka instances, you must set the Action parameter to GetInstanceList.
-   Version: the version of the API. For example, the version of the Message Queue for Apache Kafka API is 2019-09-16.
-   Parameters: the request parameters for the operation. Separate multiple parameters with ampersands \(&\). Request parameters include both common parameters and operation-specific parameters. Common parameters include information such as the API version number and authentication information. For more information, see [Common parameters](/intl.en-US/API reference/Common parameters.md).

## Call API operations by using SDKs

Message Queue for Apache Kafka provides SDKs in multiple programming languages. Message Queue for Apache Kafka SDKs can be used to automatically calculate the signature string. For more information, see [Obtain an SDK](/intl.en-US/API reference/API call through the SDK.md).

## Call API operations by using OpenAPI Explorer

OpenAPI Explorer is a visual tool for calling APIs. OpenAPI Explorer allows you to call APIs of Alibaba Cloud services and APIs provided in Alibaba Cloud Marketplace. You can call these APIs on a web page or command-line interface \(CLI\). In addition, OpenAPI Explorer allows you to view the request and response of each API call and dynamically generates SDK sample code. You can directly access [OpenAPI Explorer](https://api.aliyun.com/#/).

