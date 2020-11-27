# Monitor resources and generate alerts

Message Queue for Apache Kafka allows you to monitor the status of your resources in real time, including instances, topics, and consumer groups. You can also configure alert rules for metrics. If a metric value exceeds the specified alert threshold, Cloud Monitor sends a notification to you by using a voice message, SMS message, email, or DingTalk chatbot message. This allows you to troubleshoot issues at the earliest opportunity.

## Metrics

**Note:**

-   The data of each metric is aggregated every minute, that is, the number of bytes per second is calculated every minute. The data aggregated within one minute can be considered as the average value of this minute.
-   The data latency of each metric is one minute.

The following metrics are provided for various resource types:

-   Instance metrics

    -   Instance Message Input \(bytes/s\)
    -   Instance Message Output \(bytes/s\)
    -   Instance Disk Utilization \(%\)
    **Note:** Instance Disk Utilization \(%\) indicates the maximum disk utilization of each node of the instance.

-   Topic metrics
    -   Topic Message Input \(bytes/s\)
    -   Topic Message Output \(bytes/s\)
-   Consumer group metrics
    -   Message Accumulation \(unit\)

## View monitoring data

To view the monitoring data, perform the following steps:

1.  Log on to the [Message Queue for Apache Kafka console](https://kafka.console.aliyun.com).

2.  In the top navigation bar, select the region where the resource resides.

3.  In the left-side navigation pane, click **Monitoring**.

4.  On the **Monitoring and Alerts** page, select an instance and click the resource tab. On the resource tab, find the resource, click **Actions** in the **View Monitoring** column, and then set a time range.


## Set an alert

To set an alert, perform the following steps:

1.  Log on to the [Message Queue for Apache Kafka console](https://kafka.console.aliyun.com).

2.  In the top navigation bar, select the region that the resource resides.

3.  In the left-side navigation pane, click **Monitoring**.

4.  On the **Monitoring and Alerts** page, select an instance and click the resource tab. On the resource tab, find the resource, and click **Actions** in the **Set Alert** column.

5.  On the **Create Alert Rule** page, set the alert rules and notification method, and then click **Confirm**.


## View alert information

To view alert information, perform the following steps:

1.  Log on to the [Message Queue for Apache Kafka console](https://kafka.console.aliyun.com).

2.  In the top navigation bar, select the region where the resource resides.

3.  In the left-side navigation pane, click **Monitoring**.

4.  On the **Monitoring and Alerts** page, select an instance and click the resource tab. On the resource tab, click **View Alert Information**.

5.  On the **Alert Rules** page, you can view, modify, enable or disable, and delete alert rules. You can also view historical alerts.


