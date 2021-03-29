# Grant permissions across Alibaba Cloud accounts

You can use a RAM role to grant permissions across Alibaba Cloud accounts so that an enterprise can access the Message Queue for Apache Kafka instance of another enterprise.

Enterprise A has activated Message Queue for Apache Kafka. Enterprise A requires Enterprise B to manage Message Queue for Apache Kafka resources, such as instances, topics, and consumer groups. Enterprise A has the following requirements:

-   Enterprise A can focus on its business systems and only act as the owner of Message Queue for Apache Kafka. Enterprise A can authorize Enterprise B to maintain, monitor, and manage Message Queue for Apache Kafka.
-   If an employee joins or leaves Enterprise B, no permission change is required. Enterprise B can grant its RAM users fine-grained permissions on cloud resources of Enterprise A. The RAM user credentials can be assigned to either employees or applications.
-   If the agreement between Enterprise A and Enterprise B ends, Enterprise A can revoke the permissions from Enterprise B.

## Step 1: Enterprise A creates a RAM role

Use the Alibaba Cloud account of Enterprise A to log on to the RAM console, and create a RAM role for the Alibaba Cloud account of Enterprise B.

1.  Log on to the [RAM console](https://ram.console.aliyun.com/?spm=a2c4g.11186623.2.12.50234772TAoqT9).

2.  In the left-side navigation pane, click **RAM Roles**.

3.  On the **RAM Roles** page, click **Create RAM Role**.

4.  In the **Create RAM Role** panel, select **Alibaba Cloud Account** and click **Next**.

5.  In the **RAM Role Name** field, enter a RAM role name. Set the **Select Trusted Alibaba Cloud Account** parameter to **Other Alibaba Cloud Account** and enter the ID of the Alibaba Cloud account of Enterprise B. Then, click **OK**.

    **Note:**

    -   The RAM role name can be up to 64 characters in length and can contain letters, digits, and hyphens \(-\).
    -   You can view the account ID on the **Security Settings** page of the **Account Management** console.

## Step 2: Enterprise A grants permissions to the RAM role

Grant the RAM role the permissions to access the Message Queue for Apache Kafka resources of Enterprise A. The permissions are to be granted to Enterprise B.

1.  In the left-side navigation pane of the RAM console, click **RAM Roles**.

2.  On the **RAM Roles** page, find the RAM role, and click **Add Permissions** in the **Actions** column.

3.  In the **Select Policy** section of the **Add Permissions** panel, click System Policy or Custom Policy. Enter the keyword of the policy that you want to attach to the RAM role in the search box, click the displayed policy, and then click **OK**.

    **Note:** For more information about the policies that authorize RAM roles and RAM users to access Message Queue for Apache Kafka, see [RAM policies](/intl.en-US/Access control/RAM policies.md).

4.  In the **Add Permissions** panel, check the authorization information and click **Complete**.


## Step 3: Enterprise B creates a RAM user

Use the Alibaba Cloud account of Enterprise B to log on to the RAM console and create a RAM user.

1.  Log on to the [RAM console](http://ram.console.aliyun.com).

2.  In the left-side navigation pane, choose **Identities** \> **Users**.

3.  On the **Users** page, click **Create User**.

4.  In the **User Account Information** section, enter a logon name in the **Logon Name** field and a display name in the **Display Name** field.

    **Note:**

    -   The logon name can be up to 128 characters in length and can contain letters, digits, periods \(.\), underscores \(\_\), and hyphens \(-\).
    -   The display name can be up to 24 characters in length.
5.  To create multiple RAM users, click **Add User** and repeat the previous step.

6.  In the **Access Mode** section, select an access mode and click **OK**.

    **Note:** For security reasons, we recommend that you select only one access mode.

    -   If you select **Console Access**, you must complete further settings, including the console password setting, whether to reset the password upon the next logon, and whether to enable multi-factor authentication.
    -   If you select **Programmatic Access**, RAM automatically creates an AccessKey pair for the RAM user.

        **Note:** For security reasons, the RAM console allows you to view or download the AccessKey secret only once. Therefore, when you create an AccessKey pair, you must keep your AccessKey secret strictly confidential.

7.  In the **Verify by Phone Number** dialog box, click **Get Verification Code**, enter the verification code sent to your mobile phone, and then click **OK**.


## Step 4: Enterprise B grants permissions to the RAM user

Attach the AliyunSTSAssumeRoleAccess permission policy to the RAM user.

1.  In the left-side pane, choose **Identities** \> **Users**.

2.  On the **Users** page, find the RAM user and click **Add Permissions** in the **Actions** column.

3.  In **Select Policy** section of the **Add Permissions** panel, click **System Policy**. Enter AliyunSTSAssumeRoleAccess in the search box, click the displayed policy to add it to the Selected list, and then click **OK**.

4.  In the **Add Permissions** panel, check the authorization information and click **Complete**.


The RAM user of Enterprise B can access the Message Queue for Apache Kafka resources of Enterprise A in the following ways:

-   Use the Alibaba Cloud Management console
    1.  Open the [RAM Account Login page](https://signin.aliyun.com/login.htm) in your browser.
    2.  On the **RAM Account Login** page, enter the name of the RAM user, click **Next**, enter the password, and then click **Login**.

        **Note:** The logon name of the RAM user is in the format of <$username\>@<$AccountAlias\> or <$username\>@<$AccountAlias\>.onaliyun.com. <$AccountAlias\> is the alias of the RAM user. If no alias is set, use the ID of the Alibaba Cloud account.

    3.  On the RAM user center page, move the pointer over the profile in the upper-right corner and click **Switch Identity**.
    4.  On the **Switch Role** page, enter the enterprise alias or default domain name of Enterprise A, and the RAM role name, and then click **Submit**.

        **Note:**

        -   To view the enterprise alias, use the Alibaba Cloud account of Enterprise A to log on to the Alibaba Cloud user center. Move the pointer over the profile picture in the upper-right corner and view the value on the floating layer.
        -   To view the default domain name, use the Alibaba Cloud account of Enterprise A to log on to the RAM console. On the **Settings** page, click the **Advanced** tab to view the default domain name.
-   Call API operations
    1.  Call the AssumeRole operation to obtain the AccessKey ID, AccessKey secret, and Security Token Service \(STS\) token. For more information, see [AssumeRole](/intl.en-US/API Reference/API Reference (STS)/Operation interfaces/AssumeRole.md).
    2.  Use the obtained AccessKey ID, AccessKey secret, and STS token to call a specific API operation to access the corresponding Message Queue for Apache Kafka resources.

