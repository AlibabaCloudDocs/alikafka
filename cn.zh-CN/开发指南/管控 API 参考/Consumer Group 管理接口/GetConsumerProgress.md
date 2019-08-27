# GetConsumerProgress {#concept_122223_zh .concept}

使用 GetConsumerProgress 接口，根据实例 ID 和 Consumer Group ID 查询该 Consumer Group 的消费进度信息。

## 请求参数列表 {#section_nyj_7lr_v3j .section}

|名称|类型|是否必需|描述|
|--|--|----|--|
|RegionId|String|是|此实例所在地域，对应的 RegionId 请参见[接入指南](cn.zh-CN/开发指南/管控 API 参考/接入指南.md#)|
|InstanceId|String|是|实例 ID，可使用 [GetInstanceList](cn.zh-CN/开发指南/管控 API 参考/实例管理接口/GetInstanceList.md#) 获取|
|ConsumerId|String|是|Consumer Group ID，可使用 [GetConsumerList](cn.zh-CN/开发指南/管控 API 参考/Consumer Group 管理接口/GetConsumerList.md#) 获取|

## 返回参数列表 {#section_w4x_dqi_r20 .section}

|名称|类型|描述|
|--|--|--|
|RequestId|String|请求的唯一标识 ID|
|Code|Integer|返回码，返回 “200” 为成功|
|Message|String|描述信息|
|ConsumerProgress|Struct|ConsumerProgress 的数据结构|

|名称|类型|描述|
|--|--|--|
|TotalDiff|Long|此 Consumer Group 未消费的消息总量，即堆积量|
|LastTimestamp|Long|此 Consumer Group 最后被消费的消息的产生时间|
|TopicList|Array|此 Consumer Group 对应的每个 Topic 的消费进度列表|

|名称|类型|描述|
|--|--|--|
|Topic|String|Topic 名称|
|TotalDiff|Integer|该 Topic 的未消费消息总量，即堆积量|
|LastTimestamp|Long|该 Topic 最后被消费的消息的产生时间|
|OffsetList|Array|该 Topic 的消费位点信息|

|名称|类型|描述|
|--|--|--|
|Partition|Integer|分区的 ID|
|BrokerOffset|Long|最大位点|
|ConsumerOffset|Long|消费位点|
|LastTimestamp|Long|该分区最后被消费的消息的产生时间|

## 使用示例 {#section_hx5_gwy_abw .section}

该示例为在“华北2（北京）”地域，查询某个实例下的某个 Consumer Group 的消费进度。

``` {#codeblock_6lb_y16_oeh}
public static void main(String[] args) {
          //构建 Client
        IAcsClient iAcsClient = buildAcsClient();

        //构造获取 Consumer Group 消费进度的 request
        GetConsumerProgressRequest request = new GetConsumerProgressRequest();
        request.setAcceptFormat(FormatType.JSON);

        //必要参数，此实例所在地域，必须使用 GetInstanceList 返回值的实例所在的地域
        request.setRegionId("cn-beijing");
        //必要参数，实例 ID
        request.setInstanceId("alikafka_pre-xxxxxx");
        //必要参数，Consumer Group ID
        request.setConsumerId("CID_xxxxxx");

        //获取返回值
        try {
            GetConsumerProgressResponse response = iAcsClient.getAcsResponse(request);
            if (200  == response.getCode()) {
                GetConsumerProgressResponse.ConsumerProgress consumerProgress = response.getConsumerProgress();
                long totalDiff = consumerProgress.getTotalDiff();
                System.out.println(totalDiff);
                for (GetConsumerProgressResponse.ConsumerProgress.TopicListItem item : consumerProgress.getTopicList()) {
                    System.out.println(item.getTopic());;
                    item.getTotalDiff();
                    List<OffsetListItem> offsetListItems = item.getOffsetList();
                    for (OffsetListItem offsetListItem : offsetListItems) {
                        long brokerOffset = offsetListItem.getBrokerOffset();
                        long consumerOffset = offsetListItem.getConsumerOffset();
                        System.out.println("brokerOffset="+brokerOffset);
                        System.out.println("consumerOffset="+consumerOffset);
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

