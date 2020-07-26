---
title: "Site to site vpn"
date: 2020-07-26T09:13:29+07:00
---

# What is AWS Site-to-Site VPN?

By default, instances that you launch into an Amazon VPC can't communicate with your own (remote) network. You can enable access to your remote network from your VPC by creating an AWS Site-to-Site VPN (Site-to-Site VPN) connection, and configuring routing to pass traffic through the connection.
connection between your VPC and your own on-premises network. Site-to-Site VPN supports Internet Protocol security (IPsec) VPN connections.

# Concepts

The following are the key concepts for Site-to-Site VPN:

**VPN connection** :A secure connection between your on-premises equipment and your VPCs.

**VPN tunnel**: An encrypted link where data can pass from the customer network to or from AWS.

Each VPN connection includes two VPN tunnels which you can simultaneously use for high availability.

**Customer gateway**: An AWS resource which provides information to AWS about your customer gateway device.

**Customer gateway device**: A physical device or software application on your side of the Site-to-Site VPN connection.


# Site-to-Site VPN limitations

A Site-to-Site VPN connection has the following limitations.

* IPv6 traffic is not supported.
* An AWS VPN connection does not support Path MTU Discovery.
* In addition, take the following into consideration when you use Site-to-Site VPN.
When connecting your VPCs to a common on-premises network, we recommend that you use **non-overlapping CIDR blocks for your networks**.

You can use pre-shared keys, or certificates to authenticate your Site-to-Site VPN tunnel endpoints.

# Pre-shared keys

A pre-shared key is the default authentication option.
A pre-shared key is a Site-to-Site VPN tunnel option that you can specify when you create a Site-to-Site VPN tunnel.
A pre-shared key is a string that you enter when you configure your customer gateway device. If you do not specify a string, we auto-generate one for you.

# Private certificate from AWS

you can use a private certificate from AWS Certificate Manager Private Certificate Authority to authenticate your VPN.
You must create a service-link role to generate and use the certificate for the AWS side of the Site-to-Site VPN tunnel endpoint.
After you generate the private certificate, you specify the certificate when you create the customer gateway, and then apply it to your customer gateway device.
If you do not specify the IP address of your customer gateway device, we do not check the IP address. This operation allows you to move the customer gateway device to a different IP address without having to re-configure the VPN connection.

# Accelerated Site-to-Site VPN connections

You can optionally enable acceleration for your Site-to-Site VPN connection. An accelerated Site-to-Site VPN connection (accelerated VPN connection) uses AWS Global Accelerator to route traffic from your on-premises network to an AWS edge location that is closest to your customer gateway device. By default, when you create a Site-to-Site VPN connection, acceleration is disabled. 

# Rules and restrictions

Acceleration is only supported for Site-to-Site VPN connections that are attached to a transit gateway. Virtual private gateways do not support accelerated VPN connections.
You cannot enable or disable acceleration for an existing Site-to-Site VPN connection. Instead, you can create a new Site-to-Site VPN connection with acceleration enabled or disabled as needed. 
Site-to-Site VPN connections that use certificate-based authentication might not be compatible with AWS Global Accelerator, due to limited support for packet fragmentation in Global Accelerator. 

# Static and dynamic routing

If your customer gateway device supports Border Gateway Protocol (BGP), specify dynamic routing 
If your customer gateway device does not support BGP, specify static routing.
We recommend that you use BGP-capable devices, when available, because the BGP protocol offers robust liveness detection checks that can assist failover to the second VPN tunnel if the first tunnel goes down. Devices that don't support BGP may also perform health checks to assist failover to the second tunnel when needed.

