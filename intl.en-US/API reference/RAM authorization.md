# RAM authorization

Before you use a Resource Access Management \(RAM\) user to call Message Queue for Apache Kafka API operations, you must use an Alibaba Cloud account to create an authorization policy and then assign permissions to the RAM user. In the policy, you can specify the authorized API operations in the Action element and authorized resources in the Resource element. Each resource is indicated by its Alibaba Cloud Resource Name \(ARN\).

## Types of Message Queue for Apache Kafka resources that can be accessed by RAM users

The following table lists the ARNs of Message Queue for Apache Kafka resources that can be accessed by RAM users.

|Resource type|ARN|
|-------------|---|
|Instance|acs:alikafka:\*:\*:instanceid|

In the ARN, `instanceid` indicates the ID of the resource, and `*` indicates all of the corresponding resources.

## Message Queue for Apache Kafka API operations that can be accessed by RAM users

The following table lists the Message Queue for Apache Kafka API operations that can be accessed by RAM users and the ARNs of these API operations.

|API|ARN|
|---|---|
|GetInstanceList|acs:alikafka:\*:\*:instanceid|
|StartInstance|acs:alikafka:\*:\*:instanceid|
|ConvertPostPayOrder|acs:alikafka:\*:\*:instanceid|
|ModifyInstanceName|acs:alikafka:\*:\*:instanceid|
|ListTopic|acs:alikafka:\*:\*:instanceid|
|CreatePrePayOrder|acs:alikafka:\*:\*:instanceid|
|DeleteInstance|acs:alikafka:\*:\*:instanceid|
|UpgradePrePayOrder|acs:alikafka:\*:\*:instanceid|
|CreateTopic|acs:alikafka:\*:\*:instanceid|
|GetTopicList|acs:alikafka:\*:\*:instanceid|
|DeleteTopic|acs:alikafka:\*:\*:instanceid|
|GetTopicStatus|acs:alikafka:\*:\*:instanceid|
|CreateConsumerGroup|acs:alikafka:\*:\*:instanceid|
|DeleteConsumerGroup|acs:alikafka:\*:\*:instanceid|
|GetConsumerList|acs:alikafka:\*:\*:instanceid|
|GetConsumerProgress|acs:alikafka:\*:\*:instanceid|
|ListTagResources|acs:alikafka:\*:\*:instanceid|
|TagResources|acs:alikafka:\*:\*:instanceid|
|UntagResources|acs:alikafka:\*:\*:instanceid|

