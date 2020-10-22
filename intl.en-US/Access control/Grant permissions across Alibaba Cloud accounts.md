# Grant permissions across Alibaba Cloud accounts

You can use a Resource Access Management \(RAM\) role to grant permissions across Alibaba Cloud accounts so that an enterprise can access the Message Queue for Apache Kafka instance of another enterprise.

Enterprise A has activated Message Queue for Apache Kafka. Enterprise A requires Enterprise B to manage Message Queue for Apache Kafka resources, such as instances, topics, and consumer groups. Enterprise A has the following requirements:

-   Enterprise A wants to focus on its business systems and only act as the owner of Message Queue for Apache Kafka. Enterprise A can authorize Enterprise B to maintain, monitor, and manage Message Queue for Apache Kafka.
-   If an employee joins or leaves Enterprise B, no permission change is required. Enterprise B can grant its RAM users fine-grained permissions on cloud resources of Enterprise A. The RAM user credentials can be assigned to either employees or applications.
-   If the agreement between Enterprise A and Enterprise B ends, Enterprise A can revoke the permissions from Enterprise B.

## Step 1: Enterprise A creates a RAM role

Use the Alibaba Cloud account of Enterprise A to log on to the RAM console, and create a RAM role for the Alibaba Cloud account of Enterprise B.

1.  Log on to the [RAM console](https://ram.console.aliyun.com/?spm=a2c4g.11186623.2.12.50234772TAoqT9).

2.  In the left-side navigation pane, click **RAM Roles**.

3.  On the **RAM Roles** page, click **Create RAM Role**.

4.  In the **Create RAM Role** dialog box, select **Alibaba Cloud Account** and click **Next**.

5.  In the **RAM Role Name** field, enter a RAM role name. In the **Select Trusted Alibaba Cloud Account** section, select **Other Alibaba Cloud Account**, and enter the ID of the Alibaba Cloud account of Enterprise B. Then, click **OK**.

    **Note:**

    -   The RAM role name can be up to 64 characters in length and can contain letters, digits, and hyphens \(-\).
    -   You can view the account ID on the **Security Settings** page of the **Account Management** console.

## Step 2: Enterprise A grants permissions to the RAM role

Assign the RAM role the permission to access Message Queue for Apache Kafka of Enterprise A. The permission is granted to Enterprise B.

1.  In the left-side navigation pane of the RAM console, click **RAM Roles**.

2.  On the **RAM Roles** page, find the RAM role, and click **Add Permissions** in the **Actions** column.

3.  In the **Select Policy** section of the **Add Permissions** dialog box, click System Policy or Custom Policy, enter the keyword of the policy that you want to add in the search box, click the displayed policy, and then click **OK**.

    **Note:** For more information about the policies that can be granted to access Message Queue for Apache Kafka, see [RAM policies](/intl.en-US/Access control/RAM policies.md).

4.  In the **Add Permissions** dialog box, view the authorization information and click **Complete**.


## Step 3: Enterprise B creates a RAM user

Use the Alibaba Cloud account of Enterprise B to log on to the RAM console and create a RAM user.

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


## Step 4: Enterprise B grants permissions to the RAM user

Assign the AliyunSTSAssumeRoleAccess permission to the RAM user.

1.  In the left-side pane, choose **Identities** \> **Users**.

2.  On the **Users** page, find the RAM user and click **Add Permissions** in the **Actions** column.

3.  In **Select Policy** section of the **Add Permissions** dialog box, click **System Policy**, enter AliyunSTSAssumeRoleAccess in the search box, click the policy to add it to the Selected list, and then click **OK**.

4.  In the **Add Permissions** dialog box, view the authorization information and click **Complete**.


The RAM user of Enterprise B can access Message Queue for Apache Kafka of Enterprise A in the following ways:

-   Console
    1.  Open the [RAM User Logon page](https://signin.aliyun.com/login.htm) in your browser.
    2.  On the **RAM User Logon** page, enter the name of the RAM user, click **Next**, enter the password, and then click **Login**.

        **Note:** The logon name of the RAM user is in the format of <$username\>@<$AccountAlias\> or <$username\>@<$AccountAlias\>.onaliyun.com. <$AccountAlias\> is the account alias. If no account alias is set, the ID of the Alibaba Cloud account is used.

    3.  On the RAM user center page, move the pointer to the profile in the upper-right corner and click **Switch Role**.
    4.  On the **Alibaba Cloud - Switch Role** page, set the enterprise alias or default domain name of Enterprise A, and the RAM role name, and click **Switch**.

        **Note:**

        -   Enterprise alias: Use the Alibaba Cloud account of Enterprise A to log on to the Alibaba Cloud user center, move the pointer over the profile picture in the upper-right corner, and view the value on the floating layer.
        -   Default domain name: Use the Alibaba Cloud account of Enterprise A to log on to the RAM console. On the **Settings** page, click the **Advanced** tab to view the default domain name.
-   API
    1.  Call the AssumeRole operation to obtain the AccessKey ID, AccessKey secret, and SecurityToken. SecurityToken is the temporary security token. For more information, see [AssumeRole](/intl.en-US/API Reference (STS)/Operation interfaces/AssumeRole.md).
    2.  Use the obtained AccessKey ID, AccessKey secret, and SecurityToken to call an API operation in the code to access Message Queue for Apache Kafka.

