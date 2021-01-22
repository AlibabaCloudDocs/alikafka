# Service-linked roles

This topic describes the background information, policies, and considerations of the service-linked roles in Message Queue for Apache Kafka and provides answers to frequently asked questions about these roles.

An Alibaba Cloud service may need to access other Alibaba Cloud services to provide a feature. To do this, the Alibaba Cloud service must assume a service-linked role to obtain the permissions to access other Alibaba Cloud services. The service-linked role is a Resource Access Management \(RAM\) role. When you use this feature in the console of the Alibaba Cloud service for the first time, the system automatically creates a service-linked role and instructs you to complete the process. For more information, see [Service-linked roles](/intl.en-US/RAM Role Management/Service-linked roles.md).

Message Queue for Apache Kafka can assume the following service-linked roles:

-   AliyunServiceRoleForAlikafkaConnector: Message Queue for Apache Kafka assumes this RAM role to obtain the permissions to access Function Compute. This enables the instance to provide the Function Compute sink connector feature. When you create a Function Compute sink connector in the Message Queue for Apache Kafka console for the first time, the system automatically creates the AliyunServiceRoleForAlikafkaConnector role and instructs you to complete the process. For more information, see [Create a Function Compute sink connector](/intl.en-US/User guide/Connectors/Create a connector/Create a Function Compute sink connector.md).

-   AliyunServiceRoleForAlikafkaInstanceEncryption: Message Queue for Apache Kafka assumes this RAM role to obtain the permissions to use the access control and encryption features provided by Key Management Service \(KMS\). This enables the instance to provide the encryption feature. The encryption feature of the instance can be used only by calling API operations. It will be provided in the console in the future. If you deploy an encrypted instance for the first time by calling the [StartInstance](/intl.en-US/API reference/Instances/StartInstance.md) operation provided in Message Queue for Apache Kafka, the system automatically creates the AliyunServiceRoleForAlikafkaInstanceEncryption role for you.

## Policies

-   The following policy is attached to the AliyunServiceRoleForAlikafkaConnector role:

    ```
    {
        "Version": "1",
        "Statement": [
            {
                "Effect": "Allow",
                "Action": [
                    "fc:InvokeFunction",
                    "fc:GetFunction",
                    "fc:ListServices",
                    "fc:ListFunctions",
                    "fc:ListServiceVersions",
                    "fc:ListAliases"
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

-   The following policy is attached to the AliyunServiceRoleForAlikafkaInstanceEncryption role:

    ```
    {
        "Version":"1",
        "Statement":[
            {
                "Action":[
                    "kms:Listkeys",
                    "kms:Listaliases",
                    "kms:ListResourceTags",
                    "kms:DescribeKey",
                    "kms:TagResource",
                    "kms:UntagResource"
                ],
                "Resource":"*",
                "Effect":"Allow"
            },
            {
                "Action":[
                    "kms:Encrypt",
                    "kms:Decrypt",
                    "kms:GenerateDataKey"
                ],
                "Resource":"*",
                "Effect":"Allow",
                "Condition":{
                    "StringEqualsIgnoreCase":{
                        "kms:tag/acs:alikafka:instance-encryption":"true"
                    }
                }
            },
            {
                "Action":"ram:DeleteServiceLinkedRole",
                "Resource":"*",
                "Effect":"Allow",
                "Condition":{
                    "StringEquals":{
                        "ram:ServiceName":"instanceencryption.alikafka.aliyuncs.com"
                    }
                }
            }
        ]
    }
    ```


## Precautions

If you delete a service-linked role that is automatically created by the system, the dependent feature can no longer be used due to insufficient permissions. Exercise caution when you delete a service-linked role. For more information about how to create the service-linked role again and grant permissions to the service-linked role, see [Create a RAM role for a trusted Alibaba Cloud service](/intl.en-US/RAM Role Management/Create a RAM role/Create a RAM role for a trusted Alibaba Cloud service.md) and [Grant permissions to a RAM role](/intl.en-US/RAM Role Management/Grant permissions to a RAM role.md).

## FAQ

-   Why is the AliyunServiceRoleForAlikafkaConnector role for Message Queue for Apache Kafka not automatically created for my RAM user?

    If the service-linked role has been created for your Alibaba Cloud account, your RAM user inherits the service-linked role of your Alibaba Cloud account. If your RAM user fails to inherit the service-linked role, log on to the [RAM console](https://ram.console.aliyun.com/), create the following custom policy, and then attach the custom policy to the RAM user:

    ```
    {
        "Statement": [
            {
                "Action": [
                    "ram:CreateServiceLinkedRole"
                ],
                "Resource": "*",
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

-   Why is the AliyunServiceRoleForAlikafkaInstanceEncryption role for Message Queue for Apache Kafka not automatically created for my RAM user?

    If the service-linked role has been created for your Alibaba Cloud account, your RAM user inherits the service-linked role of your Alibaba Cloud account. If your RAM user fails to inherit the service-linked role, log on to the [RAM console](https://ram.console.aliyun.com/), create the following custom policy, and then attach the custom policy to the RAM user:

    ```
    {
        "Statement":[
            {
                "Action":[
                    "ram:CreateServiceLinkedRole"
                ],
                "Resource":"*",
                "Effect":"Allow",
                "Condition":{
                    "StringEquals":{
                        "ram:ServiceName":"instanceencryption.alikafka.aliyuncs.com"
                    }
                }
            }
        ],
        "Version":"1"
    }
    ```


If the service-linked role is still not automatically created for your RAM user after you attach the policy to the RAM user, attach the AliyunKafkaFullAccess policy to the RAM user. For more information, see [Grant permissions to a RAM user](/intl.en-US/RAM User Management/Grant permissions to a RAM user.md).

