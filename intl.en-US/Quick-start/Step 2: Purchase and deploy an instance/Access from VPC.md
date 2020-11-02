# Access from VPC

This topic describes how to access a Message Queue for Apache Kafka instance from a Virtual Public Cloud \(VPC\).

## Prerequisites

You have created your own Alibaba Cloud VPC instance.

## Procedure

1.  Purchase an instance
    1.  Log on to the [Message Queue for Apache Kafkaconsole](http://kafka.console.aliyun.com/).
    2.  In the top navigation bar of the console, select a region for the instance you want to purchase and deploy based on your needs.

        **Note:** The Message Queue for Apache Kafka instance must be deployed in the region that you selected when purchasing the instance.

        ![pg_region_bar ](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/8437871951/p94222.png)

    3.  In the Message Queue for Apache Kafka console, click **Overview** in the left-side navigation pane.
    4.  On the Instances page, click **Purchase Instance**.

        ![purchaseinstance](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/9437871951/p53131.png)

    5.  On the Purchase Instance page, configure the instance based on your needs. Select **VPC** for **Instance Type**.

        ![vpcorder ](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/9437871951/p94348.png)

    6.  Click **Buy Now** on the right and purchase the instance as prompted.
2.  Obtain the VPC information
    1.  Log on to the [VPC console](https://vpcnext.console.aliyun.com/). In the left-side navigation pane, click **VSwitches**.
    2.  On the VSwitches page, view the following information:

        -   VSwitch ID
        -   VPC ID
        -   Zone
        ![vpcinfo](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/9437871951/p53133.png)

        **Note:** In the Message Queue for Apache Kafka console, select the zone \(A to G\) displayed on this page. For example, if a VSwitch of the VPC is in zone E, select zone E in the Message Queue for Apache Kafka console.

3.  Deploy the instance
    1.  In the Message Queue for Apache Kafka console, click **Overview** in the left-side navigation pane to view the purchased instance.
    2.  Select an instance in the **Pending Deployment** state, click **Deploy** next to the instance, and then enter the VPC information you obtained in Step 2 as prompted.

        Then, the instance enters the **Deploying** state. It will take about 10 to 30 minutes to deploy the instance.

        **Note:** Professional edition supports cross region deployment.

    3.  Refresh the page. When the status of the instance is **Running**, the instance is deployed successfully.
4.  View instance details
    1.  After the instance is deployed, click **Details** next to the instance to view the instance details.

        ![viewdetails](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/9437871951/p53135.png)

        **Note:** Alternatively, click **Instance Details** in the left-side navigation pane, and select the target instance on the page that appears to view its details.

    2.  On the Instance Details page, view the **default endpoint** of the instance.

        **Note:** If you access the Message Queue for Apache Kafka instance from a VPC, use the **default endpoint**.

        ![default_endpoint ](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/9437871951/p94352.png)


