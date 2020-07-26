---
title: "Xray"
date: 2020-07-26T19:59:36+07:00
draft: false
---

# What is AWS X-Ray?

AWS X-Ray is a service that **collects data about requests that your application serves, and provides tools you can use to view, filter, and gain insights into that data to identify issues and opportunities for optimization**. 

# The X-Ray SDK provides:

* **Interceptors** to add to your code to trace incoming HTTP requests
* **Client handlers to instrument AWS SDK clients** that your application uses to call other AWS services
* An **HTTP client** to use to instrument calls to other internal and external HTTP web services

Instead of sending trace data directly to X-Ray, the SDK sends JSON segment documents to a daemon process listening for UDP traffic. The X-Ray daemon buffers segments in a queue and uploads them to X-Ray in batches. 

X-Ray uses trace data from the AWS resources that power your cloud applications to generate a detailed **service graph**. The service graph shows the client, your front-end service, and backend services that your front-end service calls to process requests and persist data. Use the service graph to identify bottlenecks, latency spikes, and other issues to solve to improve the performance of your applications.

AWS X-Ray receives data from services as segments. X-Ray then groups segments that have a common request into traces. X-Ray processes the traces to generate a service graph that provides a visual representation of your application.

# [Concepts](https://docs.aws.amazon.com/xray/latest/devguide/xray-concepts.html)

* Segments
* Subsegments
* Service graph
* Traces
* Sampling
* Tracing header
* Filter expressions
* Groups
* Annotations and metadata
* Errors, faults, and exceptions

# Data protection in AWS X-Ray

AWS X-Ray always encrypts traces and related data at rest. When you need to audit and disable encryption keys for compliance or internal requirements, you can configure X-Ray to use an AWS Key Management Service (AWS KMS) customer master key (CMK) to encrypt data.


