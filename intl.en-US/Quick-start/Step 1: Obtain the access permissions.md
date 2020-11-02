# Step 1: Obtain the access permissions

To enable the Message Queue for Apache Kafka instance to access resources in other Alibaba Cloud services, you must first grant the Message Queue for Apache Kafka instance the permissions to access resources in your Alibaba Cloud services.

An Alibaba Cloud account is created and real-name verification is completed. For more information, see[Create your Alibaba Cloud account](https://account.alibabacloud.com/register/intl_register.htm).

To facilitate quick activation of the service, Message Queue for Apache Kafka creates a default Resource Access Management \(RAM\) user for your account to access Alibaba Cloud resources.

**Note:** RAM is the access control system of Alibaba Cloud. You can control access to your resources by creating RAM users and roles and configuring corresponding permissions for them. For more information about RAM, see [Authorize RAM users](/intl.en-US/Access control/Grant permissions to RAM users.md).

1.  Log on tothe .

2.  On the [Cloud Resource Access Authorization](https://ram.console.aliyun.com/#/role/authorize?request=%7B%22Requests%22%3A%20%7B%22request1%22%3A%20%7B%22RoleName%22%3A%20%22AliyunKafkaDefaultRole%22%2C%20%22TemplateId%22%3A%20%22DefaultRole%22%7D%7D%2C%20%22ReturnUrl%22%3A%20%22https%3A//kafka.console.aliyun.com/%22%2C%20%22Service%22%3A%20%22Kafka%22%7D) page, click **Confirm Authorization Policy**.

    ![Access authorization](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/4515504061/p120788.png)


Then, the page jumps to the Message Queue for Apache Kafka console.

Purchase a Message Queue for Apache Kafka instance and deploy it based on the network type.

-   [Access from a VPC](/intl.en-US/Quick-start/Step 2: Purchase and deploy an instance/Access from VPC.md)

