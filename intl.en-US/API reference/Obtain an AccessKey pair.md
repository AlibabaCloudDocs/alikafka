# Obtain an AccessKey pair

This topic describes how to create an AccessKey pair for an Alibaba Cloud account or Resource Access Management \(RAM\) user. When you call the Alibaba Cloud APIs, you must use the AccessKey pair to verify your identity.

An AccessKey pair consists of an AccessKey ID and an AccessKey secret.

-   The AccessKey ID is used to verify the identity of the user.
-   The AccessKey secret is used to encrypt and verify the signature string. You must keep your AccessKey secret strictly confidential.

**Warning:** If the AccessKey pair of your Alibaba Cloud account is disclosed, the security of your resources are threatened. We recommend that you use a RAM user to call API operations. This minimizes the possibility of disclosing the AccessKey pair of your Alibaba Cloud account.

1.  Log on to the [Alibaba Cloud Management Console](https://home-intl.console.aliyun.com/) by using your Alibaba Cloud account.

2.  Move the pointer over your profile picture in the upper-right corner and click **AccessKey Management**.

3.  In the **Security Tips** dialog box, click Use Current AccessKey Pair or Use AccessKey Pair of RAM User.

    ![pg_warning](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/6415559951/p48002.png)

4.  Obtain the AccessKey pair of an account.

    -   Obtain the AccessKey pair of the Alibaba Cloud account
        1.  Click **Use Current AccessKey Pair**.
        2.  On the AccessKey Management page, click **Create AccessKey**.
        3.  In the Phone Verification dialog box, enter the verification code in the Verification Code field and click **OK**.
        4.  In the Create AccessKey dialog box, view the AccessKey ID and AccessKey secret.**** You can click **Save AccessKey Information** to download the AccessKey pair.

            ![pg_ak_success](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/6415559951/p48003.png)

    -   Obtain the AccessKey pair of a RAM user
        1.  Click **Use AccessKey Pair of RAM User**.
        2.  Go to the [RAM console](https://ram.console.aliyun.com/users/new) and create a RAM user on the Create User page. Skip this step if you want to obtain the AccessKey pair of an existing RAM user.
        3.  In the [RAM console](https://ram.console.aliyun.com/users/new), choose **Identities** \> **Users** in the left-side navigation pane.
        4.  Find the target RAM user and click the user logon name. On the Authentication tab, click **Create AccessKey** in the User AccessKeys section.
        5.  In the Create AccessKey dialog box, view the AccessKey ID and AccessKey secret.

            ![pg_ak_success](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/6415559951/p48004.png)

        6.  Optional. In the Create AccessKey dialog box, click **Download CSV File** to download the AccessKey pair, or click **Copy** to copy the AccessKey pair.

