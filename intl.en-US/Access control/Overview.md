---
keyword: [kafka, ram, acl]
---

# Overview

This topic describes two access control mechanisms supported by Message Queue for Apache Kafka: Resource Access Management \(RAM\) and access control list \(ACL\).

|Access control mechanism|Description|Documentation|
|------------------------|-----------|-------------|
|RAM|RAM is a service provided by Alibaba Cloud to manage user identities and resource access permissions. You can only grant permissions to RAM users in the Message Queue for Apache Kafka console or by using the corresponding API operations. No matter whether RAM users are authorized or not, RAM users can use SDKs to send and subscribe to messages. For more information, see [What is RAM?](/intl.en-US/Product Introduction/What is RAM?.md).|-   [RAM policies](/intl.en-US/Access control/RAM policies.md)
-   [Authorize RAM users](/intl.en-US/Access control/Grant permissions to RAM users.md)
-   [Grant permissions across Alibaba Cloud accounts](/intl.en-US/Access control/Grant permissions across Alibaba Cloud accounts.md)
-   [Service linked role](/intl.en-US/Access control/Service linked role.md) |
|ACL|The ACL feature is provided by Message Queue for Apache Kafka to manage the permissions of Simple Authentication and Security Layer \(SASL\) users and clients to send and subscribe to messages by using SDKs. It is consistent with the ACL feature in open-source Apache Kafka. The ACL feature is only applicable to scenarios where you want to implement access control for users that use Message Queue for Apache Kafka SDK to send and subscribe to messages. It is not applicable to scenarios where you want to implement access control for users that send and subscribe to messages in the Message Queue for Apache Kafka console or by using API operations. For more information, see [Authorization and ACLs](http://kafka.apache.org/090/documentation.html#security_authz).|[Authorize SASL users](/intl.en-US/Access control/Authorize SASL users.md)|

