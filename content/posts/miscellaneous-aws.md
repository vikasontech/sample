---
title: "Miscellaneous Aws"
date: 2020-08-20T08:35:33+07:00
draft: true
---

## Migration Service

**AWS DataSync**, users can create a task to specify the data source and destination and then configure the data transfer.

**Server Migration Service** is used to migrate on-premise servers such as VMware.

**Database Migration Service** is used to migrate a database instead of local storage.

**Direct Connect** sets up a dedicated network connection to AWS. For higher throughput, a [**LAG(Link aggregate group)**](https://docs.aws.amazon.com/directconnect/latest/UserGuide/lags.html) can be used to aggregate multiple DX connections to give a maximum of **40 Gig bandwidth**. A **[VPN connection](https://aws.amazon.com/premiumsupport/knowledge-center/create-vpn-direct-connect/) over this connection can provide secure connections between on-premise devices to AWS VGW**.

## Amazon S3 

* Amazon S3 can be used to **store any amount of data which can be accessed from multiple locations concurrently without any impact on latency.**
* For **web sites with dynamic user interactions, using Amazon EFS is an ideal option to use along with using Amazon S3** for static non changing data.
* **Amazon EBS** can be used for **dynamic content only in case a web server is hosted on an EC2 instance**.
* Amazon S3 **cannot be used for dynamic contents**, Amazon EC2 or EFS can be used.

### Query S3 data using S3 select 

Amazon S3 Select can be used to **query a subset of data from the objects stored** in S3 bucket using **simple SQL**. For using this, objects need to be stored in S3 bucket with **CSV, JSON or Apache Parquet format. GZIP & BZIP2 compression is supported with CSV or JSON format with server-side encryption**.

Each object in an S3 bucket can have a **user-defined storage class**.

### Ensure that all objects are encrypted at rest in the bucket

* Ensure that the **default encryption is enabled** for the S3 bucket
* Ensure to **change the configuration of the bucket to use a KMS key** to encrypt the objects
Amazon S3 default encryption provides a way to set the default encryption behavior for an S3 bucket. You can set default encryption on a bucket so that **all new objects are encrypted** when they are stored in the bucket. The objects are encrypted using **server-side encryption with either Amazon S3-managed keys (SSE-S3) or customer master keys (CMKs) stored in AWS Key Management Service (AWS KMS).**

### Removing incomplete uploads using lifecycle hooks

Amazon S3 Lifecycle rules can be configured to abort all multipart uploads which are failing to complete in a specific time period. For all files from size 5 MB to 5GB, multipart upload can be used. 

### S3 Cross-region replication 

S3 Cross-region replication is a feature that enables an automatic and asynchronous copy of user data from one destination bucket to another destination bucket located in one of the other AWS regions.

### S3 Migration 

#### S3 glacier 
Data retrival from s3 glacier with in 3-5 hrs.

#### Vault Lock
This feature of Amazon Glacier allows you to **lock your vault with a variety of compliance controls that are designed to support such long-term records retention**.

#### Expedited retrieval

It allows you to **quickly access your data when occasional urgent requests are required for a subset of archives**. The data is available within **1 - 5 minutes**. 

#### Bulk retrieval

They are the **lowest-cost retrieval option**, enabling you to **retrieve large amounts of data within 5 - 12 hours**. 

#### Standard retrievals

Standard retrievals are a **low-cost** way to access your data **within just a few hours**. For example, you can use Standard retrievals to restore backup data, retrieve archived media content for same-day editing or distribution, or pull and analyze logs to drive business decisions within hours.

[more details](https://aws.amazon.com/glacier/faqs/#dataretrievals)

### Automatically trigger pipeline with changes in the source S3 bucket,

* To automatically trigger pipeline with changes in the source S3 bucket, Amazon CloudWatch Events rule & AWS CloudTrail trail must be applied. **When there is a change in the S3 bucket, events are filtered using AWS CloudTrail & then Amazon CloudWatch events are used to trigger the start of the pipeline.** 
* This default method is faster. **Periodic checks should be disabled to have events-based triggering of CodePipeline.**
* Webhooks are used to trigger pipeline when the source is GitHub repository.
* Periodic checks are not a faster way to trigger CodePipeline.

### Ensure that all objects are encrypted at rest in the bucket

* Ensure that the default encryption is enabled for the S3 bucket
* Ensure to change the configuration of the bucket to use a KMS key to encrypt the objects
* Amazon S3 default encryption provides a way to set the default encryption behavior for an S3 bucket. You can set default encryption on a bucket so that all new objects are encrypted when they are stored in the bucket. 
* The objects are encrypted using server-side encryption with either Amazon S3-managed keys (SSE-S3) or customer master keys (CMKs) stored in AWS Key Management Service (AWS KMS).i**

## Amazon SQS

Amazon Simple Queue Service (SQS) is a **fully managed message queuing service** that enables you to **decouple and scale microservices, distributed systems, and serverless applications**. SQS eliminates the complexity and overhead associated with managing and operating message-oriented middleware and empowers developers to focus on differentiating work. **Using SQS, you can send, store, and receive messages between software components at any volume, without losing messages or requiring other services to be available**.

**ApproximateNumberOfMessagesVisible** describes the **number of messages available for retrieval**. It can be used to decide the queue length.

### Amazon SQS dead-letter queues

Amazon SQS supports dead-letter queues, which other queues (source queues) can target for messages that can't be processed (consumed) successfully. Dead-letter queues are useful for debugging your application or messaging system because they let you isolate problematic messages to determine why their processing doesn't succeed.
SQS dead-letter queue should be selected as it is designed to isolate problematic messages in the queue.

### SQS visibility timeout 

**Default visibility timeout is 30 seconds**. This will hide the message from other consumers for 30 seconds, so they will not process the same message which is in the process by the original consumer.

## Amazon Polly

### Lexicons 

**Lexicons are specific to a region**. You will need to **upload Lexicon in each region** where you need to use it. For a **single text which appears multiple times in the content, you can create an alias** using multiple Lexicons to have different speech.
**If a single word is repeating multiple times in content & needs to have different speech, we need to have multiple Lexicons created.**

### SSML tags

Using **SSML tags**, we can control the speech generated by Amazon Polly.

**Using SSML tags, convert commas to period & tag words and paragraphs as "Strong", will help to control the speech speed, adds appropriate pause & emphasis on appropriate words slowing speaking rate.**

## Amazon Athena 

Amazon Athena is a suitable **tool for querying Network Load Balancers logs**. If large amount of logs are saved in S3 buckets from Network load balancer, **Amazon Athena can be used to query logs and generate required details of client IP address and TLS handshake time.**


## CloudFront 

Amazon CloudFront is a web service that **speeds up the distribution of static and dynamic web content, such as .html, .css, .js, image files, video fiels to your users**. CloudFront delivers your content through a worldwide network of data centers called **edge locations**.

Amazon CloudFront Can be used to offload origin server loads. In the above case, for videos which are saved in on-premise servers & Amazon S3 bucket, Amazon CloudFront can be used to deliver this content to users with less latency & also will offload load on servers.

Since, EBS would be preferred for rapidly changing web content, using **Amazon S3 transfer Acceleration would be costly as compared to using Amazon CloudFront**. Also, for **files less than 1 Gb, using Amazon CloudFront would provide better performance than Amazon S3 Transfer Acceleration.**

**Amazon CloudFront is a global service** for which events are delivered to **CloudTrail** trails which include global services. **To avoid duplicate Amazon CloudFront events, you can disable these events from delivering to CloudTrail trails in all regions & enable in only one region**.

Changes to Global service event logs can be done **only via AWS CLI & not via AWS console**.

If you run **PCI or HIPAA-compliant workloads** based on the AWS Shared Responsibility Model, we recommend that you log your **CloudFront usage data for the last 365 days for future auditing purposes**. To log usage data, you can do the following:

* Enable CloudFront access logs.
* Capture requests that are sent to the CloudFront API.

### CloudTrail log file integrity

After you **enable** CloudTrail log file integrity, it will create a **hash file called digest file** which refers to logs that are generated. This digest file is saved in a different folder in the S3 bucket where log files are saved. 
Each of these digest files has the **private key of public & private key pair**. The DIgest file can be **validated using the public key**. This feature ensures that all the modifications made to CloudTrail log files are recorded. 

### CloudFront Query  

* **CloudFront Query String Forwarding does not support RTMP distribution**.
* Elimiter Character should be always **'&'**. CloudFront Query String Forwarding **only supports Web distribution**. For query string forwarding, the delimiter character must be always a '&' character. Parameters' names and values used in the query string are case sensitive.

## VPN connection 

**VPN connection is encrypted and high available and have bandwidth up to 1.25 Gbps.**

Using AWS VPN is the fastest & cost-effective way of establishing IPSEC connectivity from on-premise to AWS. 

### VPN tunnel

For each VPN Tunnel, AWS provides two different VPN endpoints. **ECMP (Equal Cost Multi Path)** can be used to carry traffic on both VPN endpoints which can increase performance & faster data transfer.

**ECMP needs to be enabled on Client end devices & not on VGW end.**

## Security

### AWS Multi-Factor Authentication (MFA) 

AWS Multi-Factor Authentication (MFA) is a simple best practice that adds an **extra layer of protection on the top of your user name and password**.

### Context-aware MFA

With **Context-aware MFA**, AWS SSO analyzes user sign-in context such as browser, location, and devices. If any deviation is observed, only then it asks for the additional second level of verification code. With this, a user does not have to perform MFA repeatedly from the same device.

### MFA Delete

**MFA Delete** can be used to add another layer of security to S3 Objects to prevent accidental deletion of objects.

## AWS Logs

### Logs categories - 

* **AWS Infrastructure Logs** - AWS CloudTrail, Amazon VPC Flow Logs
* **AWS Service Logs** - Amazon S3, AWS Elastic Load Balancing, Amazon CloudFront, AWS Lambda, AWS Elastic Beanstalk, etc.,
* **Host Based Logs** - Messages, Security, NGINX/Apache/IIS, Windows Event Logs, Windows Performance Counters, etc.,

### VPC Flow Logs 

VPC Flow Logs is a feature that enables you to capture information about the **IP traffic going to and from network interfaces in your VPC**. Flow log data is stored using **Amazon CloudWatch Logs**. After you've created a flow log, you can view and retrieve its data in Amazon CloudWatch Logs.

### Amazon CloudWatch Logs

You can use Amazon CloudWatch Logs to **monitor, store, and access your log files from Amazon Elastic Compute Cloud (Amazon EC2) instances, AWS CloudTrail, and other sources**.

## AWS Storage Gateway 

### Cached volumes 

**Cached volumes** – You store your data in Amazon Simple Storage Service (Amazon S3) and **retain a copy of frequently accessed data subsets locally**. Cached volumes offer substantial cost savings on primary storage and minimize the need to scale your storage on-premises. You also retain low-latency access to your frequently accessed data.

### Stored volumes
**Stored volumes** – If you need **low-latency access to your entire dataset**, first configure your on-premises gateway to store all your data locally. Then asynchronously back up point-in-time snapshots of this data to Amazon S3.

## IAM roles

IAM roles are designed so that your applications can securely make API requests from your instances, without requiring you to manage the security credentials that the applications use. Instead of creating and distributing your AWS credentials, you can delegate permission to make API requests using IAM roles 

### IAM roles for Amazon ECS tasks

With IAM roles for Amazon ECS tasks, you can specify an IAM role to be used by the containers in a task. Applications are required to sign their AWS API requests with AWS credentials, and this feature provides a strategy to manage credentials for your application's use. This is similar to how Amazon EC2 instance profiles provide credentials to EC2 instances

### Cross-account IAM roles 

Cross-account IAM roles allow customers to **securely grant access to AWS resources in their account to a third party**, like an APN Partner, while retaining the ability to control and audit who is accessing their AWS account. Cross-account roles **reduce the amount of sensitive information APN Partners** need to store for their customers so that they can focus on their product instead of managing keys.

### Cross Account access is safer

A basic analogy of the difference is handing someone an access badge (which could be used by anyone) vs handing someone an access badge that requires that person's fingerprints to successfully use.

## EBS instance

### Change AZ of an EBS instance

In order for a volume to be available in another availability zone, 
* You need to first create a snapshot from the volume. 
* Create a volume from the snapshoti
* You can then specify the new availability zone accordingly.

**Note**: **The EBS Volumes attached to the EC2 Instance will always have to remain in the same availability zone as the EC2 Instance**. A possible reason for this could be because of the fact that EBS Volumes are present outside of the host machine and instances have to be connected over the network, if the EBS Volumes are present outside the Availability Zone there can be potential latency issues and subsequent performance degradation.

## EFS System 

### Encryption in transit in EFS

While mounting Amazon EFS, if encryption of data in transit is enabled, **EFS Mount helper** initializes the client Stunnel process to encrypt data in transit. EFS Mount helper uses TLS 1.2 to encrypts data in transit.

### Amazon S3 vs EFS vs EBS Comparison

| AMAZON S3 | AMAZON EBS | AMAZON EFS |
| --------- | ---------- | ---------- |
| Can be publicly accessible Web interface Object Storage Scalable **Slower than EBS and EFS** | Accessible only via the given EC2 Machine File System interface Block Storage Hardly **scalable Faster than S3 and EFS** | Accessible via several EC2 machines and AWS services Web and file system interface Object storage Scalable **Faster than S3, slower than EBS**
Good for **storing backups** | Is meant to be **EC2 drive** | Good for **shareable applications and workloads** like application and servers
Object storage | Block Storage | Bock Storage

## RDS 

### Amazon RDS Multi-AZ deployments

* **Multi-AZ feature allows high availability across Availability Zones, not regions.**
* Amazon RDS Multi-AZ deployments provide enhanced availability and durability for Database (DB) Instances, making them a natural fit for production database workloads. When you provision a Multi-AZ DB Instance, Amazon RDS **automatically creates a primary DB Instance and synchronously replicates the data to a standby instance in a different Availability Zone (AZ)**.

### RDS-encrypted

RDS-encrypted resources, data is **encrypted at rest**, including the underlying storage for a database (DB) instance, its automated backups, read replicas, and snapshots. This capability uses the open **standard AES-256 encryption algorithm** to encrypt your data, which is transparent to your database engine.

This encryption option protects against **physical exfiltration or access to your data bypassing** the DB instances. It is therefore critical to **complement encrypted resources with an effective encryption key management and database credential management practice to mitigate any unauthorized access**. Otherwise, compromised credentials or insufficiently protected keys might allow unauthorized users to access the plaintext data directly through the database engine.

Encryption key management is provided using the AWS KMS

### Encrypt existing un-encrypted instance 

Existing unencrypted RDS instances and their snapshots cannot be encrypted. Users can only enable encryption for an RDS DB instance when they create it. 

### Automatic RDS backup

Amazon RDS creates a storage volume snapshot of your DB instance, **backing up the entire DB instance and not just individual databases**. You can set the **backup retention period** when you create a DB instance. If you don't set the backup retention period, Amazon RDS uses a **default period retention period of one day**. You can modify the backup retention period; valid values are 0 (for no backup retention) to a **maximum of 35 days**.

Automatic Backups are taken **daily** at whichever time we specify, the **point in time recovery feature enables to recover the database at any point in time and AWS applies the transaction logs to the most appropriate DB backup**. 

During automated backup, Amazon RDS performs a **storage volume snapshot of the entire Database** Instance. Also, it captures **transaction logs every 5 minutes**. To restore a DB instance at a specific point of time, **a new DB instance is created using this DB snapshot**.

### Manual DB Snapshot(RDS backup)

DB snapshots are a manual thing where we user manually triggers the backup and then restores it from the desired time period.

**Note:** We highly **discourage disabling automated backups because it disables point-in-time recovery**. Disabling automatic backups for a DB instance deletes all existing automated backups for the instance. If you disable and then re-enable automated backups, you are only able to restore starting from the time you re-enabled automated backups.

## Amazon CloudWatch 

Amazon CloudWatch is a **monitoring and management service** which collects monitoring and operational data in the form of logs, metrics, and events, providing you with a **unified view of AWS resources, applications and services** that run on AWS, and **on-premises servers**. It captures **real-time events which can be further used to trigger AWS Lambda**

### Amazon CloudWatch alarm actions

Amazon CloudWatch alarm actions, you can create alarms that **automatically stop, terminate, reboot or recover your instances**. You can use the stop or terminate actions to **save money** when you no longer need an instance. You can use the reboot and recover actions to automatically reboot those instances or **recover them onto new hardware if a system impairment occurs.**

## EC2 (Elastic Compute)

### AWS OpsWorks Stacks

AWS OpsWorks Stacks lets you **manage applications and servers on AWS and on-premises**. With OpsWorks Stacks, you can **model your application as a stack containing different layers**, such as load balancing, database, and application server. You can deploy and configure Amazon EC2 instances in each layer or connect other resources such as Amazon RDS databases. 

A **stack** is basically a **collection of instances that are managed together** for serving a common task.

### VM Import/Export

VM Import/Export enables customers to **import Virtual Machine (VM) images** in order to create Amazon EC2 instances. Customers can also **export previously imported EC2 instances to create VMs**. Customers can use VM Import/Export to **leverage their previous investments in building VMs by migrating their VMs to Amazon EC2**.

### AWS CloudHSM 

To **backup** the AWS CloudHSM data to **Amazon S3 buckets** in the same region, AWS CloudHSM generates a **unique Ephemeral Backup Key (EBK) to encrypt all data using AES 256-bit encryption key**. This Ephemeral Backup Key (EBK) is further **encrypted using Persistent Backup Key (PBK)** which is also **AES 256-bit encryption key**.

**Data Key & Customer Managed Key are not used by AWS CloudHSM** for the encryption of data, instead of that **EBK & PBK are used**

While taking the backup of data from **different AWS CloudHSM clusters to the Amazon S3 bucket**, the **Amazon S3 bucket should be in the same region as that of the AWS CloudHSM cluster**.

### VPN CloudHub

If you have **multiple AWS Site-to-Site VPN connections**, you can provide **secure communication between sites using the AWS VPN CloudHub**. This enables your **remote sites to communicate with each other, and not just with the VPC**. The VPN CloudHub operates on a **simple hub-and-spoke model** that **you can use with or without a VPC**.

This design is **suitable if you have multiple branch offices and existing internet connections and would like to implement a convenient, potentially low-cost hub-and-spoke model for primary or backup connectivity between these remote offices**.

Sites that use **AWS Direct Connect connections to the virtual private gateway can also be part of the AWS VPN CloudHub**. 

### AWS Batch

AWS Batch Job sends log information to CloudWatch logs which requires **awslogs log driver** to be configured on **compute resources having customized AMI**. In case of Amazon **ECS-optimized AMI, these drivers are pre-configured**.

### AWS ElastiCache

#### At-Rest Encryption in ElastiCache for Redis

ElastiCache for Redis **at-rest encryption** is an optional feature to **increase data security by encrypting on-disk data**. When enabled on a replication group

ElastiCache for Redis offers default (service managed) encryption at rest, as well as **ability** to use **your own symmetric customer managed customer master keys in AWS Key Management Service (KMS)**.

**At-rest encryption can be enabled on a replication group only when it is created**. Because there is some processing needed to encrypt and decrypt the data, enabling at-rest encryption can have a performance impact during these operations. You should benchmark your data with and without at-rest encryption to determine the performance impact for your use cases.

### EC2-Classic Instance

For the communication between EC2-Classic instance and resources in VPC, **ClassicLink** should be used. 


#### NAT Gateway multi-AZ 

If you have resources in multiple Availability Zones and they share one NAT Gateway, in the event that the NAT Gateway’s Availability Zone is down, resources in the other Availability Zones lose internet access. **To create an Availability Zone-independent architecture, create a NAT Gateway in each Availability Zone and configure your routing to ensure that resources use the NAT Gateway in the same Availability Zone.**

### Gateway VPC Endpoint

Gateway VPC Endpoint provides secure private access to **Amazon S3 and DynamoDB** without traffic routing via the Internet. When Gateway Endpoints are created, **VPC Endpoint is created along with the addition of S3 prefixes in the routing table, pointing to VPC endpoints.**

Gateway VPC Endpoint **does not support access to Amazon ECR**; it **supports private access only to Amazon S3 & Amazon DynamoDB**.

### AWS PrivateLink 

* AWS PrivateLink provides **secure private access to various AWS services by adding an Elastic Network Interface within a VPC**. AWS creates a local/ regional DNS entry that resolves to the local IP address assigned to ENI.

**Note:** AWS PrivateLink does not support access to Amazon S3. Amazon S3 can be accessed privately from a VPC via Gateway VPC Endpoint.

### Security Group

Ensure that the security group allows **Inbound SSH** traffic from the IT Administrator’s Workstation. Since **Security groups are stateful, we do not have to configure outbound traffic**. What enters the inbound traffic is allowed in the outbound traffic too.

### Network ACL default config

The default network ACL is configured to **allow all traffic to flow in and out** of the subnets to which it is associated.

### AWS SSO

#### Connecting AWS SSO to On-Premise Active Directory

A **two-way trust relationship** is required because when two-way trust relationships are created between AWS Managed Microsoft AD and an on-premises Active Directory, on-premises users can sign in with their corporate credentials to various AWS services and business applications.

### Amazon Kinesis 

#### Amazon Kinesis Data Streams 

You can use Amazon Kinesis Data Streams to **collect and process large streams of data records in real-time**. Kinesis data streams are highly customizable and best suited for developers building custom applications or streaming data for specialized needs whereas Firehose handles **loading data streams directly into AWS products** for processing.

Amazon Kinesis Data Streams (KDS) is a **massively scalable and durable real-time data streaming service**. KDS can continuously capture **gigabytes of data per second from thousands of sources** such as website clickstreams, database event streams, financial transactions, social media feeds, IT logs, and location-tracking events.

Make your streaming data available to **multiple real-time analytics applications**, to Amazon S3 or to AWS Lambda within **70 milliseconds of the data being collected**.

#### Amazon Kinesis Data Firehose 

You should use Kinesis Data firehose if you want to do **some custom processing with streaming data**. With **Kinesis Data Firehose you are simply ingesting it into S3, Redshift or ElasticSearch**.

**Note: Firehose is nearly real-time but not exactly real-time**

### Network Load Balancer

**Network Load Balancer can be used to **terminate TLS connections** instead of back end instance reducing the load on this instance**. With Network Load Balancers, **millions of simultaneous sessions** can be established with **no impact on latency along with preserving client IP address**. To negotiate TLS connections with clients, NLB uses a security policy which consists of protocols & ciphers.

## AWS Config

AWS Config is a fully managed service that provides you with **a resource inventory, configuration history, and configuration change notifications to enable security and governance**. Also, you can discover existing AWS resources, export a complete inventory of your AWS resources with all configuration details, and **determine how a resource was configured at any point in time**.


### How CloudTrail and Config are Different

**Config reports on what has changed, whereas CloudTrail reports on who made the change, when, and from which location**. Config is focused on the configuration of your AWS resources and reports with detailed snapshots on how your resources have changed. CloudTrail focuses on the events, or API calls, that drive those changes. It focuses on the user, application, and activity performed on the system.

## CloudTrail

This is an **API monitoring service** and using CloudTrail, you can **log, continuously monitor, and retain account activity related to actions across your AWS infrastructure. CloudTrail provides event history of your AWS account activity, including actions taken through the AWS management console, AWS SDKs, command-line tools, and other AWS services. This event history simplifies security analysis, resource change tracking, and troubleshooting**. In addition, you can use CloudTrail to detect unusual activity in your AWS accounts.

Also, **CloudTrail records the changes made to AWS Config including who made the change, vice versa may not true**.

**AWS CloudTrail**: it is used to record AWS **API calls** for your account like,
* who made the **API call**?
* when was the **API call** made?
* what was the **API call**?
* which resources were acted upon in the **API call**?
* where was the **API calls** made form and made to?

**Note:** AWS has launched a new feature called [VPC Traffic Mirroring](https://aws.amazon.com/blogs/aws/new-vpc-traffic-mirroring/) which is used to **capture and inspect network traffic at scale**. 

### CloudTrail log file integrity

It will create a hash file called **digest file** which refers to logs that are generated. This digest file is saved in a **different folder in the S3 bucket where log files are saved**. Each of these digest files has the **private key of public & private key pair**. The Digest file can be validated using the public key. This feature **ensures that all the modifications made to CloudTrail log files are recorded.** 

By default all CloudTrail log files are delivered to S3 buckets using **SSE-S3 encryption** Amazon SSE-KMS encryption for CloudTrail log file, there would be an additional layer of security for log files,

## DynamoDB 

### DynamoDB Streams

When you enable **DynamoDB Streams** on a table, you can associate the **stream ARN with a Lambda function** that you write. Immediately after an item in the table is modified, a new record appears in the table's stream. AWS Lambda polls the stream and invokes your Lambda function **synchronously** when it detects new stream records. Since our requirement is to have an immediate entry made to an application in case an item in the DynamoDB table is modified, a lambda function is also required. 

### DynamoDB auto scaling

DynamoDB auto scaling uses the AWS Application Auto Scaling service to dynamically adjust provisioned throughput capacity on your behalf, in response to actual traffic patterns. This enables a table or a global secondary index to increase its provisioned read and write capacity to handle sudden increases in traffic, without throttling. When the workload decreases, Application Auto Scaling decreases the throughput so that you don't pay for unused provisioned capacity.

You can optionally allow **DynamoDB Auto-scaling to manage your table's throughput capacity**. However, you still must provide initial settings for read and write capacity when you create the table**. DynamoDB auto scaling uses these initial settings as a starting point and then adjusts them dynamically in response to your application's requirements. 

As your application data and access requirements change, you might need to **adjust your table's throughput settings**. If you're using **DynamoDB Auto-scaling, the throughput settings are automatically adjusted in response to actual workloads**. You can also use the **UpdateTable operation to manually adjust your table's throughput capacity**. You might decide to do this if you need to bulk-load data from an existing data store into your new DynamoDB table.


### Choose a partition key for dynamodb

We recommend that you **choose a partition key that can have a large number of distinct values** relative to the number of items in the table.

When an Amazon DynamoDB table is created, you can specify the desired throughput in Reads per second and Writes per second. The table will then be provisioned across multiple partitions sufficient to provide the requested throughput.

It is fully managed by DynamoDB. Additional partitions will be created as the quantity of data increases or when the provisioned throughput is increased.

### Integrating the Amazon RDS database with AWS Elastic Beanstalk

Best practice is to **launch RDS outside the AWS Elastic Beanstalk environment**. It helps in having multiple environments connecting to a single database, using database types not supported with the integrated database, performing blue/green deployments. Also, the database instance remains up & running when the AWS Elastic Beanstalk environment is terminated.

### [Error Handling with DynamoDB](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Programming.Errors.html#Programming.Errors.MessagesAndCodes)

#### HTTP Status Code 400

* AccessDeniedException
* ConditionalCheckFailedException
* IncompleteSignatureException
* ItemCollectionSizeLimitExceededException
* LimitExceededException
* MissingAuthenticationTokenException
* ProvisionedThroughputExceededException
* RequestLimitExceeded
* ResourceInUseException
* ResourceNotFoundException
* ThrottlingException
* UnrecognizedClientException
* ValidationException

#### HTTP Status Code 5xx

* Internal Server Error (HTTP 500)
* Service Unavailable (HTTP 503)

## EC2 Reserved Instance Utilization and Coverage reports

**EC2 RI Utilization %** offers relevant data to identify and **act on opportunities to increase your Reserved Instance usage efficiency**. It’s calculated by dividing the Reserved Instance used hours by total Reserved Instance purchased hours.

**EC2 RI Coverage %** shows **how much of your overall instance usage is covered by Reserved Instances**. This lets you make informed decisions about when to purchase or modify a Reserved Instance to ensure maximum coverage. It’s calculated by dividing the Reserved Instance used hours by total EC2 On-Demand and Reserved Instance hours.

**RI Coverage Budget reports the number of instances that are part of the Reserved Instance**. This helps you to **get an alert when the number of instances covered by reservation falls below 50% of the total number of instances launched. This report can identify the instance which is consistently running using On-Demand instance & can be converted to Reserved Instance for cost savings**.

AWS Organization member accounts' owners can create a budget for individual accounts. AWS Organization master account pays for usage incurred by all accounts in the organization.


## Control over the number of cores allocated to the application

In **EC2 - Dedicated Hosts** You have visibility of the number of sockets and physical cores that support your instances on a Dedicated Host. You can use this information to manage to license for your own server-bound software that is licensed per-socket or per-core.

## Route 53

### Routing policy

**Simple routing policy** – Use for a single resource that performs a given function for your domain, for example, a web server that serves content for the example.com website.

**Failover routing policy** – Use when you want to configure active-passive failover.

**Geolocation routing policy** – Use when you want to route traffic based on the location of your users.

**Geoproximity routing policy** – Use when you want to route traffic based on the location of your resources and, optionally, shift traffic from resources in one location to resources in another.

**Latency routing policy** – Use when you have resources in multiple AWS Regions and you want to route traffic to the region that provides the best latency.

**Multivalue answer routing policy** – Use when you want Route 53 to respond to DNS queries with up to eight healthy records selected at random.

**Weighted routing policy** – Use to route traffic to multiple resources in proportions that you specify.

## AWS Organization 

### Service Control Policies

* Service Control Policies will be **applied to all users within member accounts including root accounts within each of accounts**. These policies are **not applied to users who are part of these accounts under AWS Organisations & have permission to access AWS resources**.
* SCP **doesn't impact users outside the accounts with AWS Organisations** having access to AWS resources.
* SCP will **apply to all accounts within AWS organizations including root accounts of individual accounts in an AWS Organisation**.

### Sharing AWS Resources

For sharing AWS Resources with AWS Resource Access Manager, **sharing needs to be enabled with the master account of AWS Organisation**. 
**Only resources that are owned by the account are shared with other accounts & resources are not re-shared from other accounts.**

### Consolidate Billing

* Consolidate Billing combines the usage of all Accounts within an organization & shares Reserved Instance between multiple accounts. **This discount is valid only if the account which has purchased Reserved Instance is not using its all Instances & other accounts have launched Instance in the same AZ as that of the account which has purchased this Instance**.

* **When you purchase a Reserved Instance, you determine the scope of the Reserved Instance. The scope is either regional or zonal.**

* AWS Randomizes in AZs but you can choose the scope as **Zonal Reserved Instance** .Is in case you want to be in specific AZ or **if you want to get discount in any AZ, you should go for Regional Reserved Instances.**

* **When Consolidated Billing is enabled, Reserved Instance is by default shared between all accounts. No prior approvals are required to be granted for this sharing.**

* **Reserved Instance to be used by other accounts in the same organization, Instance should be launched in the any AZ and it can be part of different VPC.**

* Reserved Instance discounts, each Account should use launch instance in the same region

### VPC peering 

A VPC peering connection is a **networking connection between two VPCs** that enables you to route traffic between them **privately**. Instances in either VPC can communicate with each other as if they are within the same network. You can create a VPC peering connection between your own VPCs, **with a VPC in another AWS account**, or with a VPC in a different AWS Region**. **without going to internet.**

## Auto Scaling group(ASG)

### Schedule scalling

Scaling based on a schedule allows you to **scale your application in response to predictable load changes**. For example, every week the traffic to your web application starts to increase on Wednesday, remains high on Thursday, and starts to decrease on Friday. You can plan your scaling activities based on the predictable traffic patterns of your web application.

### Conditions

* If **two policies are executed at the same time, Amazon EC2 Auto Scaling follows the policy with the greater impact**. For example, if you have one policy to add two instances and another policy to add four instances, Amazon EC2 Auto Scaling adds four instances when both policies are triggered at the same time.
* If your scaling events are not based on the right metrics and do not have the right threshold defined, then the scaling will not occur as you want it to happen.
* **AutoScaling can be used without Load Balancer also.**
**Health checks will help us know the health status of an Auto Scaling instance. It is not a Check if AutoScaling is not working as expected.** It is a health check for Instance.

### Lifecycle Hooks to Auto Scaling group 

Adding Lifecycle Hooks to Auto Scaling group puts the instance into **wait state before termination**. During this wait state, you can perform **custom activities to retrieve critical operational data from a stateful instance**. **Default Wait period is 1 hour**.

### Termination policy 

Termination policy is used to specify **which instances to terminate first during scale in**. 

## AWS CloudFormation 

### AWS CloudFormation Drift Detection

AWS CloudFormation Drift Detection can be used to **detect changes made to AWS resources outside the CloudFormation Templates**. 
AWS CloudFormation Drift Detection **only checks property values that are explicitly set by stack templates or by specifying template parameters**. It does not determine drift for property values that are set by default. To determine drift for these resources, you can explicitly set property values which can be the same as that of the default value.

## Redshift

### Server side encryption Redshift

Amazon Redshift uses a **hierarchy of encryption keys to encrypt the database**. You can use either **AWS Key Management Service (AWS KMS) or a hardware security module (HSM) to manage the top-level encryption keys** in this hierarchy. The process that Amazon Redshift uses for encryption differs depending on how you manage keys.


## AWS Lambda

AWS Lambda **automatically monitors** Lambda functions on your behalf, reporting metrics through Amazon **CloudWatch**. To help you **troubleshoot failures in a function, Lambda logs all the requests handled by your function and also automatically stores logs generated by your code through Amazon CloudWatch Logs**.


Source: AWS Official Documents

