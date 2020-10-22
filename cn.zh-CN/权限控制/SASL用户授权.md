---
keyword: [kafka, acl, sasl]
---

# SASL用户授权

借助消息队列Kafka版的ACL，您可以按需为SASL用户赋予向消息队列Kafka版收发消息的权限，从而实现权限分割。

您的消息队列Kafka版实例必须满足以下条件：

-   规格类型为专业版。
-   版本为2.2.0及以上。
-   运行状态为服务中。

企业A购买了消息队列Kafka版，企业A希望用户A只能从消息队列Kafka版的所有Topic中消费消息，而不能向消息队列Kafka版的任何Topic生产消息。

## 步骤一：申请开启ACL

**说明：** 公网/VPC实例的默认SASL用户是没有任何权限的。开启ACL后，公网/VPC实例的默认SASL用户会因为没有任何权限而收发消息失败。您需要为该SASL用户授予所有Topic和Consumer Group的读写权限。

提交[工单](https://selfservice.console.aliyun.com/ticket/category/alikafka)联系消息队列Kafka版技术人员开启ACL。

## 步骤二：升级小版本

申请通过后，将实例的小版本升级至最新版。

1.  登录[消息队列Kafka版控制台](http://kafka.console.aliyun.com)。

2.  在顶部菜单栏，选择地域。

3.  在左侧导航栏，单击**实例详情**。

4.  在**实例详情**页面，选择实例，在**基本信息**区域的**小版本**右侧，单击**升级小版本**。

5.  在**升级小版本**对话框，完成以下操作。

    1.  在**姓名**文本框，输入您的姓名。

    2.  在**紧急联系电话**文本框，输入您的紧急联系电话。

    3.  单击**升级**。

    实例的**运行状态**显示**升级中**。

6.  在**运行状态**区域的**运行状态**，单击**进度详情**。

    **进度详情**对话框显示任务的**剩余时间**和**当前进度**。


## 步骤三：开启ACL

升级实例的小版本后，在消息队列Kafka版控制台为实例开启ACL。

1.  在消息队列Kafka版控制台的**实例详情**页面，选择申请开启的实例，单击**实例详情**页签。

2.  在**基本信息**区域，单击**开启ACL**。

    ![pg_enable_acl](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/7247559951/p99533.png)

3.  在对话框，单击**确定**。

    单击**确定**后：

    -   实例的**基本信息**区域显示**SASL接入点**。

        不同类型接入点的区别，请参见[接入点对比](/cn.zh-CN/产品简介/接入点对比.md)。

    -   实例的运行状态显示**升级中**。

## 步骤四：创建SASL用户

实例升级完成后，为用户A创建SASL用户。

1.  在消息队列Kafka版控制台的**实例详情**页面，选择开启了ACL的实例，单击**SASL用户**页签。

2.  在**SASL用户**页签下方，单击**创建SASL用户**。

3.  在**创建SASL用户**对话框，设置SASL用户，然后单击**创建**。

    ![pg_create_sasl_user ](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/7247559951/p99571.png)

    |参数|描述|
    |--|--|
    |用户名|SASL用户的名称。|
    |密码|SASL用户的密码。|
    |用户类型|消息队列Kafka版支持的SASL机制如下：     -   PLAIN： 一种简单的用户名密码校验机制。消息队列Kafka版优化了PLAIN机制，支持不重启实例的情况下动态增加SASL用户。
    -   SCRAM：一种用户名密码校验机制，安全性比PLAIN更高。消息队列Kafka版使用SCRAM-SHA-256。 |

    创建完成后，**SASL用户**页签下方显示您创建的SASL用户。


## 步骤五：授予SASL用户权限

为用户A创建SASL用户后，为该SASL用户授予从Topic和Consumer Group读取消息的权限。

1.  在消息队列Kafka版控制台的**实例详情**页面，选择开启了ACL的实例，单击**SASL权限**页签。

2.  在**SASL权限**页签下方，单击**创建ACL**。

3.  在**创建ACL**对话框，设置ACL，然后单击**创建**。

    ![pg_read_from_Topic](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/7247559951/p99574.png)

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

    ![pg_read_from_Topic](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/7247559951/p99587.png)


完成授权后，用户A可以通过SASL接入点接入消息队列Kafka版并使用PLAIN机制消费消息。详情请参见[SDK概述](/cn.zh-CN/SDK参考/SDK概述.md)。

