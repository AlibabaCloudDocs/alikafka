# Service linked role

This topic describes the background information, permission policy, usage notes, and frequently asked questions \(FAQ\) about the service linked role for Message Queue for Apache Kafka.

An Alibaba Cloud service may need to access other Alibaba Cloud services to implement a feature. In this case, the Alibaba Cloud service must assume a service linked role, which is a RAM role, to obtain the permissions to access other Alibaba Cloud services. When you use the feature in the console of the Alibaba Cloud service for the first time, the system notifies you that the service linked role is automatically created. For more information, see [Service linked roles](/intl.en-US/RAM Role Management/Service linked roles.md).

Message Queue for Apache Kafka assumes the following service linked role:

AliyunServiceRoleForAlikafkaConnector: Message Queue for Apache Kafka assumes this RAM role to obtain the permission to access Function Compute and implement the Function Compute sink connector feature. When you create a Function Compute sink connector in the Message Queue for Apache Kafka console for the first time, the system notifies you that the AliyunServiceRoleForAlikafkaConnector role is automatically created. For more information, see [Create a Function Compute sink connector](/intl.en-US/User guide/Connectors/Create a connector/Create a Function Compute sink connector.md).

## Permission policy

The following permission policy is attached to the AliyunServiceRoleForAlikafkaConnector role:

```
{
    "Version": "1",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "fc:InvokeFunction",
                "fc:GetFunction"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": "ram:DeleteServiceLinkedRole",
            "Resource": "*",
            "Condition": {
                "StringEquals": {
                    "ram:ServiceName": "connector.alikafka.aliyuncs.com"
                }
            }
        }
    ]
}
```

## Usage notes

If you delete the service linked role that is automatically created, you cannot use the feature that depends on the service linked role due to insufficient permissions. Exercise caution when you delete the role. For more information about how to create the service linked role again and grant permissions to the service linked role, see [Create a RAM role for a trusted Alibaba Cloud service](/intl.en-US/RAM Role Management/Create a RAM role/Create a RAM role for a trusted Alibaba Cloud service.md) and [Grant permissions to a RAM role](/intl.en-US/RAM Role Management/Grant permissions to a RAM role.md).

## FAQ

Why is the service linked role for Message Queue for Apache Kafka AliyunServiceRoleForAlikafkaConnector not automatically created for my RAM user?

If the service linked role has been created for your Alibaba Cloud account, your RAM user inherits the service linked role of your Alibaba Cloud account. If your RAM user fails to inherit the service linked role, log on to the [RAM console](https://ram.console.aliyun.com/), create the following custom permission policy, and then attach the custom permission policy to the RAM user:

```
{
    "Statement": [
        {
            "Action": [
                "ram:CreateServiceLinkedRole"
            ],
            "Resource": "acs:ram:*:${accountid}:role/*",
            "Effect": "Allow",
            "Condition": {
              "StringEquals": {
                "ram:ServiceName": "connector.alikafka.aliyuncs.com"
                }
            }
        }
    ],
    "Version": "1"
}
```

**Note:** Replace $\{accountid\} with the ID of your Alibaba Cloud account.

If the service linked role is still not automatically created for your RAM user after you attach the permission policy to the RAM user, attach the AliyunKafkaFullAccess permission policy to the RAM user. For more information, see [Grant permissions to a RAM user](/intl.en-US/RAM User Management/Grant permissions to a RAM user.md).

