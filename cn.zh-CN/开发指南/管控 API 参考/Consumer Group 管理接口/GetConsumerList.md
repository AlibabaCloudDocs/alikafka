# GetConsumerList {#concept_122222_zh .concept}

使用 GetConsumerList 接口，根据实例 ID 获取该实例下的 Consumer Group 列表。

## 请求参数列表 {#section_1aw_8b2_ajm .section}

|名称|类型|是否必需|描述|
|--|--|----|--|
|InstanceId|String|是|实例 ID，可使用 [GetInstanceList](cn.zh-CN/开发指南/管控 API 参考/实例管理接口/GetInstanceList.md#) 获取|
|RegionId|String|是|此实例所在地域，对应的 RegionId 请参见[接入指南](cn.zh-CN/开发指南/管控 API 参考/接入指南.md#)|

## 返回参数列表 {#section_qz5_bsf_ff3 .section}

|名称|类型|描述|
|--|--|--|
|RequestId|String|请求的唯一标识 ID|
|Code|Integer|返回码，返回 “200” 为成功|
|Message|String|描述信息|
|ConsumerList|Array|Consumer Group 列表信息|

|名称|类型|描述|
|--|--|--|
|RegionId|String|地域的 ID|
|InstanceId|Long|实例 ID|
|ConsumerId|String|Consumer Group ID|

## 使用示例 {#section_ikt_mgx_w0a .section}

该示例为在“华北2（北京）”地域，查询某个实例 ID 下的 Consumer Group 列表。

``` {#codeblock_0q7_hal_bg9}
public static void main(String[] args) {
         //构建 Client
        IAcsClient iAcsClient = buildAcsClient();

        //构造获取 consumerList 信息 request
        GetConsumerListRequest request = new GetConsumerListRequest();
        request.setAcceptFormat(FormatType.JSON);

        //必要参数实例 ID
        request.setInstanceId("alikafka_pre-xxxxxx");
        //必要参数，此实例所在地域，必须使用 GetInstanceList 返回值的实例所在的地域
        request.setRegionId("cn-beijing");

        //获取返回值
        try {
            GetConsumerListResponse response = iAcsClient.getAcsResponse(request);
            if (200  == response.getCode()) {
                List<ConsumerListItem> consumerList = response.getConsumerList();
                if (CollectionUtils.isNotEmpty(consumerList)) {
                    for (ConsumerListItem item : consumerList) {
                        String consumerId = item.getConsumerId();
                        System.out.println(consumerId);
                        //......
                    }
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

