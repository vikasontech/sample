---
title: "Dynamodb"
date: 2020-08-08T20:54:02+07:00
---
# What Is Amazon DynamoDB?

Amazon DynamoDB is a fully managed NoSQL database service that provides fast and predictable performance with seamless scalability. DynamoDB lets you offload the administrative burdens of operating and scaling a distributed database so that you don't have to worry about hardware provisioning, setup and configuration, replication, software patching, or cluster scaling. 
DynamoDB also offers encryption at rest, which eliminates the operational burden and complexity involved in protecting sensitive data. 
DynamoDB provides on-demand backup capability. It allows you to create full backups of your tables for long-term retention and archival for regulatory compliance needs. 
You can create on-demand backups and enable point-in-time recovery for your Amazon DynamoDB tables. Point-in-time recovery helps protect your tables from accidental write or delete operations. With point-in-time recovery, you can restore that table to any point in time during the last 35 days

# High Availability and Durability

DynamoDB automatically spreads the data and traffic for your tables over a sufficient number of servers to handle your throughput and storage requirements, while maintaining consistent and fast performance.

You can use global tables to keep DynamoDB tables in sync across AWS Regions. 

# Primary Key

DynamoDB supports two different kinds of primary keys:
* Partition key – A simple primary key, composed of one attribute known as the partition key.
DynamoDB uses the partition key's value as input to an internal hash function. The output from the hash function determines the partition (physical storage internal to DynamoDB) in which the item will be stored.
In a table that has only a partition key, no two items can have the same partition key value.
* Partition key and sort key – Referred to as a composite primary key, this type of key is composed of two attributes
The partition key of an item is also known as its hash attribute. The term hash attribute derives from the use of an internal hash function in DynamoDB that evenly distributes data items across partitions, based on their partition key values.

The sort key of an item is also known as its range attribute. The term range attribute derives from the way DynamoDB stores items with the same partition key physically close together, in sorted order by the sort key value.

# Secondary Indexes

DynamoDB supports two kinds of indexes:
**Global secondary index** – An index with a partition key and sort key that can be different from those on the table.
**Local secondary index** – An index that has the same partition key as the table, but a different sort key.

Each table in DynamoDB has a quota of 20 global secondary indexes (default quota) and 5 local secondary indexes per table.

# DynamoDB Streams

DynamoDB Streams is an optional feature that **captures data modification events in DynamoDB tables**. The data about these events appear in the stream in near-real time, and in the order that the events occurred.
Each event is represented by a **stream record**. If you enable a stream on a table, DynamoDB Streams writes a stream record whenever one of the following events occurs:
**A new item is added to the table**: The stream captures an image of the entire item, including all of its attributes.
**An item is updated**: The stream captures the "before" and "after" image of any attributes that were modified in the item.
**An item is deleted from the table**: The stream captures an image of the entire item before it was deleted.
Each stream record also contains the name of the table, the event timestamp, and other metadata. Stream records have a lifetime of 24 hours; after that, they are automatically removed from the stream.
You can use DynamoDB Streams together with AWS Lambda to create a trigger—code that executes automatically whenever an event of interest appears in a stream. 
In addition to triggers, DynamoDB Streams enables powerful solutions such as data replication within and across AWS Regions, materialized views of data in DynamoDB tables, data analysis using Kinesis materialized views, and much more.

# Read Consistency

DynamoDB supports eventually consistent and strongly consistent reads.

## Eventually consistent 

When you read data from a DynamoDB table, the response might not reflect the results of a recently completed write operation. The response might include some stale data. If you repeat your read request after a short time, the response should return the latest data.

## Strongly Consistent Reads

When you request a strongly consistent read, DynamoDB returns a response with the most up-to-date data, reflecting the updates from all prior write operations that were successful. However, this consistency comes with some disadvantages:

* A strongly consistent read might not be available if there is a network delay or outage. In this case, DynamoDB may return a server error (HTTP 500).
* Strongly consistent reads may have higher latency than eventually consistent reads.
* Strongly consistent reads are not supported on global secondary indexes.
* Strongly consistent reads use more throughput capacity than eventually consistent reads. For details, see


# Read/Write Capacity Mode

Amazon DynamoDB has two read/write capacity modes for processing reads and writes on your tables:
* On-demand
* Provisioned (default, free-tier eligible)
The read/write capacity mode controls how you are charged for read and write throughput and how you manage capacity. You can set the read/write capacity mode when creating a table or you can change it later.


# On-demand

Amazon DynamoDB on-demand is a flexible billing option capable of serving thousands of requests per second without capacity planning. DynamoDB on-demand offers pay-per-request pricing for read and write requests so that you pay only for what you use.

On-demand mode is a good option if any of the following are true:

* You create new tables with unknown workloads.
* You have unpredictable application traffic.
* You prefer the ease of paying for only what you use.

# Provisioned Mode

You specify the number of reads and writes per second that you require for your application. You can use auto scaling to adjust your table’s provisioned capacity automatically in response to traffic changes.
This helps you govern your DynamoDB use to stay at or below a defined request rate in order to obtain cost predictability.
Provisioned mode is a good option if any of the following are true:
* You have predictable application traffic.
* You run applications whose traffic is consistent or ramps gradually.
* You can forecast capacity requirements to control costs.


# DynamoDB Auto Scaling

If you use the AWS Management Console to create a table or a global secondary index, DynamoDB auto scaling is enabled by default.
You can manage auto scaling settings at any time by using the console, the AWS CLI, or one of the AWS SDKs.
DynamoDB auto scaling actively manages throughput capacity for tables and global secondary indexes. With auto scaling, you define a range (upper and lower limits) for read and write capacity units. You also define a target utilization percentage within that range. DynamoDB auto scaling seeks to maintain your target utilization, even as your application workload increases or decreases.

# Reserved Capacity

Reserved capacity is not available in on-demand mode.
You can purchase reserved capacity in advance, as described at Amazon DynamoDB Pricing. With reserved capacity, you pay a one-time upfront fee and commit to a minimum provisioned usage level over a period of time. Your reserved capacity is billed at the hourly reserved capacity rate. By reserving your read and write capacity units ahead of time, you realize significant cost savings compared to on-demand provisioned throughput settings.

# Partitions and Data Distribution

Amazon DynamoDB stores data in partitions. A partition is an allocation of storage for a table, backed by solid state drives (SSDs) and automatically replicated across multiple Availability Zones within an AWS Region. Partition management is handled entirely by DynamoDB—you never have to manage partitions yourself.
DynamoDB allocates additional partitions to a table in the following situations:
If you increase the table's provisioned throughput settings beyond what the existing partitions can support.
If an existing partition fills to capacity and more storage space is required.
DynamoDB is optimized for uniform distribution of items across a table's partitions, no matter how many partitions there may be. We recommend that you choose a partition key that can have a large number of distinct values relative to the number of items in the table.


# Global Tables: Multi-Region Replication

Amazon DynamoDB global tables provide a fully managed solution for deploying a multiregion, multi-master database, without having to build and maintain your own replication solution. With global tables you can specify the AWS Regions where you want the table to be available. DynamoDB performs all of the necessary tasks to create identical tables in these Regions and propagate ongoing data changes to all of them.
DynamoDB global tables are ideal for massively scaled applications with globally dispersed users. In such an environment, users expect very fast application performance. Global tables provide automatic multi-master replication to AWS Regions worldwide. They enable you to deliver low-latency data access to your users no matter where they are located.

# In-Memory Acceleration with DynamoDB Accelerator (DAX)

DAX is a DynamoDB-compatible caching service that enables you to benefit from fast in-memory performance for demanding applications. DAX addresses three core scenarios:
* As an in-memory cache, DAX reduces the response times 
* DAX reduces operational and application complexity 
* For read-heavy or bursty workloads, DAX provides increased throughput 

DAX supports server-side encryption. With encryption at rest, the data persisted by DAX on disk will be encrypted. DAX writes data to disk as part of propagating changes from the primary node to read replicas.

# Monitoring Tools(DAX)

## Automated Monitoring Tools
* Amazon CloudWatch Alarms – Watch a single metric over a time period that you specify, and perform one or more actions based on the value of the metric relative to a given threshold over a number of time periods.
* Amazon CloudWatch Logs – Monitor, store, and access your log files from AWS CloudTrail or other sources. 
* Amazon CloudWatch Events – Match events and route them to one or more target functions or streams to make changes, capture state information, and take corrective action.
* AWS CloudTrail Log Monitoring – Share log files between accounts, monitor CloudTrail log files in real time by sending them to CloudWatch Logs

# DAX Encryption at Rest

With encryption at rest, the data persisted by DAX on disk is encrypted using 256-bit Advanced Encryption Standard, also known as AES-256 encryption.
DAX encryption at rest automatically integrates with AWS Key Management Service (AWS KMS) for managing the single service default key that is used to encrypt your clusters. If a service default key doesn't exist when you create your encrypted DAX cluster, AWS KMS automatically creates a new key for you. This key is used with encrypted clusters that are created in the future. AWS KMS combines secure, highly available hardware and software to provide a key management system scaled for the cloud.
DAX does not call AWS KMS for every single DAX operation. DAX only uses the key at the cluster launch. Even if access is revoked, DAX can still access the data until the cluster is shut down. Customer-specified AWS KMS keys are not supported.

# Security Best Practices

Encryption at rest
* Use IAM roles to authenticate access to DynamoDB
* Use IAM policies for DynamoDB base authorization
* Use IAM policy conditions for fine-grained access control
* Use a VPC endpoint and policies to access DynamoDB - If you only require access to DynamoDB from within a virtual private cloud (VPC), you should use a VPC endpoint to limit access from only the required VPC. Doing this prevents that traffic from traversing the open internet and being subject to that environment.
* Consider client-side encryption


# Detective Security Best Practices

* Use AWS CloudTrail to monitor AWS managed KMS key usage
* Use CloudTrail to monitor DynamoDB control-plane operations
* Consider using DynamoDB Streams to monitor modify/update data-plane operations
* Monitor DynamoDB configuration with AWS Config
* Monitor DynamoDB compliance with AWS Config rules
* Tag your DynamoDB resources for identification and automation

