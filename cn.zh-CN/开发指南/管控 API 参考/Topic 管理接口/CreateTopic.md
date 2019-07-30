# CreateTopic {#concept_94538_zh .concept}

使用 CreateTopic 接口，根据实例 ID 创建 Topic。

## 使用限制 {#section_l9p_y9j_ml2 .section}

该接口有以下使用限制：

-   接口调用受白名单限制，请[提交工单](https://selfservice.console.aliyun.com/ticket/createIndex)申请将用户 ID 加入白名单。

-   单用户请求频率限制为 1 QPS。

-   每个实例下最多可创建的 Topic 数量与您所购买的实例版本相关。详情请参见[计费说明](../../../../cn.zh-CN/产品定价/计费说明.md#)。


## 请求参数列表 {#section_n9p_o6e_siu .section}

|名称|类型|是否必需|描述|
|--|--|----|--|
|InstanceId|String|是|实例 ID，可使用 [GetInstanceList](cn.zh-CN/开发指南/管控 API 参考/实例管理接口/GetInstanceList.md#) 获取|
|RegionId|String|是|此实例所在地域（Region），对应的 RegionId 请参见[接入指南](cn.zh-CN/开发指南/管控 API 参考/接入指南.md#)|
|Topic|String|是|创建的 Topic 名称，限制 64 个字符|
|Remark|String|是|创建的 Topic 的备注，限制 64个字符|

## 返回参数列表 {#section_id4_ohf_kg3 .section}

|名称|类型|描述|
|--|--|--|
|RequestId|String|请求的唯一标识 ID|
|Code|Integer|返回码，返回 “200” 为成功|
|Success|Boolean|是否成功|
|Message|String|描述信息|

## 使用示例 {#section_qqz_qby_k4x .section}

该示例为在“华北2（北京）”地域的某实例下创建一个 Topic。

``` {#codeblock_jne_0zb_h5a}
public static void main(String[] args) {
         //构建 Client
        IAcsClient iAcsClient = buildAcsClient();
        //构造创建 Topic 信息的 request
        CreateTopicRequest request = new CreateTopicRequest();
        request.setAcceptFormat(FormatType.JSON);
        //必要参数，实例 ID
        request.setInstanceId("alikafka_XXXXXXXXXXXXXX");
        //必要参数，实例所在的地域
        request.setRegionId("cn-beijing");

        //必要参数 Topic 名称，限制在 64 个字符以内
        request.setTopic("alikafka_for_test_1");
        //必要参数 Topic 备注，限制在 64 个字符以内
        request.setRemark("myTest");

        //获取返回值
        try {
            CreateTopicResponse response = iAcsClient.getAcsResponse(request);
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

