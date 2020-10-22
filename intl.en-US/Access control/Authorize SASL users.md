# Authorize SASL users

The access control list \(ACL\) feature of Message Queue for Apache Kafka allows you to grant Simple Authentication and Security Layer \(SASL\) users different permissions to access Message Queue for Apache Kafka resources.

Your Message Queue for Apache Kafka instance must meet the following conditions:

-   The instance type is of the Professional Edition.
-   The open-source Apache Kafka version is 2.2.0 or later.
-   The instance is in the Running state.

**Note:** The default SASL user of an instance of the Internet and VPC type does not have any permissions. After the ACL feature is enabled, the default SASL user of an instance of the Internet and VPC type fails to send and subscribe to messages due to lack of permissions. You need to grant the default SASL user permissions to read and write all topics and consumer groups of the instance.

The ACL feature is provided by Alibaba Cloud Message Queue for Apache Kafka to manage Message Queue for Apache Kafka SASL users and Message Queue for Apache Kafka resource access permissions. For more information, see [t1884059.md\#section\_qi3\_i3h\_fsm](/intl.en-US/Access control/Overview.md).

## Scenarios

An enterprise has purchased a Message Queue for Apache Kafka instance. The enterprise wants employee A to only consume messages from all topics of the instance but not produce messages in any topic of the instance. This topic describes how to use the ACL feature of Message Queue for Apache Kafka to grant different permissions to different users.

## Step 1: Apply to enable the ACL feature

Submit a [ticket](https://workorder-intl.console.aliyun.com/#/ticket/add/?productId=1352), and contact Message Queue for Apache Kafka Customer Services to enable the ACL feature.

## Step 2: Enable the ACL feature

After the application is approved, enable the ACL feature for an instance in the Message Queue for Apache Kafka console.

1.  Log on to the [Message Queue for Apache Kafka console](https://kafka.console.aliyun.com/).

2.  In the left-side navigation pane, click **Instance Details**.

3.  On the **Instance Details** page, click the target instance, and click the **Instance Details** tab.

4.  In the **Basic Information** section, click **Enable ACL**.

5.  In the dialog box that appears, click **OK**.

    After you click **OK**, the following information appears:

    -   **SASL Endpoints** appears in the **Basic Information** section of the instance.

        ![pg_9094_endpoint_enabled](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/0670567951/p99537.png)

        For more information about the differences among the three types of endpoints, see [Endpoint comparison](/intl.en-US/Introduction/Endpoint comparison.md).

    -   The instance is in the **Upgrading** state.

## Step 3: Create an SASL user

After the instance is upgraded, create an SASL user for employee A.

1.  On the **Instance Details****Instance Details** page of the Message Queue for Apache Kafka console, click the instance with ACL enabled, and click the **SASL Users** tab.

2.  On the **SASL Users** tab, click **Create SASL User**.

3.  On the **Create SASL User** page, perform the following operations:

    1.  In the **Username** field, enter User\_A.

    2.  In the **Password** field, enter Paasword\_A.

    3.  Select **PLAIN**.

    4.  Click **Create**.

    ![pg_create_sasl_user](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/0670567951/p102887.png)

    |Parameter|Description|Example|
    |---------|-----------|-------|
    |Username|The name of the SASL user.|User\_A|
    |Password|The password of the SASL user.|Paasword\_A|
    |User type|Message Queue for Apache Kafka supports the following SASL mechanisms:     -   PLAIN: a simple username and password verification mechanism. Message Queue for Apache Kafka improves the PLAIN mechanism and allows you to dynamically add SASL users without restarting the instance.
    -   SCRAM: a username and password verification mechanism, providing higher security than PLAIN. Message Queue for Apache Kafka uses SCRAM-SHA-256.
|PLAIN|

    The SASL user that you created appears on the **SASL User** tab.

    ![pg_create_sasl_user_result](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/0670567951/p102886.png)


## Step 4: Grant the SASL user permissions

After creating the SASL user for employee A, grant the SASL user permissions to read messages from topics and consumer groups.

1.  On the **Instance Details** page of the Message Queue for Apache Kafka console, click the instance with ACL enabled, and click the **SASL Permissions** tab.

2.  On the **SASL Permissions** tab, click **Create ACL**.

3.  On the **Create ACL** page, perform the following operations:

    1.  Select User\_A from the **Username** drop-down list.

    2.  Select **Topic** for Resource Type.

    3.  Select **LITERAL** for Matching Mode.

    4.  Select **Read** for Permission.

    5.  From the **Resource Name** drop-down list, select Wildcard \(\*\).

    6.  Click **Create**.

    ![pg_create_acl](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/0670567951/p102885.png)

4.  On the **SASL Permissions** tab, click **Create ACL**.

5.  On the **Create ACL** page, perform the following operations:

    1.  Select **User\_A** from the Username drop-down list.

    2.  Select **Group** for Resource Type.

    3.  Select **LITERAL** for Matching Mode.

    4.  Select **Read** for Permission.

    5.  From the **Resource Name** drop-down list, select Wildcard \(\*\).

    6.  Click **Create**.

    ![pg_read_from_Topic](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/1670567951/p99587.png)

    |Parameter|Description|
    |---------|-----------|
    |Username|The name of the SASL user. Message Queue for Apache Kafka supports the wildcard \(\*\), representing all user names.|
    |Resource Type|Message Queue for Apache Kafka grants permissions for the following types of resources:     -   Topic: message topic
    -   Group: consumer group |
    |Matching Mode|Message Queue for Apache Kafka supports the following matching modes:     -   LITERAL: matches resources by literal value. In this mode, only resources with the same name are matched.
    -   PREFIXED: matches resources by prefix. In this mode, any resource name that starts with the specified prefix is matched. |
    |Permission|Message Queue for Apache Kafka supports the following types of operations:     -   Write: writes data.
    -   Read: reads data.
**Note:** Group only supports read data permission. |
    |Resource Name|The name of the topic or consumer group. Message Queue for Apache Kafka supports the wildcard \(\*\), representing all resource names.|


## Step 5: Query the SASL user permissions

After authorization is completed, query the SASL user permissions.

1.  On the **SASL Permissions** tab, perform the following operations:

    1.  Select **User\_A** from the Username drop-down list.

    2.  Select **LITERAL** for Matching Mode.

    3.  Select **Topic** for Resource Type.

    4.  From the **Resource Name** drop-down list, select Wildcard \(\*\).

    5.  Click **Search**.

        ![pg_query_sasl_topic](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/1670567951/p102884.png)

2.  On the **SASL Permissions** tab, perform the following operations:

    1.  Select **Group** for Resource Type.

    2.  From the **Resource Name** drop-down list, select Wildcard \(\*\).

    3.  Click **Search**.

        ![pg_query_group](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/1670567951/p99579.png)


For more information about how to consume messages throughMessage Queue for Apache Kafka by using the SASL user, see [Subscribe to messages](/intl.en-US/Quick-start/Step 4: Use the SDK to send and subscribe to messages/Use the default endpoint to send and subscribe to messages.md).

