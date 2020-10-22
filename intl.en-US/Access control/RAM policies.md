# RAM policies

Alibaba Cloud offers Resource Access Management \(RAM\), which allows you to manage permissions for the Message Queue for Apache Kafka console and API. RAM allows you to avoid sharing the AccessKey pair, which includes an AccessKey ID and an AccessKey secret, of your Alibaba Cloud account with other users. Instead, you can grant users only the minimum required permissions.

## RAM policies

In RAM, policies are a set of permissions that are described based on the policy structure and syntax. You can use policies to describe the authorized resource sets, authorized operation sets, and authorization conditions. For more information, see [Policy structure and syntax](/intl.en-US/Policy Management/Policy language/Policy structure and syntax.md).

In RAM, a policy is a resource entity. Message Queue for Apache Kafka supports the following types of policies:

-   System policies: System policies are created and updated by Alibaba Cloud and you cannot modify them. These policies are applicable to coarse-grained control of RAM user permissions.
-   Custom policies: You can create, update, and delete custom policies and maintain policy versions. These policies are applicable to fine-grained control of RAM user permissions.

## System policies

The following table lists the system policies supported by Message Queue for Apache Kafka.

|Policy|Description|
|------|-----------|
|AliyunKafkaFullAccess|The management permission of Message Queue for Apache Kafka. The RAM user who has been granted this permission has the permission equivalent to the Alibaba Cloud account, that is, all operation permissions of the console and API.|
|AliyunKafkaReadOnlyAccess|The read-only permission of Message Queue for Apache Kafka. The RAM user who has been granted this permission has only the read-only permission of all resources of the Alibaba Cloud account, and does not have the operation permissions of the console and API.|

## Examples of system policies

Use the system policy AliyunKafkaFullAccess as an example. The RAM user who has been granted this permission has the permission equivalent to the Alibaba Cloud account, that is, all operation permissions of the console and API. The following code displays the policy content:

```
{
    "Version": "1",
    "Statement": [
        {
            "Action": "alikafka:*",
            "Resource": "*",
            "Effect": "Allow"
        }
    ]
}
```

## Custom policies

The following table lists the custom policies supported by Message Queue for Apache Kafka.

|Action|Permission description|Read-only or not|
|------|----------------------|----------------|
|ReadOnly|Only reads all resources.|Yes|
|ListInstance|Views instances.|Yes|
|StartInstance|Deploys instances.|No|
|UpdateInstance|Changes instance configuration.|No|
|ReleaseInstance|Releases instances.|No|
|ListTopic|Views topics.|Yes|
|CreateTopic|Creates topics.|No|
|UpdateTopic|Changes topic configuration.|No|
|DeleteTopic|Deletes topics.|No|
|ListGroup|Views consumer groups.|Yes|
|CreateGroup|Creates consumer groups.|No|
|UpdateGroup|Changes consumer group configuration.|No|
|DeleteGroup|Deletes consumer groups.|No|
|QueryMessage|Queries messages.|Yes|
|SendMessage|Sends messages.|No|
|DownloadMessage|Downloads messages.|Yes|
|CreateDeployment|Creates connector tasks.|No|
|DeleteDeployment|Deletes connector tasks.|No|
|ListDeployments|Views connector tasks.|Yes|
|UpdateDeploymentRemark|Updates connector task description.|No|
|GetDeploymentLog|Obtains the operational logs of connector tasks.|Yes|
|EnableAcl|Enables the access control list \(ACL\) feature.|No|
|CreateAcl|Creates an ACL.|No|
|DeleteAcl|Deletes an ACL.|No|
|ListAcl|Queries ACLs.|Yes|
|CreateSaslUser|Creates a Simple Authentication and Security Layer \(SASL\) user.|No|
|DeleteSaslUser|Deletes an SASL user.|No|
|ListSaslUser|Queries SASL users.|Yes|

## Examples of custom policies

Use the custom policy AliyunKafkaCustomAccess as an example. The RAM user who has been granted this permission only has the permissions to view the alikafka\_post-cn-xxx instance, view topics, view consumer groups, query messages, and download messages in the console and by using API operations. The following code displays the policy content:

```
{
    "Version": "1",
    "Statement": [
        {
            "Action": [
              "alikafka:ListInstance",
              "alikafka:ListTopic",
              "alikafka:ListGroup",
              "alikafka:QueryMessage",
              "alikafka:DownloadMessage"
                       ],
            "Resource": "acs:alikafka:*:*:alikafka_post-cn-xxx",
            "Effect": "Allow"
        }
    ]
}
```

