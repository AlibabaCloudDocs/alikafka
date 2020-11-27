---
keyword: [kafka, acl, sasl]
---

# SASL用户授权

借助消息队列Kafka版的ACL，您可以按需为SASL用户赋予向消息队列Kafka版收发消息的权限，从而实现权限分割。

您的消息队列Kafka版实例必须满足以下条件：

-   规格类型为专业版。
-   运行状态为服务中。
-   大版本为2.2.0及以上。如何升级实例大版本，请参见[升级大版本](/intl.zh-CN/用户指南/实例/升级实例版本.md)。
-   小版本为最新版。如何升级实例小版本，请参见[升级小版本](/intl.zh-CN/用户指南/实例/升级实例版本.md)。

**说明：** 公网/VPC实例的默认SASL用户是没有任何权限的。开启ACL后，公网/VPC实例的默认SASL用户会因为没有任何权限而收发消息失败。您需要为该SASL用户授予所有Topic和Consumer Group的读写权限。

企业A购买了消息队列Kafka版，企业A希望用户A只能从消息队列Kafka版的所有Topic中消费消息，而不能向消息队列Kafka版的任何Topic生产消息。

## 步骤一：开启ACL

升级实例的小版本后，在消息队列Kafka版控制台为实例开启ACL。

1.  登录[消息队列Kafka版控制台](https://kafka.console.aliyun.com/?spm=a2c4g.11186623.2.22.6bf72638IfKzDm)。

2.  在顶部菜单栏，选择地域。

3.  在左侧导航栏，单击**实例详情**。

4.  在**实例详情**页面，选择实例，单击**实例详情**页签，在**基本信息**区域右侧，单击**开启ACL**。

5.  在**提示**对话框，单击**确认**，然后手动刷新页面。

    手动刷新页面后，**实例详情**页面的**基本信息**区域出现SASL接入点，运行状态显示升级中。

    **说明：** 升级完成后，实例才会开启ACL。您才可以创建SASL用户并为其授权后，通过SASL接入点接入。升级预计需要15分钟~20分钟。


## 步骤二：创建SASL用户

实例开启ACL后，为用户A创建SASL用户。

1.  在消息队列Kafka版控制台的**实例详情**页面，选择开启了ACL的实例，单击**SASL用户**页签。

2.  在**SASL用户**页签下方，单击**创建SASL用户**。

3.  在**创建SASL用户**对话框，设置SASL用户，然后单击**创建**。

    ![pg_create_sasl_user ](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/7247559951/p99571.png)

    |参数|描述|
    |--|--|
    |用户名|SASL用户的名称。|
    |密码|SASL用户的密码。|
    |用户类型|消息队列Kafka版支持的SASL机制如下：     -   PLAIN： 一种简单的用户名密码校验机制。消息队列Kafka版优化了PLAIN机制，支持不重启实例的情况下动态增加SASL用户。
    -   SCRAM：一种用户名密码校验机制，安全性比PLAIN更高。消息队列Kafka版使用SCRAM-SHA-256。 |

    创建完成后，**SASL用户**页签下方显示您创建的SASL用户。


## 步骤三：授予SASL用户权限

为用户A创建SASL用户后，为该SASL用户授予从Topic和Consumer Group读取消息的权限。

1.  在消息队列Kafka版控制台的**实例详情**页面，选择开启了ACL的实例，单击**SASL权限**页签。

2.  在**SASL权限**页签下方，单击**创建ACL**。

3.  在**创建ACL**对话框，设置ACL，然后单击**创建**。

    ![pg_read_from_Topic](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/7247559951/p99574.png)

    |参数|描述|
    |--|--|
    |用户名|SASL用户的名称。消息队列Kafka版支持通配符星号（\*）表示所有用户名。|
    |资源类型|消息队列Kafka版支持授权的资源类型如下：     -   Topic：消息主题。
    -   Group：消费组。 |
    |匹配模式|消息队列Kafka版支持的匹配模式如下：     -   全匹配：按字面值匹配资源名称。全匹配模式只会匹配名称完全相同的资源。
    -   前缀匹配：按前缀匹配资源名称。前缀匹配模式会匹配以匹配名称开头的任意资源名称。 |
    |操作类型|消息队列Kafka版支持的操作类型如下：    -   Write：写入。
    -   Read：读取。
**说明：** 资源类型Group仅支持操作类型Read。 |
    |资源名|Topic或Consumer Group的名称。消息队列Kafka版支持通配符星号（\*）表示所有资源名。|

4.  在**SASL权限**页签下方，单击**创建ACL**。

5.  在**创建ACL**对话框，设置ACL，然后单击**创建**。

    ![pg_read_from_Topic](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/7247559951/p99587.png)


## 相关操作

完成授权后，用户A可以通过SASL接入点接入消息队列Kafka版并使用PLAIN机制消费消息。如何使用SDK接入，请参见[SDK概述](/intl.zh-CN/SDK参考/SDK概述.md)。

