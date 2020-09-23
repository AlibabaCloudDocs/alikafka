# Obtain an AccessKey pair

This topic describes how to create an AccessKey pair for an Alibaba Cloud account or Resource Access Management \(RAM\) user. Alibaba Cloud uses the AccessKey pair to verify the identity of the API caller.

An AccessKey pair consists of an AccessKey ID and an AccessKey secret.

-   The AccessKey ID is used to verify the identity of the user.
-   The AccessKey secret is used to encrypt and verify the signature string. You must keep your AccessKey secret strictly confidential.

**Warning:** If the AccessKey pair of your Alibaba Cloud account is disclosed, the security of your resources will be threatened. We recommend that you use the Accesskey pair of RAM users to call operations. This reduces the risk of disclosing the AccessKey pair of your Alibaba Cloud account.

1.  Log on to the [Alibaba Cloud console](https://home.console.aliyun.com/new?spm=a2c4g.11186623.2.13.b22b5f81PaDcNA#/) by using your Alibaba Cloud account.

2.  Move the pointer over the account icon in the upper-right corner of the page, and then click **AccessKey**.

3.  In the **Security Tips** dialog box, click Continue to manage AccessKey or Get Started with Sub Users' AccessKey as required.

    ![pg_warning](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/6415559951/p48002.png)

4.  Obtain the AccessKey pair of an account.

    -   Obtain the AccessKey pair of an Alibaba Cloud account
        1.  Click **Continue to manage AccessKey**.
        2.  On the Security Management page, click **Create AccessKey**.
        3.  On the Phone Verification page, click Send verification code to obtain the verification code, enter the verification code in the Verification code field, and click **Confirm**.
        4.  In the Create User AccessKey dialog box, show **AccessKey Details** to view the AccessKey ID and AccessKey secret. You can click **Save AccessKey Information** to download the AccessKey pair.

            ![pg_ak_success](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/6415559951/p48003.png)

    -   Obtain the AccessKey pair of a RAM user
        1.  Click **Get Started with Sub Users' AccessKey**.
        2.  If no RAM user is available, click [Create User](https://ram.console.aliyun.com/users/new) in the RAM console, to create a RAM user. If you need to obtain the AccessKey pair of an existing RAM user, skip this step.
        3.  In the left-side navigation pane of the [RAM console](https://ram.console.aliyun.com/users/new), choose **Identities** \> **Users**.
        4.  Find the target user and click the logon name. On the Basic Information page, click the Authentication tab. In the User AccessKeys section, click **Create AccessKey**.
        5.  On the Phone Verification page, click Send verification code to obtain the verification code, enter the verification code in the Verification code field, and click **Confirm**.
        6.  In the Create AccessKey dialog box, view the AccessKey ID and AccessKey secret. You can click **Download CSV File** to download the AccessKey pair, or click **Copy** to copy the AccessKey pair.

            ![pg_ak_success](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/6415559951/p48004.png)


