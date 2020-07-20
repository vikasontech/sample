---
title: "Ec2"
date: 2020-07-20T14:53:37+07:00
draft: false
---

# What is Amazon EC2?

Elastic Compute Cloud (Amazon EC2) provides scalable computing capacity in the Amazon Web Services (AWS) cloud. Using Amazon EC2 eliminates your need to invest in hardware up front, so you can develop and deploy applications faster. 

An Amazon Machine Image (AMI) is a template that contains a software configuration (for example, an operating system, an application server, and applications).
An instance is a virtual server in the cloud. 

# Security best practices

Use AWS Identity and Access Management (IAM) to control access to your AWS resources, including your instances. You can create IAM users and groups under your AWS account, assign security credentials to each, and control the access that each has to resources and services in AWS.

Restrict access by only allowing trusted hosts or networks to access ports on your instance. 

Review the rules in your security groups regularly, and ensure that you apply the principle of least privilege—only open up permissions that you require. You can also create different security groups to deal with instances that have different security requirements. Consider creating a bastion security group that allows external logins, and keep the remainder of your instances in a group that does not allow external logins.

Disable password-based logins for instances launched from your AMI. Passwords can be found or cracked, and are a security risk.

Each Region is a separate geographic area. Each Region has multiple, isolated locations known as Availability Zones

Local Zones provide you the ability to place resources, such as compute and storage, in multiple locations closer to your end users. Resources aren't replicated across Regions unless you specifically choose to do so

Each Region is completely independent. Each Availability Zone is isolated, but the Availability Zones in a Region are connected through low-latency links. 

# Regions 

Each Amazon EC2 Region is designed to be isolated from the other Amazon EC2 Regions. This achieves the greatest possible fault tolerance and stability.

# Availability Zones

An Availability Zone (AZ) is one or more discrete data centers with redundant power, networking, and connectivity in an AWS Region. Availability Zones allow you to operate production applications and databases that are more highly available, fault tolerant, and scalable than would be possible from a single data center. All Availability Zones in an AWS Region are interconnected with high-bandwidth, low-latency networking, over fully redundant, dedicated metro fiber providing high-throughput, low-latency networking between Availability Zones. All traffic between Availability Zones is encrypted. The network performance is sufficient to accomplish synchronous replication between Availability Zones. Availability Zones make it easier to partition applications for high availability. If an application is partitioned across Availability Zones, the application is better isolated and protected from issues such as power outages, lightning strikes, tornadoes, earthquakes, and more. Availability Zones are physically separated by a meaningful distance from any other Availability Zone, although all are within 100 km (60 miles) of each other.

# Network border groups

A network border group is a unique set of Availability Zones or Local Zones from where AWS advertises IP addresses. You can allocate the following resources from a network border group:

Elastic IPv4 addresses that Amazon provides
IPv6 Amazon-provided VPC addresses
A network border group limits the addresses to the group. IP addresses cannot move between network border groups.

# Migrating an instance to another Availability Zone

you can migrate an instance from one Availability Zone to another.

The migration process involves:

* Creating an AMI from the original instance
* Launching an instance in the new Availability Zone
* Updating the configuration of the new instance, as shown in the following procedure


# AMI virtualization types

two types of virtualization: paravirtual (PV) or hardware virtual machine (HVM). The main differences between PV and HVM AMIs are the way in which they boot and whether they can take advantage of special hardware extensions (CPU, network, and storage) for better performance.

## HVM AMIs

For best performance, we recommend that you use an HVM AMI. In addition, HVM AMIs are required to take advantage of enhanced networking. HVM virtualization uses hardware-assist technology provided by the AWS platform. With HVM virtualization, the guest VM runs as if it were on a native hardware platform, except that it still uses PV network and storage drivers for improved performance.

HVM AMIs are presented with a fully virtualized set of hardware and boot by executing the master boot record of the root block device of your image. This virtualization type provides the ability to run an operating system directly on top of a virtual machine without any modification, as if it were run on the bare-metal hardware. The Amazon EC2 host system emulates some or all of the underlying hardware that is presented to the guest.

## PV AMIs

PV AMIs boot with a special boot loader called PV-GRUB, which starts the boot cycle and then chain loads the kernel specified in the menu.lst file on your image. Paravirtual guests can run on host hardware that does not have explicit support for virtualization, but they cannot take advantage of special hardware extensions such as enhanced networking or GPU processing. Historically, PV guests had better performance than HVM guests in many cases, but because of enhancements in HVM virtualization and the availability of PV drivers for HVM AMIs, this is no longer true.


# bookmark for your AMI

you can create a bookmark that allows a user to access your AMI and launch an instance in their own account immediately. This is an easy way to share AMI references, so users don't have to spend time finding your AMI in order to use it.

# Convert instance stored to EBS store

You can't convert an instance store-backed Windows AMI to an Amazon EBS-backed Windows AMI and you cannot convert an AMI that you do not own.


# Changing the instance type

you can change the size of your instance. For example, if your t2.micro instance is too small for its workload, you can change it to another instance type that is appropriate for the workload.

# Compatibility for resizing instances

* You can't resize an instance that was launched from a PV AMI to an instance type that is HVM only. 
* you can't resize an instance in the EC2-Classic platform to a instance type that is available only in a VPC unless you have a nondefault VPC.
* Instance types that support enhanced networking require the necessary drivers installed.


# Instance purchasing options

* On-Demand Instances – Pay, by the second, for the instances that you launch.
* Savings Plans – Reduce your Amazon EC2 costs by making a commitment to a consistent amount of usage, in USD per hour, for a term of 1 or 3 years.
* Reserved Instances – Reduce your Amazon EC2 costs by making a commitment to a consistent instance configuration, including instance type and Region, for a term of 1 or 3 years.
* Scheduled Instances – Purchase instances that are always available on the specified recurring schedule, for a one-year term.
* Spot Instances – Request unused EC2 instances, which can reduce your Amazon EC2 costs significantly.
* Dedicated Hosts – Pay for a physical host that is fully dedicated to running your instances, and bring your existing per-socket, per-core, or per-VM software licenses to reduce costs.
* Dedicated Instances – Pay, by the hour, for instances that run on single-tenant hardware.
* Capacity Reservations – Reserve capacity for your EC2 instances in a specific Availability Zone for any duration.

# Key variables that determine Reserved Instance pricing

Instance attributes

A Reserved Instance has four instance attributes that determine its price.

Instance type: For example, m4.large. This is composed of the instance family (for example, m4) and the instance size (for example, large).
Region: The Region in which the Reserved Instance is purchased.
Tenancy: Whether your instance runs on shared (default) or single-tenant (dedicated) hardware. For more information, see Dedicated Instances.
Platform: The operating system; for example, Windows or Linux/Unix. For more information, see Choosing a platform.

# Term commitment

You can purchase a Reserved Instance for a one-year or three-year commitment, with the three-year commitment offering a bigger discount.

One-year: A year is defined as 31536000 seconds (365 days).
Three-year: Three years is defined as 94608000 seconds (1095 days).


# Scheduled Reserved Instances

Scheduled Instances are a good choice for workloads that do not run continuously, but do run on a regular schedule. For example, you can use Scheduled Instances for an application that runs during business hours or for batch processing that runs at the end of the week.

You can't stop or reboot Scheduled Instances, but you can terminate them manually as needed. If you terminate a Scheduled Instance before its current scheduled time period ends, you can launch it again after a few minutes. Otherwise, you must wait until the next scheduled time period.
Amazon EC2 terminates the EC2 instances three minutes before the end of the current scheduled time period.

# Spot Instances

A Spot Instance is an unused EC2 instance that is available for less than the On-Demand price. Because Spot Instances enable you to request unused EC2 instances at steep discounts, you can lower your Amazon EC2 costs significantly. The hourly price for a Spot Instance is called a Spot price. The Spot price of each instance type in each Availability Zone is set by Amazon EC2, and adjusted gradually based on the long-term supply of and demand for Spot Instances. Your Spot Instance runs whenever capacity is available and the maximum price per hour for your request exceeds the Spot price.

Spot Instances are a cost-effective choice if you can be flexible about when your applications run and if your applications can be interrupted. For example, Spot Instances are well-suited for data analysis, batch jobs, background processing, and optional tasks. For more information, see

Spot Fleet – A set of Spot Instances that is launched based on criteria that you specify. The Spot Fleet selects the Spot Instance pools that meet your needs and launches Spot Instances to meet the target capacity for the fleet.

Spot Instance interruption – Amazon EC2 terminates, stops, or hibernates your Spot Instance when the Spot price exceeds the maximum price for your request or capacity is no longer available. Amazon EC2 provides a Spot Instance interruption notice, which gives the instance a two-minute warning before it is interrupted.


# Dedicated Hosts

An Amazon EC2 Dedicated Host is a physical server with EC2 instance capacity fully dedicated to your use. Dedicated Hosts allow you to use your existing per-socket, per-core, or per-VM software licenses, including Windows Server, Microsoft SQL Server, SUSE, and Linux Enterprise Server.

# Dedicated Instances

Dedicated Instances are Amazon EC2 instances that run in a virtual private cloud (VPC) on hardware that's dedicated to a single customer. Dedicated Instances that belong to different AWS accounts are physically isolated at a hardware level, even if those accounts are linked to a single payer account. However, Dedicated Instances may share hardware with other instances from the same AWS account that are not Dedicated Instances.

# Capacity Reservation Limits

The number of instances for which you are allowed to reserve capacity is based on your account's On-Demand Instance limit. You can reserve capacity for as many instances as that limit allows, minus the number of instances that are already running.

# Capacity Reservation Limitations and Restrictions

* Active and unused Capacity Reservations count toward your On-Demand Instance limits
* Capacity Reservations are not transferable from one AWS account to another. However, you can share Capacity Reservations with other AWS accounts. For more information, see Working with Shared Capacity Reservations.
* Zonal Reserved Instance billing discounts do not apply to Capacity Reservations
* Capacity Reservations can't be created in placement groups
* Capacity Reservations can't be used with Dedicated Hosts
* Capacity Reservations can't be used with Bring Your Own License (BYOL)
* Capacity Reservations can't be used with Local Zones


# Hibernate your Linux instance

When you hibernate an instance, we signal the operating system to perform hibernation (suspend-to-disk). Hibernation saves the contents from the instance memory (RAM) to your Amazon EBS root volume. 

# Hibernation prerequisites

Instance RAM size - must be less than 150 GB.
* Root volume type - must be an Amazon EBS volume, not an instance store volume.
* Amazon EBS root volume size - must be large enough to store the RAM contents and accommodate your expected usage, 
* Amazon EBS root volume encryption - To use hibernation, the root volume must be encrypted to ensure the protection of sensitive content that is in memory at the time of hibernation.
* Enable hibernation at launch - You cannot enable hibernation on an existing instance (running or stopped). 
* Purchasing options - This feature is available for On-Demand Instances and Reserved Instances. It is not available for Spot Instances.

The following actions are not supported for hibernation:

* Changing the instance type or size of a hibernated instance
* Creating snapshots or AMIs from instances for which hibernation is enabled
* Creating snapshots or AMIs from hibernated instances
* You cannot hibernate an instance that is in an Auto Scaling group or used by Amazon ECS. If your instance is in an Auto Scaling group and you try to hibernate it, the Amazon EC2 Auto Scaling service marks the stopped instance as unhealthy, and may terminate it and launch a replacement instance.
* do not support keeping an instance hibernated for more than 60 days. To keep the instance for longer than 60 days, you must start the hibernated instance, stop the instance, and start it.


# Retrieving instance metadata

To view all categories of instance metadata from within a running instance, use the following URI.

http://169.254.169.254/latest/meta-data/

# Monitoring Amazon EC2

you should create a monitoring plan that should include:

What are your goals for monitoring?
* What resources will you monitor?
* How often will you monitor these resources?
* What monitoring tools will you use?
* Who will perform the monitoring tasks?
* Who should be notified when something goes wrong?


# Best practices for monitoring


## CloudWatch agent

You can use the CloudWatch agent to collect both system metrics and log files from Amazon EC2 instances and on-premises servers. The agent supports both Windows Server and Linux, and enables you to select the metrics to be collected, including sub-resource metrics such as per-CPU core. We recommend that you use the agent to collect metrics and logs instead of using the monitoring scripts. 


## AWS CloudTrail

Amazon EC2 and Amazon EBS are integrated with AWS CloudTrail, a service that provides a record of actions taken by a user, role, or an AWS service in Amazon EC2 and Amazon EBS. CloudTrail captures all API calls for Amazon EC2 and Amazon EBS as events, including calls from the console and from code calls to the APIs. If you create a trail, you can enable continuous delivery of CloudTrail events to an Amazon S3 bucket, including events for Amazon EC2 and Amazon EBS. If you don't configure a trail, you can still view the most recent events in the CloudTrail console in Event history. Using the information collected by CloudTrail, you can determine the request that was made to Amazon EC2 and Amazon EBS, the IP address from which the request was made, who made the request, when it was made, and additional details.

A trail enables CloudTrail to deliver log files to an Amazon S3 bucket. By default, when you create a trail in the console, the trail applies to all Regions. The trail logs events from all Regions in the AWS partition and delivers the log files to the Amazon S3 bucket that you specify. Additionally, you can configure other AWS services to further analyze and act upon the event data collected in CloudTrail 


# Public IP Address

we release the public IP address from your instance, or assign it a new one:

* We release your instance's public IP address when it is stopped, hibernated, or terminated. Your stopped or hibernated instance receives a new public IP address when it is started.
* We release your instance's public IP address when you associate an Elastic IP address with it. When you disassociate the Elastic IP address from your instance, it receives a new public IP address.
* If the public IP address of your instance in a VPC has been released, it will not receive a new one if there is more than one network interface attached to your instance.
* If your instance's public IP address is released while it has a secondary private IP address that is associated with an Elastic IP address, the instance does not receive a new public IP address.


# Elastic IP addresses (IPv4)

An Elastic IP address is a public IPv4 address that you can allocate to your account. You can associate it to and from instances as you require, and it's allocated to your account until you choose to release it.


# Multiple IP addresses

You can specify multiple private IPv4 and IPv6 addresses for your instances. The number of network interfaces and private IPv4 and IPv6 addresses that you can specify for an instance depends on the instance type.

It can be useful to assign multiple IP addresses to an instance in your VPC to do the following:

* Host multiple websites on a single server by using multiple SSL certificates on a single server and associating each certificate with a specific IP address.
* Operate network appliances, such as firewalls or load balancers, that have multiple IP addresses for each network interface.
* Redirect internal traffic to a standby instance in case your instance fails, by reassigning the secondary IP address to the standby instance.


# Using reverse DNS for email applications

assigning a static reverse DNS record to your Elastic IP address that is used to send email can help avoid having email flagged as spam by some anti-spam organizations. Note that a corresponding forward DNS record (record type A) pointing to your Elastic IP address must exist before we can create your reverse DNS record.

Best practices for configuring network interfaces

* You can attach a network interface to an instance when it's running (hot attach), when it's stopped (warm attach), or when the instance is being launched (cold attach).
* You can detach secondary network interfaces when the instance is running or stopped. However, you can't detach the primary network interface.
* You can move a network interface from one instance to another, if the instances are in the same Availability Zone and VPC but in different subnets.
* When launching an instance using the CLI, API, or an SDK, you can specify the primary network interface and additional network interfaces.
* Launching an Amazon Linux or Windows Server instance with multiple network interfaces automatically configures interfaces, private IPv4 addresses, and route tables on the operating system of the instance.
* A warm or hot attach of an additional network interface may require you to manually bring up the second interface, configure the private IPv4 address, and modify the route table accordingly. Instances running Amazon Linux or Windows Server automatically recognize the warm or hot attach and configure themselves.
* Attaching another network interface to an instance (for example, a NIC teaming configuration) cannot be used as a method to increase or double the network bandwidth to or from the dual-homed instance.
* If you attach two or more network interfaces from the same subnet to an instance, you may encounter networking issues such as asymmetric routing. If possible, use a secondary private IPv4 address on the primary network interface instead. For more information, see Assigning a secondary private IPv4 address.


# Enhanced networking on Linux

Enhanced networking uses single root I/O virtualization (SR-IOV) to provide high-performance networking capabilities on supported instance types. SR-IOV is a method of device virtualization that provides higher I/O performance and lower CPU utilization when compared to traditional virtualized network interfaces. Enhanced networking provides higher bandwidth, higher packet per second (PPS) performance, and consistently lower inter-instance latencies. There is no additional charge for using enhanced networking.

# Enhanced networking types

## Elastic Network Adapter (ENA)
The Elastic Network Adapter (ENA) supports network speeds of up to 100 Gbps for supported instance types.

## Intel 82599 Virtual Function (VF) interface
The Intel 82599 Virtual Function interface supports network speeds of up to 10 Gbps for supported instance types.


# Elastic Fabric Adapter

An Elastic Fabric Adapter (EFA) is a network device that you can attach to your Amazon EC2 instance to accelerate High Performance Computing (HPC) and machine learning applications. EFA enables you to achieve the application performance of an on-premises HPC cluster, with the scalability, flexibility, and elasticity provided by the AWS Cloud.

**Note:** The OS-bypass capabilities of EFAs are not supported on Windows instances. If you attach an EFA to a Windows instance, the instance functions as an Elastic Network Adapter, without the added EFA capabilities.

# Differences between EFAs and ENAs

Elastic Network Adapters (ENAs) provide traditional IP networking features that are required to support VPC networking. EFAs provide all of the same traditional IP networking features as ENAs, and they also support OS-bypass capabilities. OS-bypass enables HPC and machine learning applications to bypass the operating system kernel and to communicate directly with the EFA device.


# EFA limitations

* The EFA must be a member of a security group that allows all inbound and outbound traffic to and from the security group itself.
* EFA OS-bypass traffic is not routable. Normal IP traffic from the EFA remains routable.
* You can attach only one EFA per instance.


# Monitoring an EFA

Amazon VPC flow logs
VPC Flow Log to capture information about the traffic going to and from an EFA. Flow log data can be published to Amazon CloudWatch Logs and Amazon S3.


# Amazon CloudWatch

Amazon CloudWatch provides metrics that enable you to monitor your EFAs in real time. You can collect and track metrics, create customized dashboards, and set alarms that notify you or take actions when a specified metric reaches a threshold that you specify. 


# Placement groups

You can use placement groups to influence the placement of a group of interdependent instances to meet the needs of your workload. Depending on the type of workload, you can create a placement group using one of the following placement strategies:

Cluster – packs instances close together inside an Availability Zone. This strategy enables workloads to achieve the low-latency network performance necessary for tightly-coupled node-to-node communication that is typical of HPC applications.

Partition – spreads your instances across logical partitions such that groups of instances in one partition do not share the underlying hardware with groups of instances in different partitions. This strategy is typically used by large distributed and replicated workloads, such as Hadoop, Cassandra, and Kafka.

Spread – strictly places a small group of instances across distinct underlying hardware to reduce correlated failures.


# Cluster placement groups

A cluster placement group is a logical grouping of instances within a single Availability Zone. A cluster placement group can span peered VPCs in the same Region. Instances in the same cluster placement group enjoy a higher per-flow throughput limit of up to 10 Gbps for TCP/IP traffic and are placed in the same high-bisection bandwidth segment of the network.

Cluster placement groups are recommended for applications that benefit from low network latency, high network throughput, or both. They are also recommended when the majority of the network traffic is between the instances in the group. To provide the lowest latency and the highest packet-per-second network performance for your placement group, choose an instance type that supports enhanced networking

# Partition placement groups

Partition placement groups help reduce the likelihood of correlated hardware failures for your application. When using partition placement groups, Amazon EC2 divides each group into logical segments called partitions. Amazon EC2 ensures that each partition within a placement group has its own set of racks. Each rack has its own network and power source. No two partitions within a placement group share the same racks, allowing you to isolate the impact of hardware failure within your application.

Partition placement groups can be used to deploy large distributed and replicated workloads, such as HDFS, HBase, and Cassandra, across distinct racks. When you launch instances into a partition placement group, Amazon EC2 tries to distribute the instances evenly across the number of partitions that you specify. You can also launch instances into a specific partition to have more control over where the instances are placed.
A partition placement group can have a maximum of seven partitions per Availability Zone.
The number of instances that can be launched into a partition placement group is limited only by the limits of your account.

# Spread placement groups

A spread placement group is a group of instances that are each placed on distinct racks, with each rack having its own network and power source.
Spread placement groups are recommended for applications that have a small number of critical instances that should be kept separate from each other. Launching instances in a spread placement group reduces the risk of simultaneous failures that might occur when instances share the same racks. Spread placement groups provide access to distinct racks, and are therefore suitable for mixing instance types or launching instances over time.  
A spread placement group can span multiple Availability Zones in the same Region.
Placement group rules and limitations
* you can't merge placement groups.
* An instance can be launched in one placement group at a time; it cannot span multiple placement groups.
* Cluster placement group rules and limitations
* A cluster placement group can't span multiple Availability Zones.
* Network traffic to the internet and over an AWS Direct Connect connection to on-premises resources is limited to 5 Gbps.
* Partition placement group rules and limitations

The following rules apply to partition placement groups:

A partition placement group supports a maximum of seven partitions per Availability Zone. The number of instances that you can launch in a partition placement group is limited only by your account limits.

When instances are launched into a partition placement group, Amazon EC2 tries to evenly distribute the instances across all partitions. Amazon EC2 doesn’t guarantee an even distribution of instances across all partitions.

A partition placement group with Dedicated Instances can have a maximum of two partitions.

Partition placement groups are not supported for Dedicated Hosts.

### Spread placement group rules and limitations

The following rules apply to spread placement groups:

A spread placement group supports a maximum of seven running instances per Availability Zone. For example, in a Region with three Availability Zones, you can run a total of 21 instances in the group (seven per zone). If you try to start an eighth instance in the same Availability Zone and in the same spread placement group, the instance will not launch. If you need to have more than seven instances in an Availability Zone, then the recommendation is to use multiple spread placement groups. Using multiple spread placement groups does not provide guarantees about the spread of instances between groups, but it does ensure the spread for each group, thus limiting impact from certain classes of failures.

Spread placement groups are not supported for Dedicated Instances or Dedicated Hosts.

# ClassicLink

ClassicLink allows you to link EC2-Classic instances to a VPC in your account, within the same Region. If you associate the VPC security groups with a EC2-Classic instance, this enables communication between your EC2-Classic instance and instances in your VPC using private IPv4 addresses. ClassicLink removes the need to make use of public IPv4 addresses or Elastic IP addresses to enable communication between instances in these platforms.
Linked EC2-Classic instances can access the following AWS services in the VPC: Amazon Redshift, Amazon ElastiCache, Elastic Load Balancing, and Amazon RDS. However, instances in the VPC cannot access the AWS services provisioned by the EC2-Classic platform using ClassicLink.
ou can register your linked EC2-Classic instances with the load balancer.
you can create an Amazon EC2 Auto Scaling group with instances that are automatically linked to a specified ClassicLink-enabled VPC at launch. 
By default, IAM users do not have permission to work with ClassicLink. You can create an IAM policy that grants users permissions to enable or disable a VPC for ClassicLink, link or unlink an instance to a ClassicLink-enabled VPC, and to view ClassicLink-enabled VPCs and linked EC2-Classic instances.
Linking your EC2-Classic instance to a VPC does not affect your EC2-Classic security groups. They continue to control all traffic to and from the instance. This excludes traffic to and from instances in the VPC, which is controlled by the VPC security groups that you associated with the EC2-Classic instance


# Enabling a VPC peering connection for ClassicLink

If you have a VPC peering connection between two VPCs, and there are one or more EC2-Classic instances that are linked to one or both of the VPCs via ClassicLink, you can extend the VPC peering connection to enable communication between the EC2-Classic instances and the instances in the VPC on the other side of the VPC peering connection. 

# Amazon EC2 and interface VPC endpoints

You can improve the security posture of your VPC by configuring Amazon EC2 to use an interface VPC endpoint. Interface endpoints are powered by AWS PrivateLink, a technology that enables you to privately access Amazon EC2 APIs by restricting all network traffic between your VPC and Amazon EC2 to the Amazon network. With interface endpoints, you also don't need an internet gateway, a NAT device, or a virtual private gateway.

We also recommend that you secure your data in the following ways:
Use multi-factor authentication (MFA) with each account.
Use TLS to communicate with AWS resources.
Set up API and user activity logging with AWS CloudTrail.
Use AWS encryption solutions, along with all default security controls within AWS services.
Use advanced managed security services such as Amazon Macie, which assists in discovering and securing personal data that is stored in Amazon S3.

# Security group rules

By default, security groups allow all outbound traffic.
Security group rules are always permissive; you can't create rules that deny access.
Security groups are stateful

When you associate multiple security groups with an instance, the rules from each security group are effectively aggregated to create one set of rules. Amazon EC2 uses this set of rules to determine whether to allow access.


# Default security groups

Your AWS account automatically has a default security group for the default VPC in each Region. If you don't specify a security group when you launch an instance, the instance is automatically associated with the default security group for the VPC.

The following are the default rules for each default security group:

* Allows all inbound traffic from other instances associated with the default security group. The security group specifies itself as a source security group in its inbound rules.
* Allows all outbound traffic from the instance.


# Amazon EBS

Amazon EBS provides durable, block-level storage volumes that you can attach to a running instance. You can use Amazon EBS as a primary storage device for data that requires frequent and granular updates. For example, Amazon EBS is the recommended storage option when you run a database on an instance.

# Amazon EC2 instance store

Many instances can access storage from disks that are physically attached to the host computer. This disk storage is referred to as instance store. Instance store provides temporary block-level storage for instances.

# Amazon EFS file system

Amazon EFS provides scalable file storage for use with Amazon EC2. You can create an EFS file system and configure your instances to mount the file system. You can use an EFS file system as a common data source for workloads and applications running on multiple instances


# Amazon S3

Amazon S3 provides access to reliable and inexpensive data storage infrastructure. It is designed to make web-scale computing easier by enabling you to store and retrieve any amount of data, at any time, from within Amazon EC2 or anywhere on the web.

# Benefits of using EBS volumes

## Data availability
When you create an EBS volume, it is automatically replicated within its Availability Zone to prevent data loss due to failure of any single hardware component. You can attach an EBS volume to any EC2 instance in the same Availability Zone. After you attach a volume, it appears as a native block device similar to a hard drive or other physical device. At that point, the instance can interact with the volume just as it would with a local drive. You can connect to the instance and format the EBS volume with a file system, such as ext3, and then install applications.
An EBS volume is off-instance storage that can persist independently from the life of an instance
By default, the root EBS volume that is created and attached to an instance at launch is deleted when that instance is terminated. You can modify this behavior by changing the value of the flag DeleteOnTermination to false when you launch the instance. This modified value causes the volume to persist even after the instance is terminated, and enables you to attach the volume to another instance.
By default, additional EBS volumes that are created and attached to an instance at launch are not deleted when that instance is terminated. You can modify this behavior 

## Data encryption

you can create encrypted EBS volumes with the Amazon EBS encryption feature. All EBS volume types support encryption. You can use encrypted EBS volumes to meet a wide range of data-at-rest encryption requirements for regulated/audited data and applications. Amazon EBS encryption uses 256-bit Advanced Encryption Standard algorithms (AES-256) and an Amazon-managed key infrastructure.
Amazon EBS provides the ability to create snapshots (backups) of any EBS volume and write a copy of the data in the volume to Amazon S3, where it is stored redundantly in multiple Availability Zones. The volume does not need to be attached to a running instance in order to take a snapshot. As you continue to write data to a volume, you can periodically create a snapshot of the volume to use as a baseline for new volumes.
Snapshots are incremental backups, meaning that only the blocks on the volume that have changed after your most recent snapshot are saved. If you have a volume with 100 GiB of data, but only 5 GiB of data have changed since your last snapshot, only the 5 GiB of modified data is written to Amazon S3. 

## Flexibility

EBS volumes support live configuration changes while in production. You can modify volume type, volume size, and IOPS capacity without service interruptions


# Multi-volume snapshots

You can create multi-volume snapshots, which are point-in-time snapshots for all EBS volumes attached to an EC2 instance. You can also create lifecycle policies to automate the creation and retention of multi-volume snapshots. For more information, see Automating the Amazon EBS snapshot lifecycle.

After the snapshots are created, each snapshot is treated as an individual snapshot. You can perform all snapshot operations, such as restore, delete, and copy across Regions or accounts, just as you would with a single volume snapshot. You can also tag your multi-volume snapshots as you would a single volume snapshot. We recommend you tag your multiple volume snapshots to manage them collectively during restore, copy, or retention.
To copy multi-volume snapshots to another AWS Region, retrieve the snapshots using the tag you applied to the multi-volume snapshots group when you created it. Then individually copy the snapshots to another Region.
If you would like another account to be able to copy your snapshot, you must either modify the snapshot permissions to allow access to that account or make the snapshot public so that all AWS accounts can copy it.
If you copy a snapshot to a new Region, a complete (non-incremental) copy is always created, resulting in additional delay and storage costs.
If you copy a snapshot and encrypt it to a new CMK, a complete (non-incremental) copy is always created, resulting in additional delay and storage costs.


# Accessing the contents of an EBS snapshot

ou can use the Amazon Elastic Block Store (Amazon EBS) direct APIs to create EBS snapshots, write data directly to your snapshots, read data on your snapshots, and identify the differences or changes between two snapshots. If you’re an independent software vendor (ISV) who offers backup services for Amazon EBS, the EBS direct APIs make it more efficient and cost-effective to track incremental changes on your EBS volumes through snapshots. This can be done without having to create new volumes from snapshots, and then use Amazon Elastic Compute Cloud (Amazon EC2) instances to compare the differences.
your Amazon EBS volumes. Automating snapshot management helps you to:

Protect valuable data by enforcing a regular backup schedule.
Retain backups as required by auditors or internal compliance.
Reduce storage costs by deleting outdated backups.

# Amazon EBS fast snapshot restore

Amazon EBS fast snapshot restore enables you to create a volume from a snapshot that is fully-initialized at creation. This eliminates the latency of I/O operations on a block when it is accessed for the first time. Volumes created using fast snapshot restore instantly deliver all of their provisioned performance.

To get started, enable fast snapshot restore for specific snapshots in specific Availability Zones. Each snapshot and Availability Zone pair refers to one fast snapshot restore. You can enable up to 50 fast snapshot restores per Region. When you create a volume from one of these snapshots in one of its enabled Availability Zones, the volume is restored using fast snapshot restore.

