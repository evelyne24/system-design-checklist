---
layout: default
title: Network
parent: System layers
permalink: /layers/network
nav_order: 1
---

{% include toc.md %}

## Network layer
{: .no_toc }

### Determine the <mark>network types</mark> the system has.

- [ ] In a Virtual Private Cloud (VPC) on a cloud platform like AWS, GCP, Azure etc.
- [ ] In an on-premise network, a dedicated private data centre
- [ ] Hybrid, both in the cloud and on premise

### Determine if data moves from <mark>on-premise to cloud</mark> and vice-vera.

- [ ]  Data is transferred from on-premise to the cloud
- [ ]  Data is transferred from the cloud to on premise

### Determine what sort of <mark>network topology</mark> the system has. 

For example, if multiple VPCs and on premise networks communicate with each other.

- [ ]  Hub and spikes topology (all VPCs and on premise communicate via a central hub)
- [ ]  Mesh topology (VPCs and on premise communicate point-to-point)

### Determine whether to <mark>geographically restrict data</mark>. 

For example, because of regulatory standards.

- [ ]  Data needs to be stored only in certain regions.
- [ ]  Data can be stored anywhere in the world.

### Design a <mark>highly-available VPC</mark>.

- [ ] Determine if your VPC supports IPv4 or IPv6 addresses
- [ ] Specify the range of IP addresses for the VPC, in Classless Inter-Domain Routing (CIDR) block
- [ ] Divide this IP address space equally into multiple availability zones (AZ)

### Determine the <mark>private and public subnets</mark>.

<div class="note" markdown="1">

There are **three reasons** to create multiple subnets:

1. To separate publicly-facing hosts (e.g. web servers) from the private ones (e.g. databases)
2. To support multiple availability zones, since a single subnet cannot span multi-AZ
3. To group some IP addresses (e.g. databases with PII) under the same Network Access Control List (NACL)

</div>

  - [ ] Create the public subnets
  - [ ] Create the private subnets

### Configure <mark>SGs</mark> for each instance.

<div class="note" markdown="1">

**Security Groups (SGs)** are the first layer of defence. 

Security Group are _stateful_, meaning that changes in inbound rules are reflected automatically in the outbound ones. For example, if you allow inbound traffic on a certain port, outbound traffic is automatically allowed as well. Security Groups only support _Allow_ rules. 

Security Groups are _applied to individual instances_ (i.e. virtual server) in a subnet.

</div>

Important protocol and ports to consider in a typical web application:

  - [ ] TCP port 80 for IPv4 and/or IPv6 (HTTP)
  - [ ] TCP port 443 for IPv4 and/or IPv6 (HTTPS)
  - [ ] TCP port 3306 (MySQL) or 5432 (PostgreSQL) etc. for databases
  - [ ] TCP port 22 for SSH (Linux) or 3389 for RDP (Windows)
  - [ ] ICMP (ping)

### Configure <mark>NACLs</mark> for each subnet.

<div class="note" markdown="1">

**Network Access Control Lists (NACLs)** are the second layer of defence.

NACL are _stateless_, meaning that the outbound and inbound rules are different. For example, you need to specify separately the rule for inbound and outbound traffic on certain ports. NACL support both _Allow_ and _Deny_ rules. Default is to deny all access.

NACL are _applied to all instances_ in a subnet.

</div>

  - [ ] Configure inbound rules
  - [ ] Configure outbound rules

### Configure the <mark>routing tables and gateways</mark>.

<div class="note" markdown="1">

The **Internet Gateway** connects the VPC to the Internet. Without one, no Internet traffic comes in or goes out.

The **Network Address Translation (NAT) Gateway** is like an apartment building with a doorman. People from the outside can mail you packages to the address of your building, without knowing which apartment you live in. Your doorman will then route that package to your apartment. NAT is used to route traffic to instances in a private subnet without exposing their IP addresses.

</div>

  - [ ] Configure the main routing table (default for each subnet)
  - [ ] Configure the routing tables for private and public subnets
  - [ ] Configure a route to the Internet Gateway
  - [ ] Configure a route to the NAT Gateway

### Determine <mark>extra network considerations</mark>.

  - [ ] Enable VPC flow logs to capture information about inbound and outbound traffic
  - [ ] Setup VPC endpoints to route traffic from your web servers through the private network to other cloud services, e.g. in AWS to S3 or DynamoDB


## Domain Name Server (DNS)
{: .no_toc }

<div class="info" markdown="1">
🙃 Suspicious system failure? Blame the DNS.
</div>

### Decide how to <mark>route traffic</mark> to the system.
- [ ]  _Simple routing_: route traffic to a single source, for example a web server or LB.
- [ ]  _Failover routing_: route traffic in an active/passive failover configuration
- [ ]  _Geolocation routing_: route traffic based on the location of the users
- [ ]  _Geoproximity routing_: route traffic based on the location of your resources
- [ ]  _Latency-based routing_: route traffic to the region with the best latency
- [ ]  _Weighted routing_: route traffic to multiple sources based on percentages


### Choose good <mark>TTLs</mark> for DNS records.

<div class="note" markdown="1">

**Time-To-Live (TTL)** controls how the DNS Resolver caches locally the IP address associated with your domain. Caching reduces latency by avoiding the `DNS Resolver → Route Nameserver → Top-Level Domain Nameserver → Authoritative Nameserver → DNS Resolver` round-trip.

Longer caching results in _faster responses_ and lower traffic but _hinders scalability_. For example, a Load Balancer (LB) provides one or more IP addresses to a client. When the LB scales, it remaps these IP addresses. TTL determines how much time the client waits for the DNS cache to expire before it can see the new IP addresses and can send traffic to those.

AWS recommends a Global Accelerator (GA) when setting up an Elastic Load Balancer (ELB). The GA provides two static IPv4 addresses. A static IP can be associated to an ELB to solve the DNS TTL issue. This comes with certain costs.

</div>