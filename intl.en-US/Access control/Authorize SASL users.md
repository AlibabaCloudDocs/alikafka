---
keyword: [kafka, acl, sasl]
---

# Authorize SASL users

The access control list \(ACL\) feature of Message Queue for Apache Kafka allows you to authorize Authentication and Security Layer \(SASL\) to send and subscribe to messages in Message Queue for Apache Kafka.

Your Message Queue for Apache Kafka instance must meet the following conditions:

-   The edition of the instance is Professional Edition.
-   The instance is in the Running state.
-   The major version of the instance is 2.2.0 or later. For more information about how to upgrade the major version, see [t998840.md\#section\_51f\_vbx\_nxm](/intl.en-US/User guide/Instances/Upgrade the instance version.md).
-   The minor version of the instance is the latest version. For more information about how to upgrade the minor version, see [t998840.md\#section\_3te\_xlw\_iip](/intl.en-US/User guide/Instances/Upgrade the instance version.md).

**Note:** The default SASL users of a public network/VPC instance have no permissions to perform operations. After the ACL feature is enabled, the default SASL users of a public network/VPC instance fail to send or subscribe to messages because these users have no required permissions. You must grant the default SASL users the read and write permissions to all topics and consumer groups of the instance.

Enterprise A has purchased a Message Queue for Apache Kafka instance. The enterprise wants User A to consume only messages from all topics of the Message Queue for Apache Kafka instance, but not to send messages to the topics of the Message Queue for Apache Kafka instance.

## Step 1: Enable the ACL feature

After you upgrade the minor version of an instance, enable ACL for the instance in the Message Queue for Apache Kafka console.

1.  Log on to the [Message Queue for Apache KafkaConsole](https://kafka.console.aliyun.com/?spm=a2c4g.11186623.2.22.6bf72638IfKzDm).

2.  In the top navigation bar, select a region.

3.  In the left-side navigation pane, click **Instance Details page**.

4.  On the **Instance Details** page, select the instance and then click the **Instance Details** tab. On the right side of the **Basic Information** section, click **Enable ACL**.

5.  In the **Note** dialog box, click **OK**, and then refresh the page.

    After you refresh the page, the SASL endpoint is displayed in the **Basic Information** section of the **Instance Details** page. The status changes to Upgrading.

    **Note:** After the upgrade, the ACL feature is enabled for the instance. Then, you can create an SASL user and grant the user the required permissions. The SASL user can then access the instance by using the SASL endpoint. The upgrade takes 15 to 20 minutes.


## Step 2: Create an SASL user

After you enabled the ACL feature for the instance, create an SASL user for User A.

1.  On the **Instance Details** page of the Message Queue for Apache Kafka console, select the instance and click the **SASL Users** tab.

2.  On the **SASL Users** tab, click **Create SASL User**.

3.  In the **Create SASL User** dialog box, specify the parameters, and then click **OK**.

    ![pg_create_sasl_user ](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/4739533061/p99571.png)

    |Parameter|Description|
    |---------|-----------|
    |Username|The name of the SASL user.|
    |Password|The password of the SASL user.|
    |User Type|Message Queue for Apache Kafka supports the following SASL mechanisms:     -   PLAIN: a simple username and password verification mechanism. Message Queue for Apache Kafka provides an improved PLAIN mechanism that allows you to add SASL users without restarting the instance.
    -   SCRAM: a username and password verification mechanism that provides more security than PLAIN. SCRAM-SHA-256 is used in Message Queue for Apache Kafka. |

    The SASL user that you created is displayed on the **SASL Users** tab.


## Step 3: Grant permissions to the SASL user

After you create the SASL user for User A, grant the SASL user permissions to read messages from topics and consumer groups.

1.  On the **Instance Details** page of the Message Queue for Apache Kafka console, select the instance with ACL enabled, and then click the **SASL Permissions** tab.

2.  On the **SASL Permissions** tab, click **Create ACL**.

3.  In the **Create ACL** dialog box, specify the parameters, and then click **OK**.

    ![pg_read_from_Topic](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/4739533061/p99574.png)

    |Parameter|Description|
    |---------|-----------|
    |Username|The name of the SASL user. Message Queue for Apache Kafka supports asterisks \(\*\). You can use an asterisk to represent all user names.|
    |Resource Type|Message Queue for Apache Kafka allows you to grant permissions for the following resource types:     -   Topic: message topics.
    -   Group: consumer groups. |
    |Matching Mode|Message Queue for Apache Kafka supports the following matching modes:     -   LITERAL: matches resources by literal value. In this mode, only resources with the same name are matched.
    -   PREFIXED: matches resources by prefix. In this mode, a resource name that starts with the specified prefix is matched. |
    |Permission|Message Queue for Apache Kafka supports the following types of operations:    -   Write: writes data.
    -   Read: reads data.
**Note:** If you set the Resource Type parameter to Group, you must set the Permission parameter to Read. |
    |Resource Name|The name of the resource. The value can be the name of a topic or consumer group. Message Queue for Apache Kafka supports asterisks \(\*\). You can use an asterisk to represent all resource names.|

4.  On the **SASL Permissions** tab, click **Create ACL**.

5.  In the **Create ACL** dialog box, specify the parameters, and then click **OK**.

    ![pg_read_from_Topic](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/1670567951/p99587.png)


## What to do next

After you grant the SASL user the required permissions, User A can connect to Message Queue for Apache Kafka by using the SASL endpoint and use the PLAIN mechanism to consume messages. For information about how to use the SDK, see [t998844.md\#](/intl.en-US/SDK reference/Overview.md).

