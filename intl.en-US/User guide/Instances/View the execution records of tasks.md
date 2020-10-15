# View the execution records of tasks

After restart tasks, such as configuration change, version upgrade, ACL enabling, and specification upgrade, are initiated for a Message Queue for Apache Kafka instance, you can view the execution records of these restart tasks in the Message Queue for Apache Kafka console to obtain information such as the task type, start time, end time, and status.

A Message Queue for Apache Kafka instance is created and deployed. The instance is in the running state.

1.  Log on to the [Message Queue for Apache Kafka console](https://kafka.console.aliyun.com/).

2.  In the top navigation bar, select a region.

3.  In the left-side navigation pane, click **Instances**.

4.  On the **Instance Details** page, select an instance and click the **Task Records** tab.

    The execution records of restart tasks are displayed on the **Task Records** tab.

    |Parameter|Description|Example|
    |---------|-----------|-------|
    |Task Type|The type of the executed task. Valid values:     -   Configuration Change: You can modify the message configurations of the instance, including the message retention period and the maximum message size. For more information, see [Modify the message configuration](/intl.en-US/User guide/Instances/Modify the message configuration.md).
    -   Upgrade Specification: You can upgrade the configurations of the instance, including the edition, network type, traffic specification, disk capacity, and supported topics. For more information, see [Upgrade instance specifications](/intl.en-US/User guide/Instances/Upgrade instance specifications.md).
    -   Upgrade Version: You can upgrade the major and minor versions of the instance. For more information, see [Upgrade the instance version](/intl.en-US/User guide/Instances/Upgrade the instance version.md).
    -   Enable ACL: The ACL feature is provided by Alibaba Cloud Message Queue for Apache Kafka to manage simple authentication and security layer \(SASL\) users and resource access permissions. For more information, see [Authorize SASL users](/intl.en-US/Access control/Authorize SASL users.md).
|Upgrade Specification|
    |Started At|The time when the task starts to be executed.|2020-05-27 10:46:11|
    |Ended At|The time when the task execution ends.|2020-05-27 11:16:31|
    |Status|The current status of the task. Valid values:     -   Not Executed
    -   Executing
    -   Executed
    -   Canceled
|Executed|


