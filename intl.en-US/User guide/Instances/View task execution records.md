# View task execution records

After restart tasks are initiated for a Message Queue for Apache Kafka instance, you can view the execution records of these restart tasks in the Message Queue for Apache Kafka console to obtain the information such as the task type, start time, end time, and status. Restart tasks are tasks that you change configurations, upgrade the version, enable the access control list \(ACL\) feature, or upgrade specifications of an instance.

A Message Queue for Apache Kafka instance is purchased and deployed, and it is in the Running state.

1.  Log on to the [Message Queue for Apache Kafka console](https://kafka.console.aliyun.com/?spm=a2c4g.11186623.2.22.6bf72638IfKzDm).

2.  In the top navigation bar, select the region where your instance is located.

3.  In the left-side navigation pane, click **Instances**.

4.  On the **Instances** page, click the name of the instance that you want to manage.

5.  On the **Instance Details** page, click the **Task Records** tab.

    On the **Task Records** tab, the execution records of restart tasks appear in the lower part.

    |Parameter|Description|Example|
    |---------|-----------|-------|
    |Task Type|The type of the executed task. Valid values:     -   Configuration Change: modifies the message configurations of the instance, including the message retention period and the maximum message size. For more information, see [Modify the message configuration](/intl.en-US/User guide/Instances/Modify the message configuration.md).
    -   Upgrade Specification: upgrades the configurations of the instance, including the instance edition, network type, traffic specification, disk capacity, and supported topics. For more information, see [Upgrade instance specifications](/intl.en-US/User guide/Instances/Upgrade instance specifications.md).
    -   Upgrade Version: upgrades the major and minor versions of the instance. For more information, see [Upgrade the instance version](/intl.en-US/User guide/Instances/Upgrade the instance version.md).
    -   Enable ACL: enables the ACL feature provided by Message Queue for Apache Kafka. This feature is used to manage Simple Authentication and Security Layer \(SASL\) users and resource access. For more information, see [Authorize SASL users](/intl.en-US/Access control/Authorize SASL users.md).
|Upgrade Specification|
    |Started At|The time when the task starts to be executed.|2020-05-27 10:46:11|
    |Ended At|The time when the task execution ends.|2020-05-27 11:16:31|
    |Status|The status of the task. Valid values:     -   Not Executed
    -   Executing
    -   Executed
    -   Canceled
|Executed|


