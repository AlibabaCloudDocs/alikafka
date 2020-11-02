# Step 1: Obtain the access permission

To enable the Message Queue for Apache Kafka instance to access resources in other Alibaba Cloud products, you must first grant the Message Queue for Apache Kafka instance the permission to access resources in your Alibaba Cloud products.

## Background

To facilitate quick activation of the service, Message Queue for Apache Kafka creates a default Resource Access Management \(RAM\) user for your account to conveniently access Alibaba Cloud resources.

**Note:** RAM is the access control system of Alibaba Cloud. You can control access to your resources by creating RAM users and roles and configuring corresponding permissions for them. For more information about RAM, see [Authorize RAM users](/intl.en-US/Access control/Grant permissions to RAM users.md).

## Prerequisites

You have [registered an Alibaba Cloud account](https://account.alibabacloud.com/register/intl_register.htm) and completed real-name verification.

## Procedure

1.  Log on to the [Alibaba Cloud International site](https://cn.aliyun.com/).
2.  On the [Cloud Resource Access Authorization](https://ram.console.aliyun.com/#/role/authorize?request=%7B%22Requests%22%3A%20%7B%22request1%22%3A%20%7B%22RoleName%22%3A%20%22AliyunKafkaDefaultRole%22%2C%20%22TemplateId%22%3A%20%22DefaultRole%22%7D%7D%2C%20%22ReturnUrl%22%3A%20%22https%3A//kafka.console.aliyun.com/%22%2C%20%22Service%22%3A%20%22Kafka%22%7D) page, click **Confirm Authorization Policy**.

    ![authorization](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/4437871951/p53126.png)

    Then, the page jumps to the Message Queue for Apache Kafka console.


