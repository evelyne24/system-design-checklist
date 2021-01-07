---
layout: default
title: Load balancers
parent: System layers
permalink: /layers/lb
nav_order: 3
---

{% include toc.md %}

## Load Balancer (LB)
{: .no_toc }

### Determine <mark>where in the system you need load balancing</mark>.

<div class="note" markdown="1">

| ![Load balancer examples]({{ "/assets/images/load-balancers-examples.jpg" | absolute_url }} ) |
| :---: |
| _Load balancer examples_ |


**Load Balancers (LBs)** are _reverse proxies_. A reverse proxy is a server that sits between clients and servers and acts on behalf of the servers. One that acts on behalf of the client is called _forward proxy_. 

**Jobs of LB**

- _Accepts incoming traffic via one or more listeners_.

  A listeners is a process that checks for connection request configured with a protocol and port number for connections from clients to the LB (frontends) and from the LB to its registered targets like _instances_, _IP addresses_, _containers_ (backends).

- _Monitors the health of its registered targets and routes traffic only to healthy targets_.

  When it detects an unhealthy target, it stops routing traffic to that target. It also handles _connection draining_, so that in-flight requests have time to terminate when the target is deregistering or unhealthy. This is useful for seamlessly doing maintenance tasks like software upgrades with minimal disruption

- _Monitors metrics such as throughput, CPU and memory utilisation of its targets_.

- _Dynamically scales itself and its backends, based on auto-scaling rules_.

  An Elastic Load Balancer (ELB) is designed to be _highly-available_ across multiple AZs and _abstracts away scaling itself_.

- _Can handle connection multiplexing_.

  Requests from multiple clients on multiple front-end connections can be routed to a given target through a single backend connection.

- _Can offload user authentication, including federated providers_.

- _Can offloads encryption and decryption (**SSL termination**) from its registered targets_.

**Benefits of LB**

- Makes the system more fault-tolerant
- Actively monitors the system
- Is a line of defence against network attacks
- Improves latency and reduces the load on its targets

**How LB routes requests**

1. The client resolves the LB's domain name using a DNS server. DNS servers return one or more IP addresses of the LB nodes.

   These IP addresses can be remapped quickly in response to changing traffic. The DNS entry also specifies the TTL (e.g. 60 seconds). For lengthy operations, such as file uploads, the idle timeout for connections should be adjusted to ensure they have time to complete.

2. The client determines which IP address to use to send requests to the load balancer.

3. The LB node receives the request, selects a healthy registered target and sends the request to the target using its private IP address.

**How LB selects a target**

1. _Least Connection Method_ or _Least Outstanding Requests (LOR)_

   Directs traffic to the server with the fewest active connections (or pending, unfinished requests). Useful when there are a large number of persistent client connections which are unevenly distributed between the servers. AWS has recently introduced LOR support for ALB.

2. _Least Response Time Method_

   Directs traffic to the server with the fewest active connections and the lowest average response time.

3. _Least Bandwidth Method_

   Selects the server that is currently serving the least amount of traffic measured in megabits per second (Mbps).

4. _Round Robin Method_

   Cycles through a list of servers and sends each new request to the next server. When it reaches the end of the list, it starts over at the beginning. It is most useful when the servers are of equal specification and there are not many persistent connections. _Round Robin_ is the most commonly used algorithm.

5. _Weighted Round Robin Method_

   Handles servers with different processing capacities. Each server is assigned a weight (an integer value that indicates the processing capacity). Servers with higher weights receive new connections before those with less weights and servers with higher weights get more connections than those with less weights.

6) _IP Hash_: a hash of the IP address of the client is calculated to redirect the request to a server.

**LB alternative**

An alternative to classic load balancers is a **service mesh architecture**, explained in [Define your API](/api). A service mesh improves _service-to-service communication_.

A service mesh is a separate entity that manages each inbound and outbound request to control security (encryption), identity, observability (logging, tracing), error handling and load balancing outside application code.

It is implemented as a proxy server which runs alongside each replica of a service, on the same VM, host, pod etc.

A service mesh architecture is suitable for organisations where all teams are aligned on their architecture, have control over all the services (i.e. no external third-party services) and are able to share the same Certificate Authority (CA) between services (although multiple CAs can be used).

</div>

- [ ] DNS load balancing, see [DNS routing options](/layers/network/dns)
- [ ] An external LB for web servers
- [ ] An internal LB for application servers
- [ ] An internal LB for databases
- [ ] A self-balancing service mesh architecture

### Determine <mark>the OSI layer the LB operates at</mark>.

| ![Open Systems Interconnection Model](https://www.cloudflare.com/img/learning/ddos/what-is-a-ddos-attack/osi-model-7-layers.svg) |
| :---: |
| Open Systems Interconnection Model |

- [ ] Network Load Balancer (NLB) at Layer 4

  - For _ultra-low latency_ and _high throughput_ handles millions of requests per second
  - Handles _sudden volatile traffic_ patterns
  - Supports _static Elastic IP address_, useful for _IP whitelisting_
  - Supports _UDP_ for IoT, gaming, streaming, media transfers

- [ ] Application Load Balancer (ALB) at Layer 7

  - Has _visibility over the HTTP request_ to create custom routing based on request headers, query params, source IP CIDR etc.
  - Serve _multiple domains_ with SNI and custom routing based on path
  - Supports _session affinity_ (sticky sessions) to route request from the same client to the same target
  - Can send a fixed response from the ALB, without a backend
  - Supports _offloading user authentication_ to the ALB
  - Can _perform redirects directly_ from the ALB, e.g. `HTTP` → `HTTPS`
  - Supports _slow start_ to give new targets some time to warm up before receiving requests
  - Supports _Lambdas_ as targets

### Setup <mark>health checks for targets</mark>.

<div class="note" markdown="1">

The LB periodically pings its targets to check their health and avoid sending traffic to unhealthy instances. A _health check_ can be configured with:

- a protocol, e.g. HTTP or HTTPs
- a port
- a path, e.g. `/healthy`
- an interval between checks
- a threshold to count the number of consecutive successful checks
- a timeout for failed checks
- a success status code

</div>

### Setup <mark>auto-scaling for target groups</mark>.

| ![LB auto scaling targets]({{ "assets/images/load-balancer-targets.png" | absolute_url }}) |
| :---: |
| *LB auto scaling targets* |


<div class="note" markdown="1">

Multiple targets (instances, IP addresses, containers etc.) can be grouped together into a _Target Group_, for easier configuration. Each target group can have _auto-scaling policies_ for scaling in and out, that specify how to add and remove instances when demand changes. A LB can route requests to multiple Target Groups.

Auto-scaling provides _high-availability_ e.g. across AZs and redundancy. It also performs status checks on instances which can be combined with the health checks from the LB.

</div>

- [ ] Setup steady scaling with a minimum and desired count of targets
- [ ] Setup scheduled scaling for predictable load changes, e.g. daily peaks or seasonal
- [ ] Setup dynamic scaling in response of metrics like CPU utilisation above a threshold


### Check <mark>if logging and monitoring is enabled</mark>.

- [ ] Enable metrics
- [ ] Enable access logs
- [ ] Enable request tracing
