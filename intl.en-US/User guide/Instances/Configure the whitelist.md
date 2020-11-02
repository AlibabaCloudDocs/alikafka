---
keyword: [kafka, whitelist]
---

# Configure the whitelist

You can modify the whitelist to allow specified IP addresses or ports to access Message Queue for Apache Kafka instances.

## Prerequisites

A Message Queue for Apache Kafka instance is purchased and deployed, and it is in the **Running** state.

## Procedure

Perform the following steps to add an IP address or CIDR block to the whitelist of an instance:

1.  Log on to the [Message Queue for Apache Kafka console](http://kafka.console.aliyun.com).
2.  In the top navigation bar, select a region.
3.  In the left-side navigation pane, click **Instance Details**.
4.  On the **Instance Details** page, click the instance whose whitelist you want to modify. In the **Security Configuration** section, click **Security Change**.
5.  In the **Security Change** dialog box, click **+ Add IP to Whitelist**, enter IP addresses or CIDR blocks, and then click **Add**.

    **Note:**

    -   The whitelist can contain a maximum of 100 IP addresses or CIDR blocks.
    -   You can add a maximum of 10 IP addresses or CIDR blocks at a time to the whitelist. Separate them with commas \(,\).
    -   You can delete or add only one IP address or CIDR block from or to the whitelist.
    -   You can delete the last IP address or CIDR block from the whitelist. Proceed with caution because the port range of the Message Queue for Apache Kafka instance will be inaccessible after deletion.
    The operations differ slightly for instances of different network types, with differences in the port ranges.

    -   Instances of the VPC type
        -   The port range is 9092/9092. The default CIDR block in the whitelist is 0.0.0.0/0, allowing access to the Message Queue for Apache Kafka instance through virtual private cloud \(VPC\) networks.

            ![pg_9092_vpc_whitelist](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/0940549951/p99676.png)

        -   The port range is 9094/9094. The default CIDR block in the whitelist is that of the vSwitch specified during instance deployment, allowing access to the Message Queue for Apache Kafka instance in the vSwitch of the VPC.

            **Note:** The port range 9094/9094 is displayed only after the access control list \(ACL\) feature is enabled. For more information about how to enable the ACL feature, see [Step 1: Apply to enable the ACL feature](/intl.en-US/Access control/Authorize SASL users.md).

            ![pg_whitelist_vpc](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/1940549951/p99675.png)

6.  Optional. To delete the whitelist configuration, click the Delete icon in the row where the IP address or CIDR block to be deleted is located in the **Security Change** dialog box.

