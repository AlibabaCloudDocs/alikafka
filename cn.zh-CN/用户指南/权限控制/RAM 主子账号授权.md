# RAM 主子账号授权 {#concept_68338_zh .concept}

借助访问控制 RAM 的 RAM 用户（子账号），您可以实现权限分割的目的，按需为 RAM 用户赋予不同权限，并避免因暴露阿里云账号（主账号）密钥造成的安全风险。

## 使用场景 {#section_iss_ijs_04q .section}

某企业开通了消息队列 for Apache Kafka 服务，该企业需要员工操作消息队列 for Apache Kafka 服务所涉及的资源由于每个员工的工作职责不一样，需要的权限也不一样。该企业的需求：

-   出于安全或信任的考虑，不希望将云账号密钥直接透露给员工，而希望能给员工创建相应的用户账号。

-   用户账号只能在授权的前提下操作资源，不需要对用户账号进行独立的计量计费，所有开销都计入企业账号名下。

-   随时可以撤销用户账号的权限，也可以随时删除其创建的用户账号。


## 使用说明 {#section_c8o_kwz_4ki .section}

在使用 RAM 授权时，需要注意以下几点：

-   RAM 主子账号授权只针对控制台和 Open API 的操作，利用 SDK 收发消息跟 RAM 是否授权无关。

    SDK 收发消息与开源客户端行为一致，VPC 内访问无需鉴权，公网访问利用 SASL 进行身份验证。如果想控制 SDK 收发的范围，可以在消息队列 for Apache Kafka 控制台的实例详情页设置 IP 白名单。

-   本文中提及的资源是指实例、Topic 和 Consumer Group；涉及的操作主要是指创建、删除和其它操作，其它操作包括查看 Topic 状态、重置位点等除创建和删除以外的操作。


## 步骤一：创建 RAM 用户 {#section_q2b_82z_ih1 .section}

首先需要使用阿里云账号（主账号）登录 RAM 控制台并创建 RAM 用户。

1.  登录 [RAM 控制台](http://ram.console.aliyun.com/)，在左侧导航栏中选择**人员管理** \> **用户** ，并在用户页面上单击**新建用户**。
2.  在新建用户页面的**用户账号信息**区域中，输入**登录名称**和**显示名称**。

    **说明：** 登录名称中允许使用小写英文字母、数字、“.”、“\_”和“-”，长度不超过 128 个字符。显示名称不可超过 24 个字符或汉字。

3.  （可选）如需一次创建多个用户，则单击**添加用户**，并重复上一步。
4.  在**访问方式**区域中，勾选**控制台密码登录**或**编程访问**，并单击**确定**。

    **说明：** 为提高安全性，请仅勾选一种访问方式。

    -   如果勾选**控制台密码登录**，则完成进一步设置，包括自动生成默认密码或自定义登录密码、登录时是否要求重置密码，以及是否开启 MFA 多因素认证。
    -   如果勾选**编程访问**，则 RAM 会自动为 RAM 用户创建 AccessKey（API 访问密钥）。
    **说明：** 出于安全考虑，RAM 控制台只提供一次查看或下载 AccessKeySecret 的机会，即创建 AccessKey 时，因此请务必将 AccessKeySecret 记录到安全的地方。

5.  在手机验证对话框中单击**获取验证码**，并输入收到的手机验证码，然后单击**确定**。创建的 RAM 用户显示在用户页面上。

## 步骤二：为 RAM 用户添加权限 {#section_ls9_k2v_o3j .section}

在使用 RAM 用户之前，需要为其添加相应权限。

1.  在 [RAM 控制台](http://ram.console.aliyun.com)左侧导航栏中选择**人员管理** \> **用户** 。

2.  在用户页面上找到需要授权的用户，单击**操作**列中的**添加权限**。

3.  在添加权限面板的**选择权限**区域中，通过关键字搜索需要添加的权限策略 ，并单击权限策略将其添加至右侧的**已选择**列表中，然后单击**确定**。

    -   **系统权限**

        消息队列 for Apache Kafka 目前仅支持三种系统权限策略：

        |权限策略名称|说明|
        |------|--|
        |AliyunKafkaFullAccess|管理消息队列 for Apache Kafka 的权限，等同于主账号的权限，被授予该权限的 RAM 用户具有所有消息收发权限且有控制台所有功能操作权限。|
        |AliyunKafkaPubOnlyAccess|消息队列 for Apache Kafka 的发布权限，被授予该权限的 RAM 用户具有使用主账号所有资源通过 SDK 发送消息的权限。|
        |AliyunKafkaSubOnlyAccess|消息队列 for Apache Kafka 的订阅权限，被授予该权限的 RAM 用户具有使用主账号所有资源通过 SDK 订阅消息的权限。|

        **说明：** 建议给运维人员授予 **AliyunKafkaFullAccess** 权限策略，由运维人员去创建和删除资源。给开发人员授予 **AliyunKafkaPubOnlyAccess** 和 **AliyunKafkaSubOnlyAccess** 权限策略，可以查看这些资源，但是不能删除和创建。 如果想控制开发人员只能查看某个具体的实例下的资源，可使用以下自定义策略。

    -   **自定义策略**

        假设 RAM 用户只能查看实例 ID 为 **XXX** 下的 Topic 和 Consumer Group，可做如下设置：

        ``` {#codeblock_gb9_fv4_op4}
         ```
         {
             "Version": "1",
             "Statement": [
                 {
                     "Action": [
                         "alikafka:PUB",
                         "alikafka:SUB"
                     ],
                     "Resource": [
                         "acs:alikafka:*:*:Kafka_XXX_*",
                         "acs:alikafka:*:*:Local_Kafka_XXX_*",
                         "acs:alikafka:*:*:%RETRY%Kafka_XXX_*"
                     ],
                     "Effect": "Allow"
                 }
             ]
         }
        ```

        自定义权限策略中所涉及的参数说明如下：

        -   Action：把 **alikafka:PUB** 和 **alikafka:PUB** 都配上，两者的区分已无实际意义。
        -   Resource 的配置说明如下：

            -   配置 **acs:alikafka:\*:\*:Kafka\_XXX\_\*** 代表可以访问 **XXX** 实例的所有云存储 Topic；
            -   配置 **acs:alikafka:\*:\*:Local\_Kafka\_XXX\_\*** 代表可以访问 **XXX** 实例的所有 Local 存储 Topic；
            -   配置 **acs:alikafka:\*:\*:%RETRY%Kafka\_XXX\_\*** 代表可以访问 **XXX**实例的所有 Consumer Group。存储类型的说明请参见 [Topic 存储最佳实践](../../../../cn.zh-CN/最佳实践/Topic 存储最佳实践.md#)
4.  在**添加权限**的授权结果页面上，查看授权信息摘要，并单击**完成**。

## 后续步骤 {#section_b0m_vd7_ytu .section}

使用阿里云账号（主账号）创建好 RAM 用户后，即可将 RAM 用户的登录名称及密码或者 AccessKey 信息分发给其他用户。其他用户可以按照以下步骤使用 RAM 用户登录控制台或调用 API。

-   登录控制台

    1.  打开 [RAM 用户登录](https://signin.aliyun.com/login.htm)页面。

    2.  在 RAM 用户登录页面上，输入 RAM 用户登录名称，单击**下一步**，并输入 RAM 用户密码，然后单击**登录**。

        **说明：** RAM 用户登录名称的格式为 `<$username>@<$AccountAlias>` 或 `<$username>@<$AccountAlias>.onaliyun.com`。 `<$AccountAlias>` 为账号别名，如果没有设置账号别名，则默认值为阿里云账号（主账号）的 ID。

    3.  在子用户用户中心页面上单击有权限的产品，即可访问控制台。


使用 RAM 用户的 AccessKey 调用 API

在代码中使用 RAM 用户的 AccessKeyId 和 AccessKeySecret 即可。

## 更多信息 {#section_zi7_myt_rjt .section}

-   [什么是访问控制](../../../../cn.zh-CN/产品简介/什么是访问控制.md#)
-   [基本概念](../../../../cn.zh-CN/产品简介/基本概念.md#)
-   [创建RAM用户](../../../../cn.zh-CN/用户管理/创建RAM用户.md#)
-   [为RAM用户授权](../../../../cn.zh-CN/用户管理/为RAM用户授权.md#)
-   [RAM用户登录控制台](../../../../cn.zh-CN/用户管理/RAM用户登录控制台.md#)

