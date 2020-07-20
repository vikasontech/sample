---
title: "Redshift"
date: 2020-07-20T16:47:18+07:00
draft: false
---

# AWS accounts and IAM credentials

By default, an Amazon Redshift cluster is only accessible to the AWS account that creates the cluster.

# Security groups

By default, any cluster that you create is closed to everyone. 

IAM credentials only control access to the Amazon Redshift API-related resources: the Amazon Redshift console, command line interface (CLI), API, and SDK. To enable access to the cluster from SQL client tools via JDBC or ODBC, you use security groups:

If you are using the EC2-VPC platform for your Amazon Redshift cluster, you must use VPC security groups.

You cannot move a cluster to a VPC after it has been launched with EC2-Classic. However, you can restore an EC2-Classic snapshot to an EC2-VPC cluster using the Amazon Redshift console

If you are using the EC2-Classic platform for your Amazon Redshift cluster, you must use Amazon Redshift security groups.

# Encryption

Encryption is an immutable property of the cluster. The only way to switch from an encrypted cluster to a cluster that is not encrypted is to unload the data and reload it into a new cluster. 
Encryption applies to the cluster and any backups. When you restore a cluster from an encrypted snapshot, the new cluster is encrypted as well.

you can use AWS Key Management Service (AWS KMS) to manage your Amazon Redshift encryption keys.

# Creating cluster snapshots

There are two types of snapshots: automated and manual. Amazon Redshift stores these snapshots internally in Amazon Simple Storage Service (Amazon S3) by using an encrypted Secure Sockets Layer (SSL) connection. If you need to restore from a snapshot, Amazon Redshift creates a new cluster and imports data from the snapshot that you specify.
You can use elastic resize to scale your cluster by changing the node type and number of nodes. Or, if your new node configuration is not available through elastic resize, you can use classic resize.
To resize your cluster, use one of the following approaches:

* Elastic resize
  * elastic resize takes 10–15 minutes. We recommend using elastic resize when possible

* Classic resize
  * classic resize takes 2 hours–2 days or longer, depending on your data's size.

Elastic resize is available only for clusters that use the EC2-VPC platform. 

# Managing snapshots 

Amazon Redshift takes automatic, incremental snapshots of your data periodically and saves them to Amazon S3. Additionally, you can take manual snapshots of your data whenever you want. In this section, you can find how to manage your snapshots from the Amazon Redshift console.

# Using service-linked roles

A service-linked role makes setting up Amazon Redshift easier because you don't have to manually add the necessary permissions. The role is linked to Amazon Redshift use cases and has predefined permissions. 
Only Amazon Redshift can assume the role, and only the service-linked role can use the predefined permissions policy. 
Amazon Redshift creates a service-linked role in your account the first time you create a cluster. You can delete the service-linked role only after you delete all of the Amazon Redshift clusters in your account. This protects your Amazon Redshift resources because you can't inadvertently remove permissions needed for access to the resources.

Amazon Redshift uses the service-linked role named AWSServiceRoleForRedshift 

Amazon Redshift does not allow you to edit the AWSServiceRoleForRedshift service-linked role. 

# Resilience in Amazon Redshift

AWS Regions provide multiple, physically separated and isolated Availability Zones that are connected with **low latency, high throughput, and highly redundant networking**. With Availability Zones, you can design and operate applications and databases that **automatically fail over between Availability Zones without interruption**. Availability Zones are more highly available, fault tolerant, and scalable than traditional single data center infrastructures or multiple data center infrastructures.
Almost all AWS Regions have multiple Availability Zones and data centers. You can deploy your applications across multiple Availability Zones in the same Region for fault tolerance and low latency.

