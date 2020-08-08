---
title: "Cloudfront"
date: 2020-08-08T07:57:24+07:00
---

# CloudFront 

Amazon CloudFront is a **web service that speeds up distribution of your static and dynamic web content, such as .html, .css, .js, and image files, to your users**. CloudFront delivers your content through a worldwide network of data centers called **edge locations**. When a user requests content that you're serving with CloudFront, the user is routed to the edge location that provides the **lowest latency (time delay)**, so that content is delivered with the best possible performance.

you can configure your **origin server to add headers to the files, to indicate how long you want the files to stay in the cache in CloudFront edge locations. By default, each file stays in an edge location for 24 hours before it expires**. The minimum expiration time is 0 seconds; there isn't a maximum expiration time. For more information

# Use Cases

* Accelerate Static Website Content Delivery
  cloud front will use the edge location to to serve the content
* **Serve Video On Demand or Live Streaming Video** For video on demand (VOD) streaming, you can use **CloudFront to stream in common formats such as MPEG DASH, Apple HLS, Microsoft Smooth Streaming, and CMAF, to any device**.
* You can use CF for **broadcasting**
* Encrypt Specific Fields Throughout System Processing use for **field level encryption**
* Customize at the Edge **customise data at the edge location**
* **Serve Private Content by using Lambda@Edge Customizations** use lambda functions to return customized responses

# How Regional Caches Work

* Regional edge caches are CloudFront locations that are **deployed globally, close to your viewers. They're located between your origin server and the POPs—global edge locations that serve content directly to viewers.**
* Regional edge caches have a larger cache than an individual POP, so objects remain in the cache longer at the nearest regional edge cache location. This helps keep more of your content closer to your viewers, reducing the need for CloudFront to go back to your origin server, and improving overall performance for viewers.
* When a viewer makes a request on your website or through your application, DNS routes the request to the POP that can best serve the user’s request. This location is typically the nearest CloudFront edge location in terms of latency. In the POP, CloudFront checks its cache for the requested files. If the files are in the cache, CloudFront returns them to the user. If the files are not in the cache, the POPs go to the nearest regional edge cache to fetch the object.
* In the regional edge cache location, CloudFront again checks its cache for the requested files. If the files are in the cache, CloudFront forwards the files to the POP that requested them. As soon as the first byte arrives from regional edge cache location, CloudFront begins to forward the files to the user. CloudFront also adds the files to the cache in the POP for the next time someone requests those files.

For files not cached at either the POP or the regional edge cache location, CloudFront compares the request with the specifications in your distributions and forwards the request for your files to the origin server. After your origin server sends the files back to the regional edge cache location, they are forwarded to the POP, and CloudFront forwards the files to the user. In this case, CloudFront also **adds the files to the cache in the regional edge cache location in addition to the POP for the next time a viewer requests those files. This makes sure that all of the POPs in a region share a local cache**, eliminating multiple requests to origin servers. CloudFront also keeps **persistent connections with origin servers so files are fetched from the origins as quickly as possible**.

**Proxy methods PUT/POST/PATCH/OPTIONS/DELETE go directly to the origin from the POPs and do not proxy through the regional edge caches.**

# Accessing CloudFront

* AWS Management Console
* AWS SDKs
* CloudFront API
* AWS Command Line Interface
* AWS Tools for Windows PowerShell
* RTMP distributions(will not be avilable after 31/12/2020 it's depricated)

# Using CloudFront Origin Groups

When you need **high availability**. Use origin failover to designate a **primary origin for CloudFront plus a second origin that CloudFront automatically switches to when the primary origin returns specific HTTP status code failure responses**.

**CloudFront can be more cost effective if your users access your objects frequently because, at higher usage, the price for CloudFront data transfer is lower than the price for Amazon S3 data transfer**. In addition, downloads are faster with CloudFront than with Amazon S3 alone because your objects are stored closer to your users.

# Creating Signed URLs for Restricted Content

If you have content that you want to **restrict access** to, **you can create signed URLs**. For example, **if you want to distribute your content only to users who have authenticated, you can create URLs that are valid only for a specified time period or that are available only from a specified IP address**. For more information, see Serving Private Content with Signed URLs and Signed Cookies.

# Secure Access and Restricting Access to Content

* Configure HTTPS connections
* Prevent users in specific geographic locations from accessing content
* Require users to access content using CloudFront signed URLs or signed cookies
* Set up field-level encryption for specific content fields
* Use AWS WAF to control access to your content

# Notes: 
HTTPS for Communication Between CloudFront and Your Amazon S3 Origin
you must change the value of Viewer Protocol Policy to Redirect HTTP to HTTPS or HTTPS Only. 

# Serving Private Content

You can control user access to your private content in two ways, as shown in the following illustration:

* Restrict access to files in CloudFront edge caches
* Restrict access to files in your origin by doing one of the following:
  - Set up an origin access identity (OAI) for your Amazon S3 bucket
  - Configure custom headers for a private HTTP server (a custom origin)

# Restrict access to files in CloudFront edge caches

When you create signed URLs or signed cookies to control access to your files, you can specify the following restrictions:

* An **ending date and time**, after which the URL is no longer valid.
* (Optional) The date and time that the URL becomes valid.
* (Optional) The IP address or range of addresses of the computers that can be used to access your content.

You must use **`RSA-SHA1` for signing URLs** or cookies. **CloudFront doesn't accept other algorithms**.

# Restrict access to files in your origin by doing one of the following:

You can optionally secure the content in your Amazon S3 bucket so that **users can access it through CloudFront but cannot access it directly by using Amazon S3 URLs**. This prevents someone from bypassing CloudFront and using the Amazon S3 URL to get content that you want to restrict access to. This step isn't required to use signed URLs, but we recommend it.

To require that users access your content through CloudFront URLs, you do the following tasks:

* Create a special CloudFront user called an **origin access identity(OAI)** and associate it with your CloudFront distribution.
* Give the **origin access identity permission to read the files in your bucket**.
* **Remove permission for anyone else to use Amazon S3 URLs to read the files**.

# Use signed URLs in the following cases:

* You want to use an RTMP distribution. Signed cookies aren't supported for RTMP distributions.
* You want to restrict access to individual files, for example, an installation download for your application.
* Your users are using a client (for example, a custom HTTP client) that doesn't support cookies.

# Use signed cookies in the following cases:

* You want to provide access to multiple restricted files, for example, all of the files for a video in HLS format or all of the files in the subscribers' area of website.
* You don't want to change your current URLs.



# Controlling origin requests

The origin request always includes the following information from the viewer request:

* The URL path (the path only, without URL query strings or the domain name)
* The request body (if there is one)
* The HTTP headers that CloudFront automatically includes in every origin request, including Host, User-Agent, and X-Amz-Cf-Id.

Origin request policies are separate from cache policies, which control the cache key. 
All URL query strings, HTTP headers, and cookies that you include in the cache key (using a cache policy) are automatically included in origin requests. 
Use the origin request policy to specify the information that you want to include in origin requests, but not include in the cache key. 
You can also use an origin request policy to add additional HTTP headers to an origin request that were not included in the viewer request. 
To use an origin request policy, the cache behavior must also use a cache policy. You cannot use an origin request policy in a cache behavior without a cache policy.


# Adding and Accessing Content That CloudFront Distributes

When you add a file that you want CloudFront to distribute confirm that the path pattern in the applicable cache behavior sends requests to the correct origin.
For example, suppose the path pattern for a cache behavior is *.html. If you don't have any other cache behaviors configured to forward requests to that origin, CloudFront will only forward *.html files. In this scenario, for example, CloudFront will never distribute .jpg files that you upload to the origin, because you haven't created a cache behavior that includes .jpg files.
**CloudFront servers don't determine the MIME type** for the objects that they serve. When you upload a file to your origin, we recommend that you set the Content-Type header field for it.

# Updating Existing Content with a CloudFront Distribution

There are two ways to update existing content that CloudFront is set up to distribute for you:
* Update files by using the same name
* Update by using a version identifier in the file name
* We recommend that you use a version identifier in file names or in folder names**, to help give you more control over managing the content that CloudFront serves.

# Invalidating Files

If you need to remove a file from CloudFront edge caches before it expires, you can do one of the following:
* Invalidate the file from edge caches. The next time a viewer requests the file, CloudFront returns to the origin to fetch the latest version of the file.
* Use file versioning to serve a different version of the file that has a different name. 

You cannot invalidate objects that are served by an RTMP distribution.

To invalidate files, you can specify either the path for individual files or a path that ends with the * wildcard, which might apply to one file or to many.

You can submit a certain number of invalidation paths each month for free. If you submit more than the allotted number of invalidation paths in a month, you pay a fee for each invalidation path that you submit. 

# Using AWS WAF to Control Access to Your Content

AWS WAF is a web application firewall that lets you monitor the HTTP and HTTPS requests that are forwarded to CloudFront, and lets you control access to your content. Based on conditions that you specify, such as the values of query strings or the IP addresses that requests originate from, CloudFront responds to requests either with the requested content or with an HTTP status code 403 (Forbidden). You can also configure CloudFront to return a custom error page when a request is blocked. For more information about AWS WAF, see the AWS WAF Developer Guide.
After you create an AWS WAF web access control list (web ACL), create or update a web distribution to associate the distribution with the web ACL. You can associate as many CloudFront distributions as you want with the same web ACL or with different web ACLs. For information about creating a web distribution and associating it with a web ACL, see Creating a Distribution.

# Restricting the Geographic Distribution of Your Content

**Use the CloudFront geo restriction feature**. Use this option to restrict access to all of the files that are associated with a distribution and to restrict access at the country level.
**Use a third-party geolocation service**. Use this option to restrict access to a subset of the files that are associated with a distribution or to restrict access at a finer granularity than the country level.

# Field-Level Encryption

Field-level encryption allows you to enable your users to securely upload sensitive information to your web servers. The sensitive information provided by your users is encrypted at the edge, close to the user, and remains encrypted throughout your entire application stack. This encryption ensures that only applications that need the data—and have the credentials to decrypt it—are able to do so.

To use field-level encryption, when you configure your CloudFront distribution, specify the set of fields in POST requests that you want to be encrypted, and the public key to use to encrypt them. You can encrypt up to 10 data fields in a request. (You can’t encrypt all of the data in a request with field-level encryption; you must specify individual fields to encrypt.)
**To use field-level encryption, your origin must support chunked encoding.**


# Optimizing Content Caching and Availability

**To deliver video on demand (VOD) streaming** with CloudFront, use the following services:

* Amazon S3 to store the content in its original format and to store the transcoded video.
* An encoder (such as AWS Elemental MediaConvert) to transcode the video into streaming formats.
* CloudFront to deliver the transcoded video to viewers. For Microsoft Smooth Streaming.

If you specify a web server running **Microsoft IIS** as your origin, **do not enable Smooth Streaming in the cache behaviors of your CloudFront distribution. CloudFront can’t use a Microsoft IIS server as an origin if you enable Smooth Streaming as a cache behavior**.

# Lambda@Edge

## Customizing Content at the Edge with Lambda@Edge

**Lambda@Edge is an extension of AWS Lambda**, a compute service that lets you execute functions that customize the content that CloudFront delivers. You can execute them in AWS locations globally that are **closer to the viewer**, without provisioning or managing servers. Lambda@Edge scales automatically, from a few requests per day to thousands per second. Processing requests at AWS locations closer to the viewer instead of on origin servers significantly reduces latency and improves the user experience.
You can **execute Lambda functions** when the following **CloudFront events occur**:

* When CloudFront receives a request from a viewer (viewer request)
* Before CloudFront forwards a request to the origin (origin request)
* When CloudFront receives a response from the origin (origin response)
* Before CloudFront returns the response to the viewer (viewer response)

## Testing and Debugging Lambda@Edge Functions

When you review CloudWatch log files or metrics when you're troubleshooting errors, be aware that they are displayed or stored in the Region closest to the location where the function executed. So, if you have a website or web application with users in the United Kingdom, and you have a Lambda function associated with your distribution, for example, you must change the Region to view the CloudWatch metrics or log files for the London AWS Region. 
You can use CloudWatch metrics to monitor, in real time, problems with your Lambda@Edge functions. You can also use CloudWatch Logs to get the function logs. There’s no additional charge for metrics or logs.
Lambda automatically sends function logs to CloudWatch Logs. You can access the log files using the CloudWatch console or the CloudWatch Logs API.
You can add triggers only to a numbered version, not to $LATEST.

## Deleting Lambda@Edge Functions and Replicas

You can delete a Lambda@Edge function only when the replicas of the function have been deleted by CloudFront. Replicas of a Lambda function are automatically deleted in the following situations:

After you remove the last association for the function from all of your CloudFront distributions. If more than one distribution uses a function, the replicas are deleted only after you remove the function association from the last distribution.
After you delete the last distribution that a function was associated with.
Replicas are typically deleted within a few hours. You cannot manually delete Lambda@Edge function replicas. This helps prevent a situation where a replica is deleted that is still in use, which would result in an error.

The CloudFront console includes a variety of reports about your CloudFront activity, including the following:

* CloudFront Cache Statistics Reports
* CloudFront Popular Objects Report
* CloudFront Top Referrers Report
* CloudFront Usage Reports
* CloudFront Viewers Reports

## Tracking Configuration Changes with AWS Config

You can use AWS Config to record configuration changes for CloudFront distribution settings changes. For example, you can capture changes to distribution states, price classes, origins, geo restriction settings, and Lambda@Edge configurations.

# Tagging Amazon CloudFront Distributions

* Use tags to track billing information in different categories.
* Use tags to enforce tag-based permissions on CloudFront distributions.

