# Consumer Group删除后依然可以收到消息？

老版本删除Consumer Group时，不会彻底清除路由。

## Condition

某个Consumer Group被删除后，依然可以收到消息。

## Cause

老版本删除Consumer Group时，不会彻底清除路由，导致被删除的Consumer Group依然可以收到消息。升级到新版本后，那些曾经在老版删除过的Consumer Group，其路由仍然保留着，为了对其进行彻底清除，需要“创建-\>删除-\>再创建”Consumer Group，待该流程完成之后，您可以再次创建Consumer Group。

## Remedy

1.  将服务版本升级为最新版。

    在[控制台](https://kafka.console.aliyun.com/?spm=a2c4g.11186623.2.22.6bf72638IfKzDm)，进入**实例详情**的**配置信息**页签，在**小版本**右侧查看实例版本。

    -   如果显示为**当前版本为最新版本**，则无需处理。
    -   如果显示为**升级小版本**，请单击**升级小版本**，完成版本升级。
2.  “创建-\>删除-\>再创建”Consumer Group

    进入**Group 管理**页面，创建之前删除的Consumer Group，删除该Consumer Group，然后再次创建Consumer Group。


