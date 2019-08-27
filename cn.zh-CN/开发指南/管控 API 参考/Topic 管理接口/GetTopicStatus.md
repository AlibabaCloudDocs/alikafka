# GetTopicStatus {#concept_122194_zh .concept}

使用 GetTopicStatus 接口，根据实例 ID 和 Topic 名称来查询某 Topic 的消息收发数据。

## 请求参数列表 {#section_ie1_s84_bh8 .section}

|名称|类型|是否必需|描述|
|--|--|----|--|
|RegionId|String|是|此实例所在地域（Region），对应的 RegionId 请参见[接入指南](cn.zh-CN/开发指南/管控 API 参考/接入指南.md#)|
|InstanceId|String|是|实例 ID，可使用 [GetInstanceList](cn.zh-CN/开发指南/管控 API 参考/实例管理接口/GetInstanceList.md#) 获取|
|Topic|String|是|Topic 名称，可使用 [GetTopicList](cn.zh-CN/开发指南/管控 API 参考/Topic 管理接口/GetTopicList.md#) 获取|

## 返回参数列表 {#section_g50_hf6_q2l .section}

|名称|类型|描述|
|--|--|--|
|RequestId|String|请求唯一标识 ID|
|Code|Integer|返回码，返回 “200” 代表成功|
|Message|String|描述信息|
|TopicStatus|Struct|TopicStatus 的数据结构|

|名称|类型|描述|
|--|--|--|
|TotalCount|Long|消息总数|
|LastTimeStamp|Long|最后被消费的一条消息的产生的时间|
|OffsetTable|Array|Offset 列表|

|名称|类型|描述|
|--|--|--|
|Topic|String|Topic 名称|
|Partition|Integer|分区的 ID|
|MinOffset|Long|消息最小位点|
|MaxOffset|Long|消息最大位点|
|LastUpdateTimestamp|Long|最后被消费的一条消息的产生时间|

## 使用示例 {#section_xaz_kko_xnn .section}

该示例为在“华北2（北京）”地域，查询某实例下的某个 Topic 的消息收发信息。

``` {#codeblock_me4_2zu_9fn}
public static void main(String[] args) {
         //构建 Client
        IAcsClient iAcsClient = buildAcsClient();

        //构造获取 Topic 信息的 request
        GetTopicStatusRequest request = new GetTopicStatusRequest();
        request.setAcceptFormat(FormatType.JSON);


        //必要参数，实例 ID
        request.setInstanceId("alikafka_pre-xxxxxx");
         //必要参数，此实例所在地域，必须使用 GetInstanceList 返回值的实例所在的地域
        request.setRegionId("cn-beijing");
        //必要参数，Topic 名称
        request.setTopic("test-xxxxxx");

        //获取返回值
        try {
            GetTopicStatusResponse response = iAcsClient.getAcsResponse(request);
            if (200  == response.getCode()) {
                GetTopicStatusResponse.TopicStatus topicStatus = response.getTopicStatus();
                if (topicStatus != null) {
                    Long totalCount = topicStatus.getTotalCount();
                    System.out.println(totalCount);
                    for (GetTopicStatusResponse.TopicStatus.OffsetTableItem item : topicStatus.getOffsetTable()) {
                        item.getTopic();
                        item.getPartition();
                        item.getMaxOffset();
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

