---
title: "Auto scaling group (ASG)"
date: 2020-07-21T18:14:44+07:00
draft: false
---
# What Is Amazon EC2 Auto Scaling?

Amazon EC2 Auto Scaling helps you ensure that you have the correct number of Amazon EC2 instances available to handle the load for your application. You create collections of EC2 instances, called Auto Scaling groups. You can specify the minimum number of instances in each Auto Scaling group, and Amazon EC2 Auto Scaling ensures that your group never goes below this size.
You can specify the maximum number of instances in each Auto Scaling group, and Amazon EC2 Auto Scaling ensures that your group never goes above this size

# PCI DSS Compliance

Auto Scaling supports the processing, storage, and transmission of credit card data by a merchant or service provider, and has been validated as being compliant with Payment Card Industry (PCI) Data Security Standard (DSS).

# Benefits of Auto Scaling:

Better fault tolerance. Amazon EC2 Auto Scaling can detect when an instance is unhealthy, terminate it, and launch an instance to replace it. You can also configure Amazon EC2 Auto Scaling to use multiple Availability Zones. If one Availability Zone becomes unavailable, Amazon EC2 Auto Scaling can launch instances in another one to compensate.

Better availability. Amazon EC2 Auto Scaling helps ensure that your application always has the right amount of capacity to handle the current traffic demand.

Better cost management. Amazon EC2 Auto Scaling can dynamically increase and decrease capacity as needed. Because you pay for the EC2 instances you use, you save money by launching instances when they are needed and terminating them when they aren't.

Amazon EC2 Auto Scaling attempts to distribute instances evenly between the Availability Zones that are enabled for your Auto Scaling group. 

# Auto Scaling Lifecycle

## Scale Out

When a scale-out event occurs, the Auto Scaling group launches the required number of EC2 instances, using its assigned launch configuration. These instances start in the Pending state. If you add a lifecycle hook to your Auto Scaling group, you can perform a custom action here. 

## Instances In Service

## Scale In

When a scale-in event occurs, the Auto Scaling group detaches one or more instances. The Auto Scaling group uses its termination policy to determine which instances to terminate. Instances that are in the process of detaching from the Auto Scaling group and shutting down enter the Terminating state, and can't be put back into service.


## Attach an Instance

You can attach a running EC2 instance that meets certain criteria to your Auto Scaling group. After the instance is attached, it is managed as part of the Auto Scaling group.

## Detach an Instance

You can detach an instance from your Auto Scaling group. After the instance is detached, you can manage it separately from the Auto Scaling group or attach it to a different Auto Scaling group.

# Lifecycle Hooks

You can add a lifecycle hook to your Auto Scaling group so that you can perform custom actions when instances launch or terminate.

# Enter and Exit Standby

You can put any instance that is in an InService state into a Standby state. This enables you to remove the instance from service, troubleshoot or make changes to it, and then put it back into service.

Instances in a Standby state continue to be managed by the Auto Scaling group. However, they are not an active part of your application until you put them back into service.

Amazon EC2 Auto Scaling that you can configure by using launch templates, launch templates provide more advanced Amazon EC2 configuration options. For example, you must use launch templates to use Amazon EC2 Dedicated Hosts. 

A launch template is similar to a launch configuration, in that it specifies instance configuration information. Included are the ID of the Amazon Machine Image (AMI), the instance type, a key pair, security groups, and the other parameters that you use to launch EC2 instances. However, defining a launch template instead of a launch configuration allows you to have multiple versions of a template. With versioning, you can create a subset of the full set of parameters and then reuse it to create other templates or template versions.
To create a launch template to use with an Auto Scaling group, create the template from scratch, create a new version of an existing template, or copy the parameters from a launch configuration, running instance, or other template.
Launch templates give you the flexibility of launching one type of instance, or a combination of instance types and On-Demand and Spot purchase options.
You cannot specify multiple network interfaces.
You cannot assign specific private IP addresses. When an instance launches, a private address is allocated from the CIDR range of the subnet in which the instance is launched

You cannot use host placement affinity or target a specific host by choosing a host ID.

# Launch Configurations

A launch configuration is an instance configuration template that an Auto Scaling group uses to launch EC2 instances. When you create a launch configuration, you specify information for the instances. Include the ID of the Amazon Machine Image (AMI), the instance type, a key pair, one or more security groups, and a block device mapping. If you've launched an EC2 instance before, you specified the same information in order to launch the instance.

You can specify your launch configuration with multiple Auto Scaling groups. However, you can only specify one launch configuration for an Auto Scaling group at a time, and you can't modify a launch configuration after you've created it. To change the launch configuration for an Auto Scaling group, you must create a launch configuration and then update your Auto Scaling group with it.

Keep in mind that whenever you create an Auto Scaling group, you must specify a launch configuration, a launch template, or an EC2 instance. When you create an Auto Scaling group using an EC2 instance, Amazon EC2 Auto Scaling automatically creates a launch configuration for you and associates it with the Auto Scaling group


# Auto Scaling Groups

An Auto Scaling group contains a collection of Amazon EC2 instances that are treated as a logical grouping for the purposes of automatic scaling and management. An Auto Scaling group also enables you to use Amazon EC2 Auto Scaling features such as health check replacements and scaling policies. Both maintaining the number of instances in an Auto Scaling group and automatic scaling are the core functionality of the Amazon EC2 Auto Scaling service.
An Auto Scaling group can launch On-Demand Instances, Spot Instances, or both. You can specify multiple purchase options for your Auto Scaling group only when you configure the group to use a launch template. (We recommend that you use launch templates instead of launch configurations to make sure that you can use the latest features of Amazon EC2.)

# Auto Scaling Groups with Multiple Instance Types and Purchase Options

When you configure the Auto Scaling group, you can:
* Assign each instance type an individual weight. Doing so might be useful, for example, if the instance types offer different vCPU, memory, storage, or network bandwidth capabilities.
* Specify how much On-Demand and Spot capacity to launch, and specify an optional On-Demand base portion.
* Prioritize instance types that can benefit from Reserved Instance or Savings Plan discount pricing.
* Define how Amazon EC2 Auto Scaling should distribute your Spot capacity across instance types
* Choose one or more instance types for the group (optionally overriding the instance type that is specified by the launch template).


# Allocation Strategies

In each case, Amazon EC2 Auto Scaling first tries to ensure that your instances are evenly balanced across the Availability Zones that you specified. Then, it launches instance types according to the allocation strategy that is specified.

## On-Demand Instances

The allocation strategy for On-Demand Instances is prioritized.

## Spot Instances

Amazon EC2 Auto Scaling provides two types of allocation strategies that can be used for Spot Instances:

### capacity-optimized

Amazon EC2 Auto Scaling allocates your instances from the Spot Instance pool with optimal capacity for the number of instances that are launching. Deploying in this way helps you make the most efficient use of spare EC2 capacity.

With Spot Instances, the pricing changes slowly over time based on long-term trends in supply and demand, but capacity fluctuates in real time. 
The capacity-optimized strategy automatically launches Spot Instances into the most available pools by looking at real-time capacity data and predicting which are the most available. 
This works well for workloads such as **big data and analytics, image and media rendering, and machine learning**. It also works well for **high performance computing that may have a higher cost of interruption associated with restarting work and checkpointing**. By offering the possibility of **fewer interruptions, the capacity-optimized strategy can lower the overall cost of your workload.**


### lowest-price

Amazon EC2 Auto Scaling allocates your instances from the number (N) of Spot Instance pools that you specify and from the pools with the lowest price per unit at the time of fulfillment.

For example, if you specify 4 instance types and 4 Availability Zones, your Auto Scaling group can potentially draw from as many as 16 different Spot Instance pools. If you specify 2 Spot pools (N=2) for the allocation strategy, your Auto Scaling group can fulfill Spot capacity from a minimum of 8 different Spot pools where the price per unit is the lowest.


# Instance Weighting for Amazon EC2 Auto Scaling

When you configure an Auto Scaling group to launch multiple instance types, you have the option of defining the number of capacity units that each instance contributes to the desired capacity of the group, using instance weighting. This allows you to specify the relative weight of each instance type in a way that directly maps to the performance of your application.


# Tagging Auto Scaling Groups and Instances

* you can propagate the tags from the Auto Scaling group to the Amazon EC2 instances it launches. Tagging your instances enables you to see instance cost allocation by tag in your AWS bill. 
* control which IAM users and groups in your account have permission to create, edit, or delete tags however, that a policy that restricts your users from performing a tagging operation on an Auto Scaling group does not prevent them from manually changing the tags on the instances after they have launched. 
* Tags are not propagated to Amazon EBS volumes. To add tags to Amazon EBS volumes, specify the tags in a launch template


# Elastic Load Balancing and Amazon EC2 Auto Scaling

* You can use Elastic Load Balancing to manage incoming requests by optimally routing traffic so that no one instance is overwhelmed.
* you attach the load balancer to your Auto Scaling group to register the group with the load balancer.
* Your load balancer acts as a single point of contact for all incoming web traffic to your Auto Scaling group. 
* When you use Elastic Load Balancing with your Auto Scaling group, it's not necessary to register your EC2 instances with the load balancer. Instances that are launched by your Auto Scaling group are automatically registered with the load balancer. Likewise, instances that are terminated by your Auto Scaling group are automatically deregistered from the load balancer.
* you can configure your Auto Scaling group to use Elastic Load Balancing metrics such as the request count per target (or other metrics) to scale the number of instances in the group as the demand on your instances changes.
* You can also optionally enable Elastic Load Balancing health checks to check the health of instances in your Auto Scaling group based on health checks provided by Elastic Load Balancing.

When you create a launch configuration or launch template to launch Spot Instances instead of On-Demand Instances, keep the following considerations in mind:

* Setting your maximum price 
* Changing your maximum price
* Maintaining your Spot Instances
* Balancing across Availability Zones
* Spot Instance termination
* Spot interruption notices

You can use Spot Instance interruption notices to monitor the status of your Spot Instances. For example, you can set up a rule in **Amazon EventBridge that automatically sends the EC2 Spot two-minute warning to an Amazon SNS topic**, an AWS Lambda function, or another target. 


# Scaling the Size of Your Auto Scaling Group

## Scaling Options

Amazon EC2 Auto Scaling provides several ways for you to scale your Auto Scaling group.

## Maintain current instance levels at all times

You can configure your Auto Scaling group to **maintain a specified number of running instances at all times**. To maintain the current instance levels

# Scale manually

you specify only the change in the **maximum, minimum, or desired capacity of your Auto Scaling group**. Amazon EC2 Auto Scaling manages the process of creating or terminating instances to maintain the updated capacity. For more information, see Manual Scaling for Amazon EC2 Auto Scaling.

# Scale based on a schedule

Scaling by schedule means that scaling actions are performed automatically as a function of time and date. This is useful when you know exactly when to increase or decrease the number of instances in your group, simply because the need arises on a predictable schedule. 

# Scale based on demand

A more advanced way to scale your resources, using scaling policies, lets you define **parameters that control the scaling process**. For example, let's say that you have a web application that currently runs on two instances and you want the CPU utilization of the Auto Scaling group to stay at around 50 percent when the load on the application changes. This method is useful for scaling in response to changing conditions, when you don't know when those conditions will change. You can set up Amazon EC2 Auto Scaling to respond for you.

# Use predictive scaling

You can also use Amazon EC2 Auto Scaling in combination with AWS Auto Scaling to scale resources across multiple services. AWS Auto Scaling can help you maintain optimal availability and performance by combining predictive scaling and dynamic scaling (proactive and reactive approaches, respectively) to scale your Amazon EC2 capacity faster. 

# Scaling Cooldowns 

When you use simple scaling, after the Auto Scaling group scales using a simple scaling policy, it waits for a cooldown period to complete before any further scaling activities due to simple scaling policies can start. An adequate cooldown period helps to prevent the initiation of an additional scaling activity based on stale metrics. 

# Conditions when auto scaling does not wait for cooldown period

* when a scheduled action starts at the scheduled time
* when scaling activities due to target tracking or step scaling policies start
* if an instance becomes unhealthy, Amazon EC2 Auto Scaling also does not wait for the cooldown period to complete before replacing the unhealthy instance.
* When you manually scale your Auto Scaling group, the default is not to wait for the cooldown period to complete, but you can override this behavior and honor the cooldown period when you call the API.

A default cooldown period automatically applies to any scaling activities for simple scaling policies, and you can optionally request to have it apply to your manual scaling activities. You can configure the length of time based on your instance startup time or other application needs

# Scaling-Specific Cooldown Period

One common use for a scaling-specific cooldown period is with a scale-in policy. Because this policy terminates instances, Amazon EC2 Auto Scaling needs less time to determine whether to terminate additional instances. Terminating instances should be a much quicker operation than launching instances. The default cooldown period of 300 seconds is therefore too long. In this case, a scaling-specific cooldown period with a lower value of 180 seconds for your scale-in policy can help you reduce costs by allowing the group to scale in faster.

With multiple instances, the cooldown period (either the default cooldown or the scaling-specific cooldown) takes effect starting when the last instance finishes launching or terminating.

# Scheduled Scaling

Scheduled scaling allows you to set your own scaling schedule. For example, let's say that every week the traffic to your web application starts to increase on Wednesday, remains high on Thursday, and starts to decrease on Friday. You can plan your scaling actions based on the predictable traffic patterns of your web application. Scaling actions are performed automatically as a function of time and date.

# Auto Scaling Instance Termination

Amazon EC2 Auto Scaling first identifies which of the two types (Spot or On-Demand) should be terminated. It then applies the termination policy in each Availability Zone individually, and identifies which instance (within the identified purchase option) in which Availability Zone to terminate that will result in the Availability Zones being most balanced. The same principles apply to Auto Scaling groups that use a mixed instances configuration with weights defined for the instance types.

Amazon EC2 Auto Scaling supports the following termination policies:

**Default** - Terminate instances according to the default termination policy. This policy is useful when you want your Spot allocation strategy evaluated before any other policy, so that every time your Spot instances are terminated or replaced, you continue to make use of Spot Instances in the optimal pools. It is also useful, for example, when you want to move off launch configurations and start using launch templates.

**AllocationStrategy** - Terminate instances in the Auto Scaling group to align the remaining instances to the allocation strategy for the type of instance that is terminating (either a Spot Instance or an On-Demand Instance). This policy is useful when your preferred instance types have changed. If the Spot allocation strategy is lowest-price, you can gradually rebalance the distribution of Spot Instances across your N lowest priced Spot pools. If the Spot allocation strategy is capacity-optimized, you can gradually rebalance the distribution of Spot Instances across Spot pools where there is more available Spot capacity. You can also gradually replace On-Demand Instances of a lower priority type with On-Demand Instances of a higher priority type.

**OldestLaunchTemplate** - Terminate instances that have the oldest launch template. With this policy, instances that use the noncurrent launch template are terminated first, followed by instances that use the oldest version of the current launch template. This policy is useful when you're updating a group and phasing out the instances from a previous configuration.

**OldestLaunchConfiguration** - Terminate instances that have the oldest launch configuration. This policy is useful when you're updating a group and phasing out the instances from a previous configuration.

**ClosestToNextInstanceHour** - Terminate instances that are closest to the next billing hour. This policy helps you maximize the use of your instances that have an hourly charge.

**NewestInstance** - Terminate the newest instance in the group. This policy is useful when you're testing a new launch configuration but don't want to keep it in production.

**OldestInstance** - Terminate the oldest instance in the group. This option is useful when you're upgrading the instances in the Auto Scaling group to a new EC2 instance type. You can gradually replace instances of the old type with instances of the new type.

# Instance Scale-In Protection

To control whether an Auto Scaling group can terminate a particular instance when scaling in, use instance scale-in protection. You can enable the instance scale-in protection setting on an Auto Scaling group or on an individual Auto Scaling instance. When the Auto Scaling group launches an instance, it inherits the instance scale-in protection setting of the Auto Scaling group. You can change the instance scale-in protection setting for an Auto Scaling group or an Auto Scaling instance at any time.

Instance scale-in protection starts when the instance state is InService. If you detach an instance that is protected from termination, its instance scale-in protection setting is lost. When you attach the instance to the group again, it inherits the current instance scale-in protection setting of the group.

If all instances in an Auto Scaling group are protected from termination during scale in, and a scale-in event occurs, its desired capacity is decremented. However, the Auto Scaling group can't terminate the required number of instances until their instance scale-in protection settings are disabled.

Instance scale-in protection does not protect Auto Scaling instances from the following:

* Manual termination through the Amazon EC2 consolej
* Health check replacement if the instance fails health check, so prevent Amazon EC2 Auto Scaling from terminating unhealthy instances, suspend the ReplaceUnhealthy process. 
* Spot Instance interruptions. 


# Amazon EC2 Auto Scaling Lifecycle Hooks

Lifecycle hooks enable you to perform custom actions by pausing instances as an Auto Scaling group launches or terminates them.

For example, let's say that your newly launched instance completes its startup sequence and a lifecycle hook pauses the instance. While the instance is in a wait state, you can install or configure software on it, making sure that your instance is fully ready before it starts receiving traffic. For another example of the use of lifecycle hooks, let's say that when a scale-in event occurs, the terminating instance is first deregistered from the load balancer (if the Auto Scaling group is being used with Elastic Load Balancing). Then, a lifecycle hook pauses the instance before it is terminated. While the instance is in the wait state, you can, for example, connect to the instance and download logs or other data before the instance is fully terminated.

You can add a lifecycle hook to an Auto Scaling group that triggers a notification when an instance enters a wait state.

# Temporarily Removing Instances

You can put an instance that is in the InService state into the Standby state, update or troubleshoot the instance, and then return the instance to service. Instances that are on standby are still part of the Auto Scaling group, but they do not actively handle application traffic.

For example, you can change the launch configuration for an Auto Scaling group at any time, and any subsequent instances that the Auto Scaling group launches use this configuration. However, the Auto Scaling group does not update the instances that are currently in service. You can terminate these instances and let the Auto Scaling group replace them. Or, you can put the instances on standby, update the software, and then put the instances back in service.


# Health Status of an Instance in a Standby State


Amazon EC2 Auto Scaling does not perform health checks on instances that are in a standby state. While the instance is in a standby state, its health status reflects the status that it had before you put it on standby. Amazon EC2 Auto Scaling does not perform a health check on the instance until you put it back in service.


# Automating Amazon EC2 Auto Scaling with EventBridge

Amazon EventBridge, formerly called CloudWatch Events, lets you automate AWS services and respond to system events such as application availability issues or resource changes. Events from AWS services are delivered to EventBridge in near real time. Based on the rules that you create, EventBridge invokes one or more target actions when an event matches the values that you specify in a rule. The actions that can be automatically triggered include the following:

* Invoking an AWS Lambda function
* Invoking Amazon EC2 Run Command
* Relaying the event to Amazon Kinesis Data Streams
* Activating an AWS Step Functions state machine
* Notifying an Amazon SNS topic or an Amazon SQS queue


# Service linked roles


Amazon EC2 Auto Scaling uses service-linked roles for the permissions that it requires to call other AWS services on your behalf. A service-linked role is a unique type of IAM role that is linked directly to an AWS service.


# Private connection using VPC Endpoint

You can establish a private connection between your virtual private cloud (VPC) and the Amazon EC2 Auto Scaling API by creating an interface VPC endpoint. You can use this connection to call the Amazon EC2 Auto Scaling API from your VPC without sending traffic over the internet. The endpoint provides reliable, scalable connectivity to the Amazon EC2 Auto Scaling API. It does this without requiring an internet gateway, NAT instance, or VPN connection.

