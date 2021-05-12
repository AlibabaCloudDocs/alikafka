# Obtain an AccessKey pair

This topic describes how to create an AccessKey pair for your Alibaba Cloud account or Resource Access Management \(RAM\) user.When you call the Alibaba Cloud APIs, you must use the AccessKey pair to verify your identity.

An AccessKey pair consists of an AccessKey ID and an AccessKey secret.

-   The AccessKey ID is used to verify the identity of the user.
-   The AccessKey secret is used to encrypt and verify the signature string.You must keep your AccessKey secret confidential.

**Warning:** If the AccessKey pair of your Alibaba Cloud account is disclosed, the security of your resources is compromised.We recommend that you use a RAM user to call API operations. This minimizes the risk in which the AccessKey pair of your Alibaba Cloud account is leaked.

1.  Log on to the Alibaba Cloud Management Console by using your Alibaba Cloud account.

2.  Move the pointer over the profile picture in the upper-right corner of the page and click AccessKey Management.

3.  In the Note dialog box, click Use Current AccessKey Pair or User AccessKey Pair of RAM User.

    ![pg_warning](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6415559951/p48002.png)

4.  Obtain the AccessKey pair of an account.

    -   Obtain the AccessKey pair of your Alibaba Cloud account.
        1.  Click Use Current AccessKey Pair.
        2.  On the AccessKey Management page, click Create AccessKey.
        3.  In the Phone Verification dialog box, enter the verification code in the Verification Code field and click OK.
        4.  In the Create User AccessKey dialog box, expand AccessKey Details to view the AccessKey ID and AccessKey secret.You can click Save AccessKey Information to download the AccessKey pair.

            ![pg_ak_success](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6415559951/p48003.png)

    -   Obtain the AccessKey pair of a RAM user.
        1.  Click Use AccessKey Pair of RAM User.
        2.  You are directed to the Users page in the RAM console. On the Users page, click Create User. On the Create User page, create a RAM user.If you want to obtain the AccessKey pair of an existing RAM user, skip this step.
        3.  Go to the RAM console. In the left-side navigation pane, choose IdentitiesUsers.
        4.  Find the RAM user whose AccessKey pair you want to obtain, and click the user logon name. The Authentication tab on the RAM user details page appears. In the User AccessKeys section, click Create AccessKey.
        5.  In the Create AccessKey dialog box, view the AccessKey ID and AccessKey secret.You can click Download CSV File or Copy to download or copy the AccessKey pair.

            ![pg_ak_success](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6415559951/p48004.png)


