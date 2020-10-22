# Grant permissions to RAM users

Resource Access Management \(RAM\) allows you to separately manage the permissions of your Alibaba Cloud account and its RAM users. You can grant different permissions to different RAM users to avoid security risks caused by exposure of the AccessKey pair of your Alibaba Cloud account.

Enterprise A has activated Message Queue for Apache Kafka and wants to grant different permissions to its employees with different duties to perform operations on Message Queue for Apache Kafka resources, such as instances, topics, and consumer groups. Therefore, employees with different duties require different permissions. Enterprise A has the following requirements:

-   For security reasons, the enterprise does not want to disclose the AccessKey pair of its Alibaba Cloud account to employees. Instead, it prefers to create different RAM users for the employees and grant different permissions to these users.
-   A RAM user can only use resources under authorization. Resource usage and costs are not separately calculated for that RAM user. All expenses are billed to the Alibaba Cloud account of the enterprise.
-   The enterprise can revoke the permissions granted to RAM users and delete RAM users at any time.

## Step 1. Create a RAM user

Use the Alibaba Cloud account of the enterprise to log on to the RAM console and create a RAM user.

1.  Log on to the [RAM console](http://ram.console.aliyun.com).

2.  In the left-side navigation pane, choose **Identities** \> **Users**.

3.  On the **Users** page, click **Create User**.

4.  On the **Create User** page, set **Logon Name** and **Display Name** in the **User Account Information** section.

    **Note:**

    -   The logon name can be up to 128 characters in length and can contain letters, digits, periods \(.\), underscores \(\_\), and hyphens \(-\).
    -   The display name can be up to 24 characters in length.
5.  To create multiple RAM users, click **Add User** and repeat the previous step.

6.  In the **Access Mode** section, select an access mode and click **OK**.

    **Note:** For security purposes, we recommend that you select only one access mode.

    -   If you select **Console Password Logon**, you must complete further settings, including the console password setting, whether to reset the password upon the next logon, and whether to enable multi-factor authentication.
    -   If you select **Programmatic Access**, RAM automatically creates an AccessKey pair for the RAM user.

        **Note:** For security reasons, the RAM console allows you to view or download the AccessKey secret only once. Therefore, when you create an AccessKey pair, you must keep your AccessKey secret strictly confidential.

7.  In the **Verify by Phone Number** dialog box, click **Get Verification Code**, enter the verification code sent to your mobile phone, and then click **OK**.


## Step 2: Grant permissions to the RAM user

Grant different permissions to RAM users.

1.  In the left-side navigation pane of the RAM console, choose **Identities** \> **Users**.

2.  On the **Users** page, find the user to which you want to grant permissions, and click **Add Permissions** in the **Actions** column.

3.  In the **Select Policy** section of the **Add Permissions** dialog box, click System Policy or Custom Policy, enter the keyword of the policy that you want to add in the search box, click the displayed policy, and then click **OK**.

    **Note:** For more information about the policies that can be granted to access Message Queue for Apache Kafka, see [RAM policies](/intl.en-US/Access control/RAM policies.md).

4.  In the **Add Permissions** dialog box, view the authorization information and click **Complete**.


RAM users of employees of the enterprise can access Message Queue for Apache Kafka in the following ways.

-   Console
    1.  Open the [RAM User Logon](https://signin.aliyun.com/login.htm) page in your browser.
    2.  On the **RAM User Logon** page, enter the name of the RAM user, click **Next**, enter the password, and then click **Login**.

        **Note:** The logon name of the RAM user is in the format of <$username\>@<$AccountAlias\> or <$username\>@<$AccountAlias\>.onaliyun.com. <$AccountAlias\> is the account alias. If no account alias is set, the ID of the Alibaba Cloud account is used.

-   API

    In the code, use the AccessKey ID and AccessKey secret of the RAM user to call an API to access Message Queue for Apache Kafka.


