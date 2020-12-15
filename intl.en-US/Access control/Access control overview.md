---
keyword: [kafka, ram, acl]
---

# Access control overview

This topic describes two access control mechanisms supported by Message Queue for Apache Kafka: Resource Access Management \(RAM\) and access control list \(ACL\).

|Access control mechanism|Description|Documentation|
|------------------------|-----------|-------------|
|RAM|RAM is a service provided by Alibaba Cloud to manage user identities and resource access permissions. You can only grant permissions to RAM users in the Message Queue for Apache Kafka console or by using the API operations. No matter whether RAM users are authorized or not, RAM users can use SDKs to send and receive messages. For more information, see [What is RAM?](/intl.en-US/Product Introduction/What is RAM?.md).|-   [t1848773.md\#](/intl.en-US/Access control/Grant permissions to RAM users.md)
-   [Grant permissions across Alibaba Cloud accounts](/intl.en-US/Access control/Grant permissions across Alibaba Cloud accounts.md)
-   [RAM policies](/intl.en-US/Access control/RAM policies.md)
-   [Service linked roles](/intl.en-US/Access control/Service linked roles.md) |
|ACL|The ACL feature is provided by Message Queue for Apache Kafka to manage the permissions of Simple Authentication and Security Layer \(SASL\) users and clients to send and receive messages by using SDKs. This is consistent with the ACL feature in open source Apache Kafka. The ACL feature is only applicable to scenarios where you want to implement access control for users that use SDKs to send and receive messages. This feature is not applicable to scenarios where you want to implement access control for users that send and receive messages in the Message Queue for Apache Kafka console or by using API operations. For more information, see [Authorization and ACLs](http://kafka.apache.org/090/documentation.html#security_authz).|[Authorize SASL users](/intl.en-US/Access control/Authorize SASL users.md)|

