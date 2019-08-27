# GetTopicList {#concept_122193_zh .concept}

使用 GetTopicList 接口，根据实例 ID 获取该实例下的 Topic 列表。

## 请求参数列表 {#section_c5g_zjd_5wu .section}

|名称|类型|是否必需|描述|
|--|--|----|--|
|InstanceId|String|是|实例 ID，可使用 [GetInstanceList](cn.zh-CN/开发指南/管控 API 参考/实例管理接口/GetInstanceList.md#) 获取|
|RegionId|String|是|此实例所在地域（Region），对应的 RegionId 请参见[接入指南](cn.zh-CN/开发指南/管控 API 参考/接入指南.md#)|

## 返回参数列表 {#section_atu_aok_kwz .section}

|名称|类型|描述|
|--|--|--|
|RequestId|String|请求的唯一标识 ID|
|Code|Integer|返回码，返回 “200” 为成功|
|Message|String|描述信息|
|TopicList|Array|实例列表信息|

|名称|类型|描述|
|--|--|--|
|Topic|String|Topic 名称|
|CreateTime|Long|Topic 创建时间|
|Remark|String|Topic 备注|
|Status|Integer|状态码，取值说明如下: -   “0” 代表服务中；
-   “1” 代表冻结；
-   “2” 代表暂停。

 |
|InstanceId|String|实例 ID|
|RegionId|String|实例所在地域的 ID|
|StatusName|String|状态描述|

## 使用示例 {#section_xyq_k9k_8g0 .section}

该示例为在“华北2（北京）”地域，查询某个实例下的 Topic 列表。

``` {#codeblock_k5r_vnw_vag}
public static void main(String[] args) {
         //构建 Client
        IAcsClient iAcsClient = buildAcsClient();

        //构造获取 topicList 信息的 request
        GetTopicListRequest request = new GetTopicListRequest();
        request.setAcceptFormat(FormatType.JSON);

        //必要参数实例 ID
        request.setInstanceId("alikafka_pre-xxxxxxxxxxxx");
        //必要参数，此实例所在地域，必须使用 GetInstanceList 返回值的实例所在的地域
        request.setRegionId("cn-beijing");

        //获取返回值
        try {
            GetTopicListResponse response = iAcsClient.getAcsResponse(request);
            if (200  == response.getCode()) {
                List<GetTopicListResponse.TopicListItem> topicListItems = response.getTopicList();
                if (CollectionUtils.isNotEmpty(topicListItems)) {
                    for (TopicListItem item : topicListItems) {
                        String topic = item.getTopic();
                        System.out.println(topic);
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

