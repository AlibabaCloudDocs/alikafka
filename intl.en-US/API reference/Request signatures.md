# Request signatures

You must sign all HTTP or HTTPS API requests to ensure security. Alibaba Cloud uses the request signature to verify the identity of the API caller. Message Queue for Apache Kafka implements symmetric encryption with an AccessKey pair to verify the identity of the request sender.

An AccessKey pair is an identity credential issued to Alibaba Cloud accounts and RAM users that is similar to a logon username and password for the Message Queue for Apache Kafka console. An AccessKey pair consists of an AccessKey ID and an AccessKey secret. The AccessKey ID is used to verify the identity of the user, while the AccessKey secret is used to encrypt and verify the signature string. You must keep your AccessKey secret strictly confidential.

**Note:** Message Queue for Apache Kafka provides SDKs in multiple programming languages to automatically calculate the signature string. For more information, see [Download SDKs](https://github.com/AliwareMQ/aliware-kafka-demos?spm=a2c4g.11186623.2.13.2a105cfbnSbhjT).

## Step 1: Create a canonicalized query string

1.  Arrange the request parameters \(including all [Common parameters](/intl.en-US/API reference/Common parameters.md) and operation-specific parameters except `Signature`\) in alphabetical order.
2.  Encode the request parameters and their values in UTF-8 according to RFC 3986. The encoding rules are as follows:
    -   Uppercase letters, lowercase letters, digits, and some special characters such as hyphens \(-\), underscores \(\_\), periods \(.\), and tildes \(~\) do not need to be encoded.
    -   Other characters must be percent encoded in `%XY` format. `XY` represents the ASCII code of the characters in hexadecimal notation. For example, double quotation marks \("\) are encoded as `%22`.
    -   Extended UTF-8 characters are encoded in `%XY%ZA...` format.
    -   Spaces must be encoded as `%20`. Do not encode spaces as plus signs \(+\).

        This encoding method is slightly different from the multipurpose Internet mail extensions \(MIME\) encoding method that encodes data into the format of `application/x-www-form-urlencoded`.

        If you choose `java.net.URLEncoder` in the Java standard library, first encode the request parameters and their values by using `percentEncode` in the standard library, and then replace the plus sign \(+\) with `%20`, the asterisk \(\*\) with `%2A`, and `%7E` with a tilde \(~\). In this way, you can obtain an encoded string that matches the preceding encoding rules. The following shows the sample code:

        ```
        private static final String ENCODING = "UTF-8";
        private static String percentEncode(String value) throws UnsupportedEncodingException {
        return value ! = null ? URLEncoder.encode(value, ENCODING).replace("+", "%20").replace("*", "%2A").replace("%7E", "~") : null;
        }                                
        ```

3.  Use an equal sign \(=\) to connect each encoded request parameter and its value.
4.  Use an ampersand \(&\) to connect the encoded request parameters. Note that these parameters must be arranged in the same order as that in Step 1.

Now, you have obtained a canonicalized query string \(`CanonicalizedQueryString`\) whose structure complies with [Request syntax](t1852929.md#section_8ig_hfy_nky).

## Step 2: Create a string-to-sign

1.  Create a string-to-sign by specifying `StringToSign`. You can also use `percentEncode` to encode the canonicalized query string constructed in the previous step. The rules are as follows:

    ```
    StringToSign=
    HTTPMethod + "&" + //HTTPMethod: HTTP method used to make the request, such as POST.
    percentEncode("/") + "&" + //percentEncode("/"): Encode backslashes (/) in UTF-8 as %2F.
    percentEncode(CanonicalizedQueryString) //Encode the canonicalized query string.                        
    ```

2.  Calculate the HMAC-SHA1 value of `StringToSign` according to [RFC 2104](https://www.ietf.org/rfc/rfc2104.txt). In this example, the Java Base64 encoding method is used.

    ```
    Signature = Base64( HMAC-SHA1( AccessSecret, UTF-8-Encoding-Of(StringToSign) ) )                        
    ```

    **Note:** According to RFC 2104, the key used for the calculation is obtained by appending an ampersand \(&\) to your AccessKey secret. The ASCII value of the ampersand \(&\) is 38.

3.  Add the `Signature` parameter, which is encoded according to the rule specified in [RFC 3986](https://tools.ietf.org/html/rfc3986), to the canonicalized query string URL.

## Example 1: Concatenate parameters

For example, call the `GetInstanceList` operation to query instances. Assume that you have obtained `AccessKeyID=testid` and `AccessKeySecret=testsecret`. The signature process is as follows:

1.  Create a canonicalized query string.

    ```
    http://alikafka.%s.aliyuncs.com/?Timestamp=2016-02-23T12:46:24Z&Format=XML&AccessKeyId=testid&Action=GetInstanceList&SignatureMethod=HMAC-SHA1&SignatureNonce=3ee8c1b8-83d3-44af-a94f-4e0ad82fd6cf&Version=2014-05-26&SignatureVersion=1.0                    
    ```

2.  Create a string-to-sign by specifying `StringToSign`.

    ```
    POST&%2F&AccessKeyId%3Dtestid&Action%3DGetInstanceList&Format%3DXML&SignatureMethod%3DHMAC-SHA1&SignatureNonce%3D3ee8c1b8-83d3-44af-a94f-4e0ad82fd6cf&SignatureVersion%3D1.0&Timestamp%3D2016-02-23T12%253A46%253A24Z&Version%3D2014-05-26                    
    ```

3.  Calculate the signature string. According to `AccessKeySecret=testsecret`, the key used for the calculation is `testsecret&`. Therefore, the calculated signature string is `OLeaidS1JvxuMvnyHOwuJ+uX5qY=`. In this example, the Java Base64 encoding method is used.

    ```
    Signature = Base64( HMAC-SHA1( AccessSecret, UTF-8-Encoding-Of(StringToSign) ) )                    
    ```

4.  Add `Signature=OLeaidS1JvxuMvnyHOwuJ%2BuX5qY%3D`, which is encoded according to RFC 3986, to the URL in step 1.

    ```
    http://alikafka.%s.aliyuncs.com/?SignatureVersion=1.0&Action=GetInstanceList&Format=JSON&SignatureNonce=3ee8c1b8-83d3-44af-a94f-4e0ad82fd6cf&Version=2014-05-26&AccessKeyId=testid&Signature=OLeaidS1JvxuMvnyHOwuJ%2BuX5qY%3D&SignatureMethod=HMAC-SHA1&Timestamp=2016-02-23T12%253A46%253A24Z                    
    ```


In the preceding URL, you can use tools such as a browser, curl, or wget to initiate an HTTP request to call the `GetInstanceList` operation to view instances in a specified region of Alibaba Cloud.

## Example 2: Use the programming language

For example, call the `GetInstanceList` operation to obtain instances. Assume that you have obtained `AccessKeyID=testid` and `AccessKeySecret=testsecret`, and all request parameters are placed in the `Java Map<String, String>` object.

1.  Predefine an encoding method.

    ```
    private static final String ENCODING = "UTF-8";
    private static String percentEncode(String value) throws UnsupportedEncodingException {
    return value ! = null ? URLEncoder.encode(value, ENCODING).replace("+", "%20").replace("*", "%2A").replace("%7E", "~") : null;
    }                    
    ```

2.  Predefine the encoding time format by specifying the `Timestamp` parameter. The value of the `Timestamp` parameter must comply with the [ISO 8601](/intl.en-US/API Reference/Appendix/ISO 8601 Time Format.md) standard. The time must be in UTC+0.

    ```
    private static final String ISO8601_DATE_FORMAT = "yyyy-MM-dd'T'HH:mm:ss'Z'";
    private static String formatIso8601Date(Date date) {
    SimpleDateFormat df = new SimpleDateFormat(ISO8601_DATE_FORMAT);
    df.setTimeZone(new SimpleTimeZone(0, "GMT"));
    return df.format(date);
    }                    
    ```

3.  Create a query string.

    ```
    final String HTTP_METHOD = "POST";
    Map parameters = new HashMap();
    // Specify request parameters. parameters.put("Action", "GetInstanceList");
    parameters.put("Version", "2014-05-26");
    parameters.put("AccessKeyId", "testid");
    parameters.put("Timestamp", formatIso8601Date(new Date()));
    parameters.put("SignatureMethod", "HMAC-SHA1");
    parameters.put("SignatureVersion", "1.0");
    parameters.put("SignatureNonce", UUID.randomUUID().toString());
    parameters.put("Format", "JSON");
    // Arrange the request parameters. String[] sortedKeys = parameters.keySet().toArray(new String[]{});
    Arrays.sort(sortedKeys);
    final String SEPARATOR = "&";
    // Create a string-to-sign by specifying stringToSign. StringBuilder stringToSign = new StringBuilder();
    stringToSign.append(HTTP_METHOD).append(SEPARATOR);
    stringToSign.append(percentEncode("/")).append(SEPARATOR);
    StringBuilder canonicalizedQueryString = new StringBuilder();
    for(String key : sortedKeys) {
    // Encode the key and value. canonicalizedQueryString.append("&")
    .append(percentEncode(key)).append("=")
    .append(percentEncode(parameters.get(key)));
    }
    // Encode the canonicalized query string. stringToSign.append(percentEncode(
    canonicalizedQueryString.toString().substring(1)));                    
    ```

4.  Calculate the signature string. According to `AccessKeySecret=testsecret`, the key used for the HMAC calculation is `testsecret&`. Therefore, the calculated signature string is `OLeaidS1JvxuMvnyHOwuJ%2BuX5qY%3D`.

    ```
    // The following sample code shows how to calculate the signature string. final String ALGORITHM = "HmacSHA1";
    final String ENCODING = "UTF-8";
    key = "testsecret&";
    Mac mac = Mac.getInstance(ALGORITHM);
    mac.init(new SecretKeySpec(key.getBytes(ENCODING), ALGORITHM));
    byte[] signData = mac.doFinal(stringToSign.getBytes(ENCODING));
    String signature = new String(Base64.encodeBase64(signData));                    
    ```


