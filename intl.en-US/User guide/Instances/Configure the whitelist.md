---
keyword: [kafka, whitelist]
---

# Configure the whitelist

You can modify the whitelist to allow specified IP addresses or ports to access your Message Queue for Apache Kafka instance.

## Prerequisites

A Message Queue for Apache Kafka instance is purchased and deployed, and is in the **Running** state.

## Procedure

Perform the following steps to add IP addresses or CIDR blocks to the whitelist of your instance:

1.  Log on to the [Message Queue for Apache Kafka console](http://kafka.console.aliyun.com).
2.  In the top navigation bar, select the region where your instance is located.
3.  In the left-side navigation pane, click **Instances**.
4.  On the **Instances** page, click the name of your instance.
5.  On the **Instance Details** page, click **Security Change** in the **Security Configuration** section.
6.  In the **Security Change** dialog box, click **+ Add IP to Whitelist**, enter IP addresses or CIDR blocks, and then click **Add**.

    **Note:**

    -   The whitelist can contain a maximum of 100 entries.
    -   You can add a maximum of 10 IP addresses or CIDR blocks in each entry to the whitelist. Separate them with commas \(,\).
    -   You can remove or add a single entry from or to the whitelist.
    -   You can remove the last entry from the whitelist. Proceed with caution because the port range of the Message Queue for Apache Kafka instance are inaccessible after you remove the last entry.
    The operations differ slightly for instances of different network types, with differences in port ranges.

    -   Instances of the VPC type
        -   The port range is 9092/9092. By default, 0.0.0.0/0 is added to the whitelist. Clients can connect to the instance from a virtual private cloud \(VPC\).

            ![pg_9092_vpc_whitelist](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0940549951/p99676.png)

        -   The port range is 9094/9094. By default, the CIDR block that you specify for the vSwitch when you deploy the instance is added to the whitelist. Clients can connect to the instance from the vSwitch of the VPC.

            **Note:** The port range 9094/9094 is displayed only after the access control list \(ACL\) feature is enabled. For more information about how to enable the ACL feature, see [Authorize SASL users](/intl.en-US/Access control/Authorize SASL users.md).

            ![pg_whitelist_vpc](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/1940549951/p99675.png)

    -   Instances of the Internet and VPC type
        -   Access from a VPC
            -   The port range is 9092/9092. By default, the CIDR block that you specify for the vSwitch when you deploy the instance is added to the whitelist. Clients can connect to the instance from the vSwitch of the VPC.

                ![pg_9092_whitelist_internet](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/1940549951/p99677.png)

            -   The port range is 9094/9094. By default, the CIDR block that you specify for the vSwitch when you deploy the instance is added to the whitelist. Clients can connect to the instance from the vSwitch of the VPC.

                **Note:** The port range 9094/9094 is displayed only after the ACL feature is enabled. For more information about how to enable the ACL feature, see [Authorize SASL users](/intl.en-US/Access control/Authorize SASL users.md).

                ![pg_whitelist_intenet](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/1940549951/p99674.png)

        -   Access from the Internet: The port range is 9093/9093. By default, 0.0.0.0/0 is added to the whitelist. Clients can connect to the instance over the Internet. Data security is guaranteed by using the permission authentication and data encryption mechanisms.

            ![pg_9093_whitelist_internet](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/1940549951/p99678.png)

7.  Optional. To delete the whitelist configuration, click the Delete icon in the row where the relevant IP address or CIDR block is located in the **Security Change** dialog box.

