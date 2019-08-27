# GetInstanceList {#concept_94533_zh .concept}

使用 GetInstanceList 接口来获取您在某地域（Region）下所购买的实例的信息。

## 请求参数列表 {#section_5ug_frz_k1m .section}

|名称|类型|是否必需|描述|
|--|--|----|--|
|RegionId|String|是|实例所属的地域|

|地域名称|RegionId|
|----|--------|
|华东1（杭州）|cn-hangzhou|
|华东2（上海）|cn-shanghai|
|华北1（青岛）|cn-qingdao|
|华北2（北京）|cn-beijing|
|华北3（张家口）|cn-zhangjiakou|
|华北5（呼和浩特）|cn-huhehaote|
|华南1（深圳）|cn-shenzhen|
|中国（香港）|cn-hongkong|
|新加坡|ap-southeast-1|

## 返回参数列表 {#section_zid_kwt_5sh .section}

|名称|类型|描述|
|--|--|--|
|RequestId|String|请求的唯一标识 ID|
|Code|Integer|返回码，返回 “200” 代表成功|
|Message|String|描述信息|
|InstanceList|Array|实例列表信息|

|名称|类型|描述|
|--|--|--|
|InstanceId|String|实例 ID|
|RegionId|String|部署的实例所在地域的 RegionId|
|ServiceStatus|Integer|服务状态|
|VpcId|String|VPC 的 ID|
|VSwitchId|String|VSwitch 的 ID|
|EndPoint|String|接入点信息|
|CreateTime|Long|创建时间|
|ExpiredTime|Long|过期时间|

## 使用示例 {#section_kty_uv1_w1c .section}

该示例为查询您在“华北2（北京）”地域下的实例列表。

``` {#codeblock_ni2_d4l_ikw}
public static void main(String[] args) {
        //构建 Client
        IAcsClient iAcsClient = buildAcsClient();

        //构造获取实例信息 request
        GetInstanceListRequest request = new GetInstanceListRequest();

        //必要参数，注意此参数是指定获取哪个地域下的实例信息
        request.setRegionId("cn-beijing");
        request.setAcceptFormat(FormatType.JSON);

        //获取返回值
        try {
            GetInstanceListResponse response = iAcsClient.getAcsResponse(request);
            if (200  == response.getCode()) {
                List<InstanceListItem> instanceListItems = response.getInstanceList();
                if (CollectionUtils.isNotEmpty(instanceListItems)) {
                    for (InstanceListItem instance : instanceListItems) {
                        String instanceId = instance.getInstanceId();
                        System.out.println(instanceId);
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

