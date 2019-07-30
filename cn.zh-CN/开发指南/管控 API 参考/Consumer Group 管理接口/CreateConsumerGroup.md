# CreateConsumerGroup {#concept_94539_zh .concept}

使用 CreateConsumerGroup 接口，根据实例 ID 创建 Consumer Group。

## 使用限制 {#section_xsq_8sb_hxe .section}

该接口有以下使用限制：

-   接口调用受白名单限制，请[提交工单](https://selfservice.console.aliyun.com/ticket/createIndex)申请将用户 ID 加入白名单。

-   单用户请求频率限制为 1 QPS。

-   每个实例下最多可创建 100 个 Consumer Group。


## 请求参数列表 {#section_iwj_kqa_fjh .section}

|名称|类型|是否必需|描述|
|--|--|----|--|
|InstanceId|String|是|实例 ID，可使用 [GetInstanceList](cn.zh-CN/开发指南/管控 API 参考/实例管理接口/GetInstanceList.md#) 获取|
|RegionId|String|是|此实例所在地域（Region），对应的 RegionId 请参见[接入指南](cn.zh-CN/开发指南/管控 API 参考/接入指南.md#)|
|ConsumerId|String|是|创建的 Consumer Group ID，限制 64 个字符|

## 返回参数列表 {#section_coe_blz_0wz .section}

|名称|类型|描述|
|--|--|--|
|RequestId|String|请求的唯一标识 ID|
|Code|Integer|返回码，返回 “200” 为成功|
|Success|Boolean|成功与否|
|Message|String|描述信息|

## 使用示例 {#section_son_wx9_sc3 .section}

该示例为在“华北2（北京）”地域，在某个实例下创建一个 Consumer Group。

``` {#codeblock_gpc_spq_b70}
public static void main(String[] args) {
         //构建 Client
        IAcsClient iAcsClient = buildAcsClient();
        //构造创建 Consumer Group 的 request
        CreateConsumerGroupRequest request = new CreateConsumerGroupRequest();
        request.setAcceptFormat(FormatType.JSON);
        //必要参数，实例 ID
        request.setInstanceId("alikafka_XXXXXXXXXXXXX");
        //必要参数，实例所在地域
        request.setRegionId("cn-beijing");

        //必要参数，Consumer Group ID，限制在 64 个字符以内
        request.setConsumerId("alikafka_for_test_XXXXXX");

        //获取返回值
        try {
            CreateConsumerGroupResponse response = iAcsClient.getAcsResponse(request);
            //log requestId for trace
            String requestId = response.getRequestId();
            if (200  == response.getCode()) {
                if (response.getSuccess()) {
                    //log success
                } else {
                    //log warn
                }
            } else {
                //log warn
            }
        } catch (ClientException e) {
            //log error
        }
    }
    private static IAcsClient buildAcsClient() {
        //产品 Code
        String productName = "alikafka";

        //您的 AccessKeyId 和 AccessKeySecret
        String accessKey = "xxxxxx";
        String secretKey = "xxxxxx";

        //设置接入点相关参数，通常 regionId 值和 endPointName 值相等，接入点也是用对应地域的 domain
        String regionId = "cn-beijing";
        String endPointName = "cn-beijing";
        String domain = "alikafka.cn-beijing.aliyuncs.com";
        try {
            DefaultProfile.addEndpoint(endPointName, regionId, productName, domain);
        } catch (ClientException e) {
            //log error
        }

        //构造 Client
        IClientProfile profile = DefaultProfile.getProfile(regionId, accessKey, secretKey);
        return new DefaultAcsClient(profile);
    }
			
```

