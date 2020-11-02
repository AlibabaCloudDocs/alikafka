# Monitor resources and set alerts

Message Queue for Apache Kafka allows you to monitor the resources created under your account, including instances, topics, and consumer groups. This helps you know the status of these resources in minutes.

Currently, the following metrics are provided for various resource types:

-   Instance metrics:

    -   Production Traffic of Instance Messages \(bytes/s\)

    -   Consumption Traffic of Instance Messages \(bytes/s\)

    -   Instance Disk Usage \(%\)

    **Note:** Instance Disk Usage \(%\) indicates the maximum disk usage of each node of the instance.

-   Topic metrics:

    -   Production Traffic of Topic Messages \(bytes/s\)

    -   Consumption Traffic of Topic Messages \(bytes/s\)

-   Consumer group metrics:

    -   Total Messages Not Consumed by Consumer Group

You can also set alert rules for these metrics. Message Queue for Apache Kafka connects to CloudMonitor. Therefore, you can directly create alert rules in the CloudMonitor console. When the metric value exceeds the alert threshold you set, CloudMonitor notifies you through SMS, email, TradeManager, or DingTalk chatbot to help you deal with exceptions in a timely manner.

## View monitoring data

No matter whether you set alerts, you can view the statistics of resource metrics in the Message Queue for Apache Kafka console.

-   Prerequisites
    -   You have created an instance, a topic, and a consumer group. For more information, see [Step 3: Create resources](/intl.en-US/Quick-start/Step 3: Create resources.md).

    -   The consumer group you created has subscribed to the topic. For more information, see [Message Queue for Apache Kafka demos](https://github.com/AliwareMQ/aliware-kafka-demos?spm=a2c4g.11186623.2.13.2f79f4f3OtONsI).

-   Procedure
    1.  Log on to the [Message Queue for Apache Kafka console](https://kafka.console.aliyun.com). In the top navigation bar, select the region where the target resource is located, for example, **China \(Hangzhou\)**.

    2.  In the left-side navigation pane, click **Monitoring and Alerts**. On the Monitoring and Alerts page, select the target resource.

        -   To view the monitoring data of an instance, click the **Instance** tab.

        -   To view the monitoring data of a topic or consumer group, select the instance of the topic or consumer group on the top of the page, and then click **Topic** or **Consumer Group**.

    3.  Find the target resource, and click **View Monitoring** in the **Actions** column.

        You can view the data of the last 1 hour, 3 hours, 6 hours, 12 hours, 1 day, 3 days, 7 days, or 14 days, or click the time range picker to select a time range.

        If you want to specify a time range, you can view data for the last 31 days at most \(data generated 31 days earlier is not retained\), that is, in the time range picker, **End Time** is the current system time, and **Start Time** is up to 31 days ago. If **End Time** is not the current system time, you can view data for any 7 days in the last 31 days.

        **Note:** The data aggregation cycle of a metric is 1 minute, that is, the metric is calculated once every minute. The calculated value in bytes/s can be considered as the average value of the metric within 1 minute.

-   Verification

    Corresponding metrics and monitoring data appear under the resource.

    ![metric](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/7150549951/p53152.png)


## Set an alert

You can create alert rules to receive notifications about exceptions in a timely manner.

-   Prerequisites

    You have created an instance, a topic, and a consumer group. For more information, see [Step 3: Create resources](/intl.en-US/Quick-start/Step 3: Create resources.md).

-   Procedure
    1.  Log on to the [Message Queue for Apache Kafka console](https://kafka.console.aliyun.com). In the top navigation bar, select the region where the target resource is located, for example, **China \(Hangzhou\)**.

    2.  In the left-side navigation pane, click **Monitoring and Alerts**. On the Monitoring and Alerts page, select the target resource.

        -   To set an alert for an instance, click the **Instance** tab.

        -   To set an alert for a topic or consumer group, select the instance of the topic or consumer group on the top of the page, and then click **Topic** or **Consumer Group**.

    3.  Find the target resource, and click **Set Alerts** in the **Actions** column.

        The page is redirected to the Create Alarm Rule page of the CloudMonitor console.

    4.  On the Create Alarm Rule page, set the alert rule and notification method. For more information, see [t6130.md\#](/intl.en-US/Quick Start/Alert service.md).

        ![alarm](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/8150549951/p53153.png)

        **Note:**

        -   The system does not support batch alert setting across instances.

        -   When you set an alert rule for metrics **Production Traffic of Topic Messages** and **Consumption Traffic of Topic Messages**, we recommend that you do not select **Any** in the **Topic** field. When **Any** is selected, all topics are selected.

        -   Avoid using "between" and multiple expressions when setting specific rules.

        -   In CloudMonitor, you can set up to 50 alert rules for free. If you want to set more rules, you need to upgrade your CloudMonitor instance.

-   Verification

    For more information, see [View alert information](#section_tkb_9jp_i02).


## View alert information

You can view the alert rules you created and the corresponding alert information.

-   Prerequisites

    You have created an alert rule. For more information, see [Set an alert](#section_z48_x4u_glc).

-   Procedure
    1.  Log on to the [Message Queue for Apache Kafka console](https://kafka.console.aliyun.com). In the top navigation bar, select the region where the target resource is located, for example, **China \(Hangzhou\)**.

    2.  In the left-side navigation pane, click **Monitoring and Alerts**. On the Monitoring and Alerts page, select the target resource.

        -   To view the alert information of an instance, click the **Instance** tab.

        -   To view the alert information of a topic or consumer group, select the instance of the topic or consumer group on the top of the page, and then click **Topic** or **Consumer Group**.

    3.  On the Monitoring and Alerts page, you can view the alert details in either of the following ways:

        -   Click **View Alert Information**.

            The page is redirected to the Alarm Rules page in the CloudMonitor console. By default, all alert rules of Message Queue for Apache Kafka and their statuses are displayed. You can view, modify, disable, enable, and delete alert rules.

        -   Click **Alert Items: X** in **Alert Items** of a resource. X indicates the number of alert rules you have set for this resource.

            In the Alert Items window, view all the alert rules of the resource and corresponding alert information. Find the target alert rule and click **View** in the **Actions** column. The Basic Information page of this alert rule appears in the CloudMonitor console. On this page, you can view all the information of this alert rule, and can modify, disable, enable, and delete the alert rule.


