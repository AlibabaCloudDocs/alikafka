# Service-linked roles

This topic describes the background information, policies, and usage notes of service-linked roles in Message Queue for Apache Kafka and provides answers to frequently asked questions \(FAQ\) about these roles.

An Alibaba Cloud service may need to access other Alibaba Cloud services to implement a feature that it has. In this case, the Alibaba Cloud service must assume a service-linked role to obtain the permissions to access other Alibaba Cloud services. A service-linked role is a Resource Access Management \(RAM\) role. When you use the feature in the console of the Alibaba Cloud service for the first time, the system automatically creates a service-linked role and notifies you that the service-linked role is created. For more information, see [Service-linked roles](/intl.en-US/RAM Role Management/Service-linked roles.md).

Message Queue for Apache Kafka can assume the following service-linked roles:

-   AliyunServiceRoleForAlikafka: Message Queue for Apache Kafka assumes this RAM role to access other Alibaba Cloud services. If you activate Message Queue for Apache Kafka for the first time in the Message Queue for Apache Kafka console, the system automatically creates the AliyunServiceRoleForAlikafka role and notifies you that the role is created.
-   AliyunServiceRoleForAlikafkaConnector: Message Queue for Apache Kafka assumes this RAM role to obtain access permissions on the services to which connectors can connect. This way, Message Queue for Apache Kafka implements the connector feature. If you create a connector in the Message Queue for Apache Kafka console for the first time, the system automatically creates the AliyunServiceRoleForAlikafkaConnector role and notifies you that the role is created. For more information, see [Create a Function Compute sink connector](/intl.en-US/User guide/Connectors/Create a connector/Create a Function Compute sink connector.md).

-   AliyunServiceRoleForAlikafkaInstanceEncryption: Message Queue for Apache Kafka assumes this RAM role to obtain the access and encryption permissions of Key Management Service \(KMS\). This way, your instance can provide the encryption feature. The instance encryption feature can be used only by calling API operations. This feature will be provided in the console later. If you deploy an encrypted instance for the first time by calling the [StartInstance](/intl.en-US/API reference/Instances/StartInstance.md) operation provided in Message Queue for Apache Kafka, the system automatically creates the AliyunServiceRoleForAlikafkaInstanceEncryption role for you.

## Policies

-   The following policy is attached to the AliyunServiceRoleForAlikafka role:

    ```
    {
        "Version": "1",
        "Statement": [
            {
                "Action": [
                    "ecs:CreateNetworkInterface",
                    "ecs:DeleteNetworkInterface",
                    "ecs:DescribeNetworkInterfaces",
                    "ecs:CreateNetworkInterfacePermission",
                    "ecs:DescribeNetworkInterfacePermissions",
                    "ecs:DeleteNetworkInterfacePermission",
                    "ecs:CreateSecurityGroup",
                    "ecs:AuthorizeSecurityGroup",
                    "ecs:DescribeSecurityGroupAttribute",
                    "ecs:RevokeSecurityGroup",
                    "ecs:DeleteSecurityGroup",
                    "ecs:DescribeSecurityGroups"
                ],
                "Resource": "*",
                "Effect": "Allow"
            },
            {
                "Action": [
                    "vpc:DescribeVSwitches",
                    "vpc:DescribeVpcs"
                ],
                "Resource": "*",
                "Effect": "Allow"
            },
            {
                "Effect": "Allow",
                "Action": "ram:DeleteServiceLinkedRole",
                "Resource": "*",
                "Condition": {
                    "StringEquals": {
                        "ram:ServiceName": "alikafka.aliyuncs.com"
                    }
                }
            }
        ]
    }
    ```

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
                    "fc:ListAliases",
                    "fc:CreateService",
                    "fc:DeleteService",
                    "fc:CreateFunction",
                    "fc:DeleteFunction"
                ],
                "Resource": "*"
            },
            {
                "Action": [
                    "rds:DescribeDatabases"
                ],
                "Resource": "*",
                "Effect": "Allow"
            },
            {
                "Action": [
                    "oss:ListBuckets",
                    "oss:GetBucketAcl"
                ],
                "Resource": "*",
                "Effect": "Allow"
            },
            {
                "Action": [
                    "elasticsearch:DescribeInstance",
                    "elasticsearch:ListInstance"
                ],
                "Resource": "*",
                "Effect": "Allow"
            },
            {
                "Action": [
                    "dataworks:CreateRealTimeProcess",
                    "dataworks:QueryRealTimeProcessStatus"
                ],
                "Resource": "*",
                "Effect": "Allow"
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


## Considerations

If you delete a service-linked role that is automatically created by the system, the dependent feature can no longer be used due to insufficient permissions. Exercise caution when you delete a service-linked role. For more information about how to create the service-linked role again and grant permissions to it, see [Create a RAM role for a trusted Alibaba Cloud service](/intl.en-US/RAM Role Management/Create a RAM role/Create a RAM role for a trusted Alibaba Cloud service.md) and [Grant permissions to a RAM role](/intl.en-US/RAM Role Management/Grant permissions to a RAM role.md).

## FAQ

-   Why is the AliyunServiceRoleForAlikafka role for Message Queue for Apache Kafka not automatically created for my RAM user?

    If the service-linked role is created for your Alibaba Cloud account, your RAM user inherits the service-linked role of your Alibaba Cloud account. If your RAM user fails to inherit the service-linked role, log on to the [RAM console](https://ram.console.aliyun.com/), create the following custom policy, and then attach the custom policy to the RAM user:

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
                    "ram:ServiceName": "alikafka.aliyuncs.com"
                    }
                }
            }
        ],
        "Version": "1"
    }
    ```

-   Why is the AliyunServiceRoleForAlikafkaConnector role for Message Queue for Apache Kafka not automatically created for my RAM user?

    If the service-linked role is created for your Alibaba Cloud account, your RAM user inherits the service-linked role of your Alibaba Cloud account. If your RAM user fails to inherit the service-linked role, log on to the [RAM console](https://ram.console.aliyun.com/), create the following custom policy, and then attach the custom policy to the RAM user:

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

    If the service-linked role is created for your Alibaba Cloud account, your RAM user inherits the service-linked role of your Alibaba Cloud account. If your RAM user fails to inherit the service-linked role, log on to the [RAM console](https://ram.console.aliyun.com/), create the following custom policy, and then attach the custom policy to the RAM user:

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

