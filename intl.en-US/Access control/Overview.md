# Overview

This topic describes two access control mechanisms supported by Message Queue for Apache Kafka, namely, Resource Access Management \(RAM\) and access control list \(ACL\).

## RAM

RAM is a service provided by Alibaba Cloud to manage user identities and resource access permissions. You can only grant permissions to RAM users in the console or by using the corresponding API operations. No matter whether the RAM users are authorized or not, the RAM users can use the Message Queue for Apache Kafka SDK to send and subscribe to messages. For more information about how to grant permissions to RAM users, see [Authorize RAM users](/intl.en-US/Access control/Grant permissions to RAM users.md).

## ACL

The ACL feature is provided by Alibaba Cloud Message Queue for Apache Kafka to manage Simple Authentication and Security Layer \(SASL\) users of Message Queue for Apache Kafka and permissions to access Message Queue for Apache Kafka resources. The ACL feature is only applicable to scenarios where you want to implement access control for SASL users that use the Message Queue for Apache Kafka SDK to send and subscribe to messages. That is, the ACL feature is not applicable to scenarios where you want to implement access control for users that send and subscribe to messages in the console or by using API operations. For more information about how to authorize an SASL user, see [Authorize SASL users](/intl.en-US/Access control/Authorize SASL users.md).

