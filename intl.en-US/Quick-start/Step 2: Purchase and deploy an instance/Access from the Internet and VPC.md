---
keyword: [internet, kafka, apache kafka, vpc]
---

# Access from the Internet and VPC

If you want to access Message Queue for Apache Kafka from a virtual private cloud \(VPC\) and over the Internet, you must purchase and deploy an instance of the Internet and VPC type. You cannot change the network type after you select it.

-   Message Queue for Apache Kafka is granted permissions to access resources of other cloud services. For more information, see [Step 1: Obtain the access permissions](/intl.en-US/Quick-start/Step 1: Obtain the access permissions.md).
-   A VPC is created. For more information, see [Create a VPC](/intl.en-US/VPCs and VSwitches/VPC management/Create a VPC.md).

## Step 1: Purchase a Message Queue for Apache Kafka instance

1.  Log on to the [Message Queue for Apache Kafka console](http://kafka.console.aliyun.com/).

2.  In the top navigation bar, select the region where you want to purchase an instance.

3.  In the left-side navigation pane, click **Instances**.

4.  On the **Instances** page, click **Purchase Instance**.

5.  On the buy page, select **Internet and VPC** for **Network**, set other parameters as needed, and then click **Buy Now**.

6.  Confirm and pay for the order.


## Step 2: Obtain the VPC information

1.  Log on to the [VPC console](https://vpcnext.console.aliyun.com/).

2.  Select the region where your VPC is deployed.

3.  In the left-side navigation pane, click **VSwitches**.

4.  On the **VSwitches** page, click the instance whose vSwitch ID and VPC ID you want to view. On the page that appears, view the vSwitch ID and VPC ID.


## Step 3: Deploy the instance

1.  In the Message Queue for Apache Kafka console, click **Overview** in the left-side navigation pane. Find the instance that you want to deploy, and then click **Deploy**.

2.  In the **Deploy** dialog box, deploy the instance.

    1.  From the **VPC ID** drop-down list, select your VPC ID.

    2.  From the **VSwitch ID** drop-down list, select your vSwitch ID.

        After you select the vSwitch ID, the system automatically selects the zone where the vSwitch is located.

    3.  If the instance edition is the Professional Edition, you can select whether to deploy the instance across zones.

        Deployment across zones ensures high disaster recovery capabilities and can withstand breakdowns in data centers.

    4.  Select a value for Reset the username and password.

        -   Yes: If you select this option, customize the username and password. This option applies to scenarios where multiple instances share the same username and password.
        -   No: If you select this option, the default username and password that the system assigns for each instance are used.
    5.  Click **Deploy**.

    The instance enters the **Deploying** state. The instance deployment takes about 10 to 30 minutes.


## Step 4: View instance details

1.  In the Message Queue for Apache Kafka console, click **Instances** in the left-side navigation pane. On the Instances page, click the name of the instance whose details you want to view.

2.  On the **Instance Details** page, view the endpoint, username, and password of the instance.

    1.  In the **Basic Information** section, view the endpoint of the instance.

        -   Default Endpoint: is used for access from the VPC.
        -   SSL Endpoint: is used for access over the Internet.
    2.  In the **Security Configuration** section, view **Username** and **Password**.


[Step 3: Create resources](/intl.en-US/Quick-start/Step 3: Create resources.md)

