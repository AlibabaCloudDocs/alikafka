# Create a MySQL source connector

This topic describes how to create a MySQL source connector to synchronize data from ApsaraDB RDS for MySQL to topics in your Message Queue for Apache Kafka instance by using DataWorks.

Before you export data, make sure that the following operations are completed:

-   The connector feature is enabled for the Message Queue for Apache Kafka instance. For more information, see [Enable the connector feature](/intl.en-US/User guide/Connectors/Enable the connector feature.md).

    **Note:** Your Message Queue for Apache Kafka instance is deployed in the China \(Shenzhen\), China \(Chengdu\), China \(Beijing\), China \(Zhangjiakou\), China \(Hangzhou\), or China \(Shanghai\) region.

-   An ApsaraDB RDS for MySQL instance is created. For more information, see [Create an ApsaraDB RDS for MySQL instance](/intl.en-US/RDS MySQL Database/Quick start/Create an ApsaraDB RDS for MySQL instance.md).
-   Databases and accounts are created in the ApsaraDB RDS for MySQL instance. For more information, see [Create accounts and databases for an ApsaraDB RDS for MySQL instance](/intl.en-US/RDS MySQL Database/Quick start/Create accounts and databases for an ApsaraDB RDS for MySQL instance.md).
-   Tables are created in the databases. For information about the SQL statements that are frequently used in ApsaraDB RDS for MySQL, see [Commonly used SQL statements for MySQL](/intl.en-US/RDS MySQL Database/Appendixes/Commonly used SQL statements for MySQL.md).
-   DataWorks is authorized to access your elastic network interfaces \(ENIs\) no matter whether you use an Alibaba Cloud account or a Resource Access Management \(RAM\) user. To grant permissions, go to the [Cloud Resource Access Authorization](https://ram.console.aliyun.com/role/authorization?request=%7B%22Services%22%3A%5B%7B%22Service%22%3A%22DataWorks%22%2C%22Roles%22%3A%5B%7B%22RoleName%22%3A%22AliyunDataWorksAccessingENIRole%22%2C%22TemplateId%22%3A%22ENIRole%22%7D%5D%7D%5D%2C%22ReturnUrl%22%3A%22https%253A%252F%252Fkafka.console.aliyun.com%22%7D) page.

    **Note:** If you are a RAM user, make sure that you are granted the following permissions:

    -   AliyunDataWorksFullAccess: the permission to manage all the resources within the Alibaba Cloud account in DataWorks.
    -   AliyunBSSOrderAccess: the permission to purchase an Alibaba Cloud service.
    For information about how to attach policies to RAM users, see [t1848773.md\#section\_hxj\_k93\_hbr](/intl.en-US/Access control/Grant permissions to RAM users.md).

-   Both the data source and the data destination are created by you. The data source is an ApsaraDB RDS for MySQL instance. The data destination is your Message Queue for Apache Kafka instance.
-   The Classless Inter-Domain Routing \(CIDR\) block of the virtual private cloud \(VPC\) where the ApsaraDB RDS for MySQL instance is located and the CIDR block of the VPC where the Message Queue for Apache Kafka instance is located do not overlap. If the CIDR blocks overlap, a data synchronization task cannot be created in Message Queue for Apache Kafka.

You can create a data synchronization task in the [Message Queue for Apache Kafka console](https://kafka.console.aliyun.com/) to synchronize data from tables in your ApsaraDB RDS for MySQL instance to topics in your Message Queue for Apache Kafka instance. The implementation of the data synchronization task also depends on DataWorks, as shown in the following figure.

![mysql_connector](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/1951537161/p245245.png)

After a data synchronization task is created in the Message Queue for Apache Kafka console, DataWorks Basic Edition is automatically activated for free. DataWorks Basic Edition allows you to create DataWorks workspaces for free and create exclusive resource groups for Data Integration that **charge fees**. The specifications of an exclusive resource group for Data Integration are 4 vCPUs and 8 GB memory. The resource groups are available for monthly subscriptions. By default, an exclusive resource group for Data Integration is automatically renewed upon expiration. For more information about the billing of DataWorks, see [Overview]().

In addition, DataWorks automatically generates destination topics in Message Queue for Apache Kafka based on the configurations of your data synchronization task. Source tables and destination topics have one-to-one mappings. By default, DataWorks creates a topic with six partitions for each table with a primary key and a topic with one partition for each table without a primary key. Make sure that after DataWorks creates the topics and partitions, the total numbers of topics and partitions in your Message Queue for Apache Kafka instance will not exceed the limits. Otherwise, the task fails and an error is thrown because topics fail to be created.

The name of each topic must be in the `Specified prefix_Name of the corresponding source table` format. The underscore \(\_\) is automatically added by the system. The following figure provides an example.

![table_topic_match](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/1951537161/p245247.png)

In this example, the specified prefix is mysql. The source tables to be synchronized are table\_1, table\_2, ..., and table\_n. DataWorks automatically creates topics for you to receive the data that is synchronized from the source tables. The topics are named mysql\_table\_1, mysql\_table\_2, ..., and mysql\_table\_n.

## Considerations

-   Notes for regions
    -   Your data source and Message Queue for Apache Kafka instance may not be in the same region. In this case, make sure that you have a Cloud Enterprise Network \(CEN\) instance within your Alibaba Cloud account. Make sure that the VPCs of the data source and the Message Queue for Apache Kafka instance are attached to the CEN instance. In addition, make sure that the configurations of traffic and bandwidth are complete and that end-to-end connectivity is available.

        Otherwise, a CEN instance may be automatically created. In this case, the VPCs of your destination Message Queue for Apache Kafka instance and Elastic Compute Service \(ECS\) instances of your exclusive resource group are attached to the CEN instance to ensure end-to-end connectivity. However, the bandwidth of the automatically created CEN instance is extremely low because the bandwidth is not manually configured. Therefore, a network access error may occur when you create a data synchronization task or when the data synchronization task is running.

    -   Assume that your data source and Message Queue for Apache Kafka instance are in the same region. When you create a data synchronization task, an ENI is automatically created in the VPC of the data source or Message Queue for Apache Kafka instance. The ENI is also automatically bound to the ECS instances of your exclusive resource group to ensure end-to-end connectivity.
-   Notes for exclusive resource groups in DataWorks
    -   DataWorks allows you to use each exclusive resource group to run up to three data synchronization tasks. Assume that DataWorks finds an existing resource group has been used to run less than three data synchronization tasks when you create a data synchronization task. DataWorks uses this resource group to run the newly created data synchronization task.
    -   Each exclusive resource group in DataWorks can be bound to the ENIs of up to two VPCs. The ENI bound to the existing resource group that DataWorks finds and the ENI that needs to be bound may have overlapping CIDR blocks. This, as well as other technical issues, causes DataWorks to fail to create a data synchronization task by using the existing resource group. In this case, even if the existing resource group has been used to run less than three data synchronization tasks, DataWorks still creates a resource group to ensure that a data synchronization task can be created.

## Create and deploy a MySQL source connector

1.  Log on to the [Message Queue for Apache Kafka console](https://kafka.console.aliyun.com/?spm=a2c4g.11186623.2.22.6bf72638IfKzDm).

2.  In the top navigation bar, select the region where your instance is located.

3.  In the left-side navigation pane, click **Instances**.

4.  On the **Instances** page, click the name of the instance that you want to manage.

5.  In the left-side navigation pane, click **Connector \(Public Preview\)**.

6.  On the **Connector \(Public Preview\)** page, click **Create Connector**.

7.  In the **Create Connector** wizard, perform the following steps:

    1.  In the **Enter Basic Information** step, enter a connector name in the **Connector Name** field, set **Dump Path** to **ApsaraDB RDS for MySQL** Dump To **Message Queue for Apache Kafka**, and then click **Next**.

        **Note:** By default, the **Authorize to Create Service Linked Role** check box is selected. This means that Message Queue for Apache Kafka will create a service-lined role based on the following rules:

        -   If no service-linked role is created, Message Queue for Apache Kafka automatically creates a service-linked role for you to use the MySQL source connector to synchronize data from ApsaraDB RDS for MySQL to Message Queue for Apache Kafka.
        -   If you have created a service-linked role, Message Queue for Apache Kafka does not create it again.
        For more information about service-linked roles, see [Service-linked roles](/intl.en-US/Access control/Service-linked roles.md).

        |Parameter|Description|Example|
        |---------|-----------|-------|
        |**Connector Name**|The name of the connector. Take note of the following rules when you specify a connector name:        -   The connector name must be 1 to 48 characters in length. It can contain digits, lowercase letters, and hyphens \(-\), but cannot start with a hyphen \(-\).
        -   The connector name must be unique within a Message Queue for Apache Kafka instance.
The data synchronization task of the connector must use a consumer group that is named in the connect-Task name format. If you have not manually created such a consumer group, the system automatically creates a consumer group for you.

|kafka-source-mysql|
        |**Dump Path**|The source and destination of data transfer. Select a data source from the first drop-down list and a destination from the second drop-down list.|**ApsaraDB RDS for MySQL** Dump To **Message Queue for Apache Kafka**|

    2.  In the **Configure Source Instance** step, set the parameters that are described in the following table and click **Next**.

        |Parameter|Description|Example|
        |---------|-----------|-------|
        |**ApsaraDB RDS for MySQL Instance ID**|The ID of the ApsaraDB RDS for MySQL instance from which data is to be synchronized.|rm-wz91w3vk6owmz\*\*\*\*|
        |**Region of ApsaraDB RDS for MySQL Instance**|The ID of the region where the ApsaraDB RDS for MySQL instance is located. Select an ID from the drop-down list.|cn-shenzhen|
        |**ApsaraDB RDS for MySQL Database**|The name of the database from which data is to be synchronized in the ApsaraDB RDS for MySQL instance.|mysql-to-kafka|
        |**ApsaraDB RDS for MySQL Database Account**|The account that you can use to connect to the ApsaraDB RDS for MySQL database.|mysql\_to\_kafka|
        |**Account and Password of ApsaraDB RDS for MySQL Instance**|The password that you can use to connect to the ApsaraDB RDS for MySQL database.|N/A|
        |**ApsaraDB RDS for MySQL Database Table**|The name of the table from which data is to be synchronized in the ApsaraDB RDS for MySQL database. Separate multiple table names with commas \(,\). Source tables and destination topics have one-to-one mappings.

|mysql\_tbl|
        |**Name Prefix**|The prefix used to name topics that are to be created in Message Queue for Apache Kafka. Each topic name consists of the prefix and the name of the corresponding source table in the ApsaraDB RDS for MySQL database. Make sure that the prefix is globally unique.|mysql|

    3.  The **Configure Destination Instance** step displays the destination Message Queue for Apache Kafka instance to which the data is to be synchronized. Confirm the information and click **Next**.

    4.  In the **Preview/Submit** step, confirm the configurations of the connector and click **Submit**.

8.  In the **Create Connector** panel, click **Deploy**.

    On the **Connector \(Public Preview\)** page, you can find the created data synchronization task and the status of the task is **Running**.

    **Note:** If the task fails to be created, check whether all the prerequisites that are described in this topic are met.


## Verify the results

1.  Insert data into a data source table in the ApsaraDB RDS for MySQL database.

    The following sample code provides an example:

    ```
    INSERT INTO mysql_tbl
        (mysql_title, mysql_author, submission_date)
        VALUES
        ("mysql2kafka", "tester", NOW())
    ```

    For information about the SQL statements that are frequently used in ApsaraDB RDS for MySQL, see [Commonly used SQL statements for MySQL](/intl.en-US/RDS MySQL Database/Appendixes/Commonly used SQL statements for MySQL.md).

2.  Use the message query feature of Message Queue for Apache Kafka to verify whether the data of the table in the ApsaraDB RDS for MySQL database can be synchronized to a topic in your Message Queue for Apache Kafka instance.

    For more information, see [Query messages](/intl.en-US/User guide/Query messages.md).

    The following sample code provides an example of the data that is synchronized from a table in ApsaraDB RDS for MySQL to a topic in Message Queue for Apache Kafka:

    ```
    {
        "schema":{
            "dataColumn":[
                {
                    "name":"mysql_id",
                    "type":"LONG"
                },
                {
                    "name":"mysql_title",
                    "type":"STRING"
                },
                {
                    "name":"mysql_author",
                    "type":"STRING"
                },
                {
                    "name":"submission_date",
                    "type":"DATE"
                }
            ],
            "primaryKey":[
                "mysql_id"
            ],
            "source":{
                "dbType":"MySQL",
                "dbName":"mysql_to_kafka",
                "tableName":"mysql_tbl"
            }
        },
        "payload":{
            "before":null,
            "after":{
                "dataColumn":{
                    "mysql_title":"mysql2kafka",
                    "mysql_author":"tester",
                    "submission_date":1614700800000
                }
            },
            "sequenceId":"1614748790461000000",
            "timestamp":{
                "eventTime":1614748870000,
                "systemTime":1614748870925,
                "checkpointTime":1614748870000
            },
            "op":"INSERT",
            "ddl":null
        },
        "version":"0.0.1"
    }
    ```


