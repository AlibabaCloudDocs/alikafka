# Grant permissions to RAM users

By using Resource Access Management \(RAM\), you can grant different permissions to different RAM users to avoid security risks caused by exposure of your Alibaba Cloud account's AccessKey pair.

## Scenarios

An enterprise has activated Message Queue for Apache Kafka and wants to grant different permissions to its employees with different duties to operate Message Queue for Apache Kafka resources. This enterprise has the following requirements:

-   For security reasons, the enterprise does not want to disclose the AccessKey pair of its Alibaba Cloud account to employees. Instead, it prefers to create different RAM users for the employees and grant different permissions to these users.
-   A RAM user can only use resources under authorization. Resource usage and costs are not calculated separately for that RAM user. All expenses are billed to the Alibaba Cloud account of the enterprise.
-   The enterprise can revoke the permissions granted to RAM users and delete RAM users at any time.

## Instructions

Before authorizing RAM users, note the following:

You can only grant permissions to RAM users in the console or by using the corresponding API operations of Message Queue for Apache Kafka. No matter RAM users are authorized or not, the RAM users can use the Message Queue for Apache Kafka SDK to send and subscribe to messages.

You can use the SDK to send and subscribe to messages in the same way as in open-source clients. To manage the IP addresses that can use the SDK to send and subscribe to messages, log on to the Message Queue for Apache Kafka console, and set an IP address whitelist on the **Instance Details** page.

## Step 1: Create a RAM user

Use your Alibaba Cloud account to log on to the RAM console and create a RAM user.

1.  Log on to the [RAM console](http://ram.console.aliyun.com/).
2.  In the left-side navigation pane, choose **Identities** \> **Users**.
3.  On the Users page, click **Create User**.
4.  On the Create User page, set **Logon Name** and **Display Name** in the **User Account Information** section.
5.  If you want to create multiple RAM users at a time, click **Add User**, and repeat the previous step.
6.  In the **Access Mode** section, select **Console Password Logon** or **Programmatic Access**, and then click **OK**.

    **Note:** For security purposes, select only one access mode.

    -   If you select **Console Password Logon**, perform further settings. For example, you can select Automatically Generate Default Password or Custom Logon Password for Console Password, Required at Next Logon or Not Required for Password Rest, and Required to Enable MFA or Not Required for Multi-factor Authentication.
    -   If you select **Programmatic Access**, RAM automatically generates an AccessKey pair for the RAM user. Then, the RAM user can access your Message Queue for Apache Kafka by calling the corresponding API operations.
    **Note:** For security reasons, the RAM console allows you to view or download the AccessKey secret only once. Therefore, when creating an AccessKey pair, record the AccessKey secret safely.

7.  In the Verify by Phone Number dialog box, click **Get Verification Code**, enter the verification code sent to your mobile phone, and then click **OK**.

## Step 2: Grant permissions to the RAM user

Before using a RAM user, you must grant permissions to the RAM user.

1.  Log on to the [RAM console](http://ram.console.aliyun.com).
2.  In the left-side navigation pane, choose **Identities** \> **Users**.
3.  On the Users page, find the target user, and click **Add Permissions** in the **Actions** column.
4.  On the Add Permissions page, select a permission policy in the **Select Policy** drop-down list. Enter the permission policy you want to add to the text box, click the permission policy displayed, and then click **OK**.
    -   System policy

        Currently, Message Queue for Apache Kafka supports two coarse-grained system policies.

        |Policy|Description|
        |------|-----------|
        |AliyunKafkaFullAccess|The permission to manage Message Queue for Apache Kafka. It is equivalent to the permission that the Alibaba Cloud account has. A RAM user to which this permission is granted can send and subscribe to all messages and use all the features of the console.|
        |AliyunKafkaReadOnlyAccess|The read-only permission of Message Queue for Apache Kafka. A RAM user to which this permission is granted can only read all resources of the Alibaba Cloud account.|

        **Note:** We recommend that you grant AliyunKafkaFullAccess to O&M personnel to create and delete resources. We recommend that you grant AliyunKafkaReadOnlyAccess to developers, so that developers can view resources but cannot delete or create resources. If you want to control the permissions of developers in a more fine-grained manner, you can use the following custom policy.

    -   Custom policy

        If you need more fine-grained authorization, you can create a custom policy for access control.

        For more information about how to create a custom policy, see [Create a custom policy](/intl.en-US/Policy Management/Custom policies/Create a custom policy.md).

        To help you customize RAM policies, the following table lists mapping of custom policies for Message Queue for Apache Kafka.

        |Action|Description|Read-only or not|
        |------|-----------|----------------|
        |ReadOnly|Reads all resources only. It is a compound permission.|Yes|
        |ListInstance|Views instances.|Yes|
        |StartInstance|Deploys instances.|No|
        |UpdateInstance|Changes instance configuration.|No|
        |ReleaseInstance|Releases instances.|No|
        |ListTopic|Views topics.|Yes|
        |CreateTopic|Creates topics.|No|
        |UpdateTopic|Changes topic configuration.|No|
        |DeleteTopic|Deletes topics.|No|
        |ListGroup|Views consumer groups.|Yes|
        |CreateGroup|Creates consumer groups.|No|
        |UpdateGroup|Changes consumer group configuration.|No|
        |DeleteGroup|Deletes consumer groups.|No|
        |QueryMessage|Queries messages.|Yes|
        |SendMessage|Sends messages.|No|
        |DownloadMessage|Downloads messages.|Yes|

        Example 1: Grant a RAM user the read-only permission on the `alikafka_post-cn-xxx` instance

        ```
        {
            "Version": "1",
            "Statement": [
                {
                    "Action": [
                      "alikafka:ReadOnly"
                               ],
                    "Resource": "acs:alikafka:*:*:alikafka_post-cn-xxx",
                    "Effect": "Allow"
                }
            ]
        }
        ```

        Example 2: Grant a RAM user permissions to view instances, topics, and consumer groups and query and download messages on the `alikafka_post-cn-xxx` instance

        ```
        {
            "Version": "1",
            "Statement": [
                {
                    "Action": [
                      "alikafka:ListInstance",
                      "alikafka:ListTopic",
                      "alikafka:ListGroup",
                      "alikafka:QueryMessage",
                      "alikafka:DownloadMessage"
                               ],
                    "Resource": "acs:alikafka:*:*:alikafka_post-cn-xxx",
                    "Effect": "Allow"
                }
            ]
        }
        ```

        Example 3: Grant a RAM user all permissions on the `alikafka_post-cn-xxx` instance

        ```
        {
            "Version": "1",
            "Statement": [
                {
                    "Action": [
                      "alikafka:*Instance",
                      "alikafka:*Topic",
                      "alikafka:*Group",
                      "alikafka:*Message"
                               ],
                    "Resource": "acs:alikafka:*:*:alikafka_post-cn-xxx",
                    "Effect": "Allow"
                }
            ]
        }
        ```

5.  On the **Add Permissions** page, view the authorization information summary in the Authorization Result section, and then click **Finished**.

## What to do next

After creating a RAM user with an Alibaba Cloud account, you can distribute the logon name and password of the RAM user or AccessKey pair information to other employees. Other employees can log on to the console or call an API operation with the RAM user through the following steps.

-   Log on to the console
    1.  Open the [RAM User Logon](https://signin.aliyun.com/login.htm) page.
    2.  On the RAM User Logon page, enter the logon name of the RAM user, click **Next**, enter the password, and then click **Log on**.

        **Note:** The logon name of the RAM user is in the format of `<$username>@<$AccountAlias>` or `<$username>@<$AccountAlias>.onaliyun.com`. `<$AccountAlias>` is the account alias. If no account alias is set, the ID of the Alibaba Cloud account is used.

    3.  On the Users page, click a product with the permission to access the console.
-   Call an API operation

    Call an API operation with the RAM user's AccessKey pair.

    Use the AccessKey ID and AccessKey secret of the RAM user in the code.


## References

-   [What is RAM?](/intl.en-US/Product Introduction/What is RAM?.md)
-   [Terms](/intl.en-US/Product Introduction/Terms.md)
-   [Create a RAM user](/intl.en-US/RAM User Management/Create a RAM user.md)
-   [Grant permissions to a RAM user](/intl.en-US/RAM User Management/Grant permissions to a RAM user.md)
-   [Log on to the console as a RAM user](/intl.en-US/RAM User Management/Log on to the console as a RAM user.md)

