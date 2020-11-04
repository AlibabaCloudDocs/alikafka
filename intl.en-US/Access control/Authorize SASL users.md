---
keyword: [kafka, acl, sasl]
---

# Authorize SASL users

The access control list \(ACL\) feature of Message Queue for Apache Kafka allows you to grant Simple Authentication and Security Layer \(SASL\) users the permissions to send and subscribe to messages in Message Queue for Apache Kafka.

Your Message Queue for Apache Kafka instance must meet the following conditions:

-   The instance edition is the Professional Edition.
-   The instance version is 2.2.0 or later.
-   The instance is in the Running state.

Enterprise A has purchased a Message Queue for Apache Kafka instance. The enterprise wants User A to only consume messages from all topics of the Message Queue for Apache Kafka instance but not produce messages in any topic of the Message Queue for Apache Kafka instance.

## Step 1: Apply to enable the ACL feature

**Note:** The default SASL user of an instance of the Internet and VPC type does not have any permissions. After the ACL feature is enabled, the default SASL user of an instance of the Internet and VPC type fails to send and subscribe to messages due to lack of permissions. You must grant the default SASL user the read and write permissions for all topics and consumer groups of the instance.

Submita [ticket](https://workorder-intl.console.aliyun.com/#/ticket/add/?productId=1352), and contact Message Queue for Apache Kafka Customer Services to enable the ACL feature.

## Step 2: Upgrade the minor version

After your application is approved, upgrade the minor version of the instance to the latest version.

1.  Log on to the [Message Queue for Apache Kafka console](http://kafka.console.aliyun.com).

2.  In the top navigation bar, select a region.

3.  In the left-side navigation pane, click **Instance Details**.

4.  On the **Instance Details** page, select an instance and then click **Upgrade Minor Version** next to **Internal Version** in the **Basic Information** section.

5.  In the **Upgrade Minor Version** dialog box, complete the following operations.

    1.  Enter your name in the **Name** field.

    2.  Enter your emergency phone number in the **Emergency phone number** field.

    3.  Click **Upgrade**.

    The **Status** of the instance is displayed as **Upgrading**.

6.  In the **Status** section, click **Details** next to **Status**.

    In the **Details** dialog box, **Remaining Time** and **Progress** are displayed.


## Step 3: Enable the ACL feature

After you upgrade the minor version of an instance, enable ACL for the instance in the Message Queue for Apache Kafka console.

1.  On the **Instance Details** page of the Message Queue for Apache Kafka console, select the instance, and click the **Instance Details** tab.

2.  In the **Basic Information** section, click **Enable ACL**.

3.  In the dialog box that appears, click **OK**.

    After you click **OK**, the following information appears:

    -   **SASL Endpoint** appears in the **Basic Information** section of the instance.

        For more information about the differences among endpoints, see [t1884077.md\#](/intl.en-US/Introduction/Endpoint comparison.md).

    -   The instance is in the **Upgrading** state.

## Step 4: Create an SASL user

After the instance is upgraded, create an SASL user for User A.

1.  On the **Instance Details** page of the Message Queue for Apache Kafka console, click the instance with ACL enabled, and click the **SASL Users** tab.

2.  In the lower part of the **SASL Users** tab, click **Create SASL User**.

3.  In the **Create SASL User** dialog box, enter SASL user information, and click **OK**.

    ![pg_create_sasl_user ](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/4739533061/p99571.png)

    |Parameter|Description|
    |---------|-----------|
    |Username|The name of the SASL user.|
    |Password|The password of the SASL user.|
    |User Type|Message Queue for Apache Kafka supports the following SASL mechanisms:     -   PLAIN: a simple username and password verification mechanism. Message Queue for Apache Kafka improves the PLAIN mechanism and allows you to dynamically add SASL users without restarting the instance.
    -   SCRAM: a username and password verification mechanism, providing higher security than PLAIN. Message Queue for Apache Kafka uses SCRAM-SHA-256. |

    The SASL user that you created appears on the **SASL Users** tab.


## Step 5: Grant the SASL user permissions

After you create the SASL user for User A, grant the SASL user permissions to read messages from topics and consumer groups.

1.  On the **Instance Details** page of the Message Queue for Apache Kafka console, click the instance with ACL enabled, and click the **SASL Permissions** tab.

2.  In the lower part of the **SASL Permissions** tab, click **Create ACL**.

3.  In the **Create ACL** dialog box, enter ACL information, and then click **OK**.

    ![pg_read_from_Topic](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/4739533061/p99574.png)

    |Parameter|Description|
    |---------|-----------|
    |Username|The name of the SASL user. Message Queue for Apache Kafka supports the wildcard \(\*\), representing all user names.|
    |Resource Type|Message Queue for Apache Kafka grants permissions for the following types of resources:     -   Topic: message topic
    -   Group: consumer group |
    |Matching Mode|Message Queue for Apache Kafka supports the following matching modes:     -   LITERAL: matches resources by literal value. In this mode, only resources with the same name are matched.
    -   PREFIXED: matches resources by prefix. In this mode, any resource name that starts with the specified prefix is matched. |
    |Permission|Message Queue for Apache Kafka supports the following types of operations:    -   Write: writes data.
    -   Read: reads data.
**Note:** If you set Resource Type to Group, you can set Permission only to Read. |
    |Resource Name|The name of the topic or consumer group. Message Queue for Apache Kafka supports the wildcard \(\*\), representing all resource names.|

4.  In the lower part of the **SASL Permissions** tab, click **Create ACL**.

5.  In the **Create ACL** dialog box, enter ACL information, and then click **OK**.

    ![pg_read_from_Topic](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/1670567951/p99587.png)


After the authorization is completed, User A can connect to Message Queue for Apache Kafka through the SASL endpoint and use the PLAIN mechanism to consume messages. For more information, see [t998844.md\#](/intl.en-US/SDK reference/Overview.md).

