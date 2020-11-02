# Consumer Group删除后依然可以收到消息？

老版本删除Consumer Group时，不会彻底清除路由。

## Condition

某个Consumer Group被删除后，依然可以收到消息。

## Cause

老版本删除Consumer Group时，不会彻底清除路由，导致被删除的Consumer Group依然可以收到消息。升级到新版本后，那些曾经在老版删除过的Consumer Group，其路由仍然保留着，为了对其进行彻底清除，需要走一遍“创建—删除”的流程。这个流程走完之后，您可以再次创建Consumer Group。

## Remedy

1.  将服务版本升级为最新版

    将服务版本升级为最新版

    在消息队列Kafka版控制台，进入**实例详情**页面，在**基本信息**区域的**内部版本**的右侧，查看服务版本：

    -   如果显示为**最新版本**，则无需处理。
    -   如果显示为**服务版本升级**，请单击**服务版本升级**，完成版本升级。
2.  创建—删除—再创建

    进入**Consumer Group管理**页面，创建之前删除的Consumer Group，删除该Consumer Group，然后再次创建Consumer Group。


