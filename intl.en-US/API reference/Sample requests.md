# Sample requests

This topic provides an example of how to construct parameters and a signature by using Java and send a POST request. The construction procedure in Java is similar to those in other languages.

## Examples

```
  private static final String ISO8601_DATE_FORMAT = "yyyy-MM-dd'T'HH:mm:ss'Z'";
    private static final String ENCODING = "UTF-8";
    private static final String HTTP_METHOD = "POST";
    // The ID of the region where the purchased instance is located.
    private static final String REGION_ID = "cn-xxxxxx";
    private static final String ACCESS_KEY = "xxxxxx";
    private static final String ACCESS_KEY_SECRET = "xxxxxx";


    public static void main(String[] args) {
        try {
            // 1. Set parameters.
            Map<String, String> parameters = buildCommonParams();
            // 2. Arrange the request parameters in alphabetical order.
            String[] sortedKeys = sortParamsToArray(parameters);
            // 3. Create a string-to-sign.
            String stringToSign = buildSignString(parameters, sortedKeys);
            // 4. Calculate the signature string.
            String signature = calculateSignature(stringToSign);
            // 5. Add the signature string to the request as a parameter.
            parameters.put("Signature", signature);
            // 6. Send a POST request.
            String result = post(String.format("http://alikafka.%s.aliyuncs.com/", REGION_ID), parameters);
            // 7. Verify the results.
            System.out.println(result);
        } catch (Throwable throwable) {
            throwable.printStackTrace();
        }
    }

    private static Map<String, String> buildCommonParams() {
        Map<String, String> parameters = Maps.newHashMap();
        // Action: specifies the request method.
        // GetInstanceList: queries the instance information. GetConsumerList: queries consumer groups. GetTopicList: queries topics. GetTopicStatus: queries the topic status.
        // GetConsumerProgress: queries the consumption status. CreateTopic: creates a topic. CreateConsumerGroup: creates a consumer group.
        parameters.put("Action", "GetInstanceList");
        // API version: "2018-10-15"
        parameters.put("Version", "2018-10-15");
        // Request parameter: RegionId is required.
        // Ensure that all the parameters of the API are specified, such as InstanceId, Topic, and ConsumerId.
        // parameters.put("InstanceId", "cn-huhehaote");
        // parameters.put("Topic", "cn-huhehaote");
        // parameters.put("Remark", "cn-huhehaote");
        // parameters.put("ConsumerId", "cn-huhehaote");
        parameters.put("RegionId", REGION_ID);
        parameters.put("AccessKeyId", ACCESS_KEY);
        // Timestamp, in the format of yyyy-MM-dd'T'HH:mm:ss'Z'.
        parameters.put("Timestamp", formatIso8601Date(new Date()));
        parameters.put("SignatureMethod", "HMAC-SHA1");
        parameters.put("SignatureVersion", "1.0");
        parameters.put("SignatureNonce", UUID.randomUUID().toString());
        parameters.put("Format", "json");
        return parameters;
    }

    private static String[] sortParamsToArray(Map<String, String> parameters) {
        String[] sortedKeys = parameters.keySet().toArray(new String[]{});
        Arrays.sort(sortedKeys);
        return sortedKeys;
    }

    private static String buildSignString(Map<String, String> parameters,
                                          String[] sortedKeys) throws UnsupportedEncodingException {
        StringBuilder stringToSign = new StringBuilder();
        String SEPARATOR = "&";
        stringToSign.append(HTTP_METHOD).append(SEPARATOR);
        stringToSign.append(percentEncode("/")).append(SEPARATOR);
        StringBuilder canonicalizedQueryString = new StringBuilder();
        for(String key : sortedKeys) {
            // Encode the key and the value.
            canonicalizedQueryString.append("&")
                .append(percentEncode(key)).append("=")
                .append(percentEncode(parameters.get(key)));
        }

        // Encode the canonicalized query string.
        stringToSign.append(percentEncode(canonicalizedQueryString.toString().substring(1)));
        return stringToSign.toString();
    }

    private static String calculateSignature(String stringToSign)
        throws NoSuchAlgorithmException, InvalidKeyException, UnsupportedEncodingException {
        String ALGORITHM = "HmacSHA1";
        String ENCODING = "UTF-8";
        // The AccessKey secret of the account.
        String accessKeySecret = ACCESS_KEY_SECRET + "&";
        Mac mac = Mac.getInstance(ALGORITHM);
        mac.init(new SecretKeySpec(accessKeySecret.getBytes(ENCODING), ALGORITHM));
        byte[] signData = mac.doFinal(stringToSign.getBytes(ENCODING));
        return new String(Base64.getEncoder().encode(signData));
    }

    private static String percentEncode(String value) throws UnsupportedEncodingException {
        return value ! = null ? URLEncoder.encode(value, ENCODING).replace("+", "%20").replace("*", "%2A").replace("%7E", "~") : null;
    }

    private static String formatIso8601Date(Date date) {
        SimpleDateFormat df = new SimpleDateFormat(ISO8601_DATE_FORMAT);
        df.setTimeZone(new SimpleTimeZone(0, "GMT"));
        return df.format(date);
    }

    private static String post(String url, Map<String, String> paramMap) throws IOException {
        Form form = Form.form();
        for (String key : paramMap.keySet()) {
            form.add(key, paramMap.get(key));
        }

        return Request.Post(url).bodyForm(form.build()).connectTimeout(10000).execute().returnContent().asString();
    }
                
```

