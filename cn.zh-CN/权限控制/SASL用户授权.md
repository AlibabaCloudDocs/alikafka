---
keyword: [kafka, acl, sasl]
---

# SASL用户授权

借助消息队列Kafka版的ACL，您可以按需为SASL用户赋予不同访问消息队列Kafka版资源的权限，实现权限分割。

您的消息队列Kafka版实例必须满足以下条件：

-   规格类型为专业版。
-   版本为2.2.0及以上。
-   运行状态为服务中。

ACL是阿里云消息队列Kafka版提供的管理消息队列Kafka版的SASL用户和消息队列Kafka版资源访问权限的服务。详情请参见[ACL](/cn.zh-CN/权限控制/权限控制概述.md)。

**说明：** 公网/VPC实例的默认SASL用户是没有任何权限的。开启ACL后，公网/VPC实例的默认SASL用户会因为没有任何权限而收发消息失败。您需要为该SASL用户授予所有Topic和Consumer Group的读写权限。

## 使用场景

某企业购买了消息队列Kafka版实例。该企业希望员工A只能从该实例的所有Topic中消费消息，而不能向该实例的任何Topic生产消息。本教程说明该企业如何使用消息队列Kafka版的ACL功能实现上述权限分割。

## 步骤一：申请开启ACL

提交[工单](https://selfservice.console.aliyun.com/ticket/category/alikafka)联系消息队列Kafka版技术人员开启ACL。

## 步骤二：升级小版本

申请通过后，将实例的小版本升级至最新版。

1.  登录[消息队列Kafka版控制台](http://kafka.console.aliyun.com)。

2.  在顶部菜单栏，选择地域。

3.  在左侧导航栏，单击**实例详情**。

4.  在**实例详情**页面，选择实例，在**基本信息**区域的**小版本**，单击**升级小版本**。

5.  在**升级小版本**对话框，完成以下操作。

    1.  在**姓名**文本框，输入您的姓名。

    2.  在**紧急联系电话：**文本框，输入您的紧急联系电话。

    3.  单击**升级**。

    实例的**运行状态**显示**升级中**。

6.  在**运行状态**区域的**运行状态**，单击**进度详情**。

    **进度详情**提示框显示任务的**剩余时间**和**当前进度**。


## 步骤三：开启ACL

升级实例的小版本后，在消息队列Kafka版控制台为实例开启ACL。

1.  登录[消息队列Kafka版控制台](https://kafka.console.aliyun.com/)。

2.  在左侧导航栏，单击**实例详情**。

3.  在**实例详情**页面，选择申请开启的实例，单击**实例详情**页签。

4.  在**基本信息**区域，单击**开启ACL**。

    ![pg_enable_acl](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/7247559951/p99533.png)

5.  在提示框，单击**确定**。

    单击**确定**后：

    -   实例的**基本信息**区域显示**SASL接入点**。

        ![pg_9094_endpoint_enabled](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/7247559951/p99537.png)

        上图中三种不同类型接入点的区别，请参见[接入点对比](/cn.zh-CN/产品简介/接入点对比.md)。

    -   实例的运行状态显示**升级中**。

        ![pg_upgrade_instance_to_acl](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/7247559951/p99528.png)


## 步骤四：创建SASL用户

实例升级完成后，为员工A创建SASL用户。

1.  在消息队列Kafka版控制台的**实例详情**页面，选择开启了ACL的实例，单击**SASL用户**页签。

2.  在**SASL用户**页签，单击**创建SASL用户**。

3.  在**创建SASL用户**页面，填写SASL用户信息，然后单击**创建**。

    ![pg_create_sasl_user](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/7247559951/p99571.png)

    |参数|描述|示例值|
    |--|--|---|
    |用户名|SASL用户的名称。|User\_A|
    |密码|SASL用户的密码。|Paasword\_A|
    |用户类型|消息队列Kafka版支持的SASL机制如下：     -   PLAIN： 一种简单的用户名密码校验机制。消息队列Kafka版优化了PLAIN机制，支持不重启实例的情况下动态增加SASL用户。
    -   SCRAM：一种用户名密码校验机制，安全性比PLAIN更高。消息队列Kafka版使用SCRAM-SHA-256。
|PLAIN|

    创建完成后，**SASL用户**页签下方显示您创建的SASL用户。


## 步骤五：授予SASL用户权限

为员工A创建SASL用户后，为该SASL用户授予从Topic和Consumer Group读取消息的权限。

1.  在消息队列Kafka版控制台的**实例详情**页面，选择开启了ACL的实例，单击**SASL权限**页签。

2.  在**SASL权限**页签，单击**创建ACL**。

3.  在**创建ACL**对话框，填写ACL信息，然后单击**创建**。

    ![pg_read_from_Topic](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/7247559951/p99574.png)

4.  在**SASL权限**页签，单击**创建ACL**。

5.  在**创建ACL**页面，填写ACL信息，然后单击**创建**。

    ![pg_read_from_Topic](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/7247559951/p99587.png)

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


## 步骤六：查询SASL用户权限

授权完成后，查询该SASL用户的权限。

1.  在**SASL权限**页签下方：

    1.  从**用户名**列表，选择User\_A。

    2.  单击**全匹配**。

    3.  单击**Topic**。

    4.  从**资源名称**列表，选择通配符\*。

    5.  单击**查询**。

2.  在**SASL权限**页签下方：

    1.  单击**Group**。

    2.  从**资源名称**列表，选择通配符\*。

    3.  单击**查询**。


## 更多信息

开启ACL后，您可以通过SASL接入点接入消息队列Kafka版并收发消息：

-   [SASL接入点PLAIN机制收发消息](/cn.zh-CN/SDK参考/Java SDK/SASL接入点PLAIN机制收发消息.md)
-   [SASL接入点SCRAM机制收发消息](/cn.zh-CN/SDK参考/Java SDK/SASL接入点SCRAM机制收发消息.md)

