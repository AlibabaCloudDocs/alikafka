# API call through the SDK

Message Queue for Apache Kafka provides API operations to query information about instances, topics, and consumer groups.

This topic uses the Java SDK as an example to describe how to obtain the Message Queue for Apache Kafka SDK, set common parameters, and use the SDK.

## Obtain the SDK

-   Obtain the SDK by using a project build tool:



-   Obtain the SDK by downloading dependency JAR packages:
    -   [aliyun-java-sdk-core-4.4.9.jar](https://repo1.maven.org/maven2/com/aliyun/aliyun-java-sdk-core/4.4.9/aliyun-java-sdk-core-4.4.9.jar)
    -   [aliyun-java-sdk-alikafka-1.2.4.jar](https://repo1.maven.org/maven2/com/aliyun/aliyun-java-sdk-alikafka/1.2.4/aliyun-java-sdk-alikafka-1.2.4.jar)

## Set common parameters

To construct and start the client, you need to configure a series of parameters, as shown in the following example:

```
public static void main(String[] args) {
    // Build a client.
    IAcsClient iAcsClient = buildAcsClient();
 }
 private static IAcsClient buildAcsClient() {
        // The AccessKey ID used for authentication. You can obtain it in the Alibaba Cloud console.
        String accessKey = "XXXXXX";
        // The AccessKey secret used for authentication. You can obtain it in the Alibaba Cloud console.
        String secretKey = "XXXXXX";

        // Product code. The product code of Message Queue for Apache Kafka is the constant value "alikafka".
        String productName = "alikafka";

        // The region where the API gateway is located. Currently, cn-beijing and cn-hangzhou are supported.
        String regionId = "cn-beijing";
        // The name of the endpoint. It is the same as the region ID.
        String endPointName = "cn-beijing";
        // The domain name corresponding to the endpoint.
        String domain = "alikafka.cn-beijing.aliyuncs.com";

        try {
            DefaultProfile.addEndpoint(endPointName, regionId, productName, domain);
        } catch (ClientException e) {
            //log error
        }
        // Construct a client.
        IClientProfile profile = DefaultProfile.getProfile(regionId, accessKey, secretKey);
        return new DefaultAcsClient(profile);
}        
```

## Endpoints

|Region|RegionId|Domain|
|------|--------|------|
|China \(Hangzhou\)|cn-hangzhou|alikafka.cn-hangzhou.aliyuncs.com|
|China \(Shanghai\)|cn-shanghai|alikafka.cn-shanghai.aliyuncs.com|
|China \(Qingdao\)|cn-qingdao|alikafka.cn-qingdao.aliyuncs.com|
|China \(Beijing\)|cn-beijing|alikafka.cn-beijing.aliyuncs.com|
|China \(Zhangjiakou-Beijing Winter Olympics\)|cn-zhangjiakou|alikafka.cn-zhangjiakou.aliyuncs.com|
|China \(Hohhot\)|cn-huhehaote|alikafka.cn-huhehaote.aliyuncs.com|
|China \(Shenzhen\)|cn-shenzhen|alikafka.cn-shenzhen.aliyuncs.com|
|China \(Hong Kong\)|cn-hongkong|alikafka.cn-hongkong.aliyuncs.com|
|Singapore \(Singapore\)|ap-southeast-1|alikafka.ap-southeast-1.aliyuncs.com|

## Limits

The queries per second \(QPS\) limit on a single API operation is 3 per user.

