---
layout: default
title: Application services
parent: System layers
permalink: /layers/application
nav_order: 4
---

{% include toc.md %}

## Application services
{: .no_toc }

### Define the <mark>type of client-server communication</mark>.

<div class="note" markdown="1">

**AJAX Polling, HTTP Long-Polling, Server-Sent Events, WebSockets**

  The first gen **AJAX apps** worked on the basic idea that the client repeatedly asks (or _polls_) the server for data. Then the client would wait until a response is returned. The problems are HTTP overhead for empty responses and finding the right interval for polling is also challenging.

  The next improvement is **HTTP Long-Polling**, where the client requests data just like above and the server keeps the connection 'hanging' until data becomes available. The problem is that the connection gets closed because of timeout, so the client must reconnect.

  **Server-side events** are a persistent **uni-directional connection** between a server and a client.

  **WebSockets** are a persistent **bi-directional, full-duplex TCP connection** between a client and a server. It removes the HTTP overhead (SSL handshake, content negotiation, lots of headers) for every message.

  Read more [here](https://www.html5rocks.com/en/tutorials/eventsource/basics/).
                        
</div>

- [ ] Polling
- [ ] Server-Sent Events (SSEs)
- [ ] WebSockets


### Decide between a <mark>monolith or a microservice architecture</mark>.


<div class="note" markdown="1">

[Conway's Law](https://en.wikipedia.org/wiki/Conway%27s_law):

> _Any organisation that designs a system will inevitably produce a design whose structure is a copy of the organisation's communication structure_.


**Would you fight 100 duck-sized horses or one horse-sized duck?**

All systems evolve from simple to complex, as business needs grow. Most products start with simple use cases and small teams where speed of iteration is more important than designing a scalable architecture.

As the system complexity increases, opting for a modular architecture has obvious benefits:

- Component decoupling
- Increased security
- Increased resilience to defects
- Easier to experiment and evolve
- Faster versioning
- Faster build and deploy times
- Fewer conflicts

[This article](https://medium.com/fixter-tech/the-less-talked-about-benefits-of-microservices-fb32bbfc3ce9) explains each of the benefits above as well as the disadvantages.

**Independent Systems Architecture**

The [Independent Systems Architecture](https://isa-principles.org/) is a collection of best practices for microservices and self-contained systems, as well as challenges. In a nutshell, it advertises for two levels of architecture decisions:

1. **Macro Architecture**: decisions covering all services, e.g. communication, authentication, CI/CD
2. **Micro Architecture**: individual decisions at service level, e.g. runtime environments

Using ISAs principles, organisations can get the best of both worlds: 

- Scale the knowledge and the talent pool
- Increase the number of components and complexity of systems
- Handle different uses cases appropriately, e.g. web vs data science vs high-concurrency
- Prevent _scaling chaos and enthropy_

**Lambda vs Kappa architectures**

This is really the knitty-gritty of data stores architecture. 

A Lambda architecture handles both _batch_ and _streaming_. When new data is ingested into a system,
it goes through three layers:

1. The _batch layer_ operates with append-only raw data and pre-computes batch views
2. The _serving layer_ then indexes these batch views and opens them for querying
2. The _speed layer_ works in parallel to answers queries directly for speed and real-time views

A Kappa architecture is a simplified version of Lambda, where data flows from a combined _batch_ and _speed_ pipeline then a _serving layer_. It's useful when data is more homogenous, so the batch and stream data is fairly similar.

You can read more about it [here](http://lambda-architecture.net/) and [here](http://milinda.pathirage.org/kappa-architecture.com/).

**What about the frontend?**

Say you have a large and complicated checkout web page, which is changed by different functional teams, for example, Growth and Customer Satisfaction. Both teams want the same things:

- Keep the page operational to ensure users can pay
- Minimise the page latency to provide a great user experience
- Deploy fast experiments with new features or variations

The biggest question in this scenario is _Who owns what when things go bad?_

Github found a way to do that in a monolith with [file ownership](https://github.blog/2021-01-06-building-on-call-culture-at-github).

Another way is to split the frontend as well. This could be done page by page or within a single page.

- Each team owns components/fragments on the page, separated by responsibility
- Teams share a common design language
- Teams maintain a common UI library (very hard in practice, needs a coordinator)
- Each component has its own CI/CD 
- Each component can be built with different tech stacks. We don't all need to be React developers. 

That's the idea behind microfrontends. There are [many ways](https://thenewstack.io/microfrontends-the-benefits-of-microservices-for-client-side-development/) to implement this. Each big tech claims they do it better with their own open source frameworks. One size doesn't fit all.

**These are not decisions that the tech team should make in isolation from the business and product, just because they favour one or another.**

The communication should be both ways:

- The business can think about the likelyhood of hitting gold quickly
- Tech and product can think ahead how the system will handle an extra order of magnitude

</div>

- [ ] Decide whether to monolith or not based on the system's size and complexity and team distribution
- [ ] Think ahead for at least one order of magnitude, some companies hit gold quickly :)


### Decide between <mark>control and automation of services</mark>.

<div class="note" markdown="1">

**Server-based vs serverless**

 Cloud providers removed the need to provision and maintain expensive physical servers upfront and maintain those by offering _Infrastructure-as-a-Service (IaaS)_ and _Platform-as-a-Service (PaaS)_ at a fraction of the cost. Every business is nowadays a technology business. 

_Serverless_ and _Function-as-a-Service (FaaS)_ go further by dynamically and transparently managing the allocation and provisioning of underlying servers. Application code runs in long-running _stateless containers_ or event-triggered _lambdas/functions_. The cloud provider manages execution based on configurable _runtime environments_ and _dependencies_. Examples: AWS BeanStalk, Fargate, Google Cloud Run, GAE, Google Cloud Functions, AWS Lambda, Kubeless, KNative, Serverless etc.
 
 Serverless increases _scalability_ and _removes maintainance overhead_. The trade-off is _less control over the underlying host machines_. 

_Scale-to-Zero_ reduces costs even more. There's no need to pay for long-running servers when nobody is using the application or a particular component. This helps early stage companies cheaply test their product propositions and established ones do A/B testing.

**Serverless trade-offs**
 
1. You lose the ability to control _resource pricing_. For example, if your business needs are predictable you could negociate with the cloud provider to reserve instances up to three years in advance, giving you economies of scale. Or, if your system is flexible and fault-tolerant components, you can benefit from scrappy instances like AWS Spot.
2. You lose the ability to provide a _consistent multi-cloud developer experience_. I met many ML/AI companies that are multi-cloud because of better price negotiation and discounts. In that case, if you want to provide ready-made images for your teams to start new components e.g. database + server + cache, you want to create your own container images and orchestration which are not cloud-specific. You can't easily do that with serverless, although an infrastructure-as-code tool helps a bit.
3. Serverless is scalable but _not limitless_. If your system has super high CPU needs, e.g. for transcoding videos, you might hit serverless limitations. For example, lambdas can only execute for 15 minutes at a time and you can't have more than 3000 concurrent invocations, although these limits might be negotiable with the cloud provider.
4. Serverless has a lower learning curve. However, for some types of systems like real-time financial trading platforms, there might be a need to go really low level like making sure the clocks are synchronised with NTP across the cloud provider hosts. This level of tunning is not possible with serverless. This requires more DevOps expertise in the team. 
</div>

- [ ] Define the long running services
- [ ] Define the short-lived services

### Determine if services <mark>scales to zero</mark>.

This reduces costs for early stages of development.

- [ ] Services supports scale-to-zero
- [ ] Scale-to-zero is not as important

### Determine if services are <mark>polyglot</mark>.

- [ ] Services are created by multiple teams with different coding languages
- [ ] Services are created with the same technology stack

### Determine how services <mark>communicate</mark>.

For example, inter-service and between services and data stores.

- [ ] Services communicate asynchronously via messages (events)
- [ ] Services communicate synchronously via API calls

### Determine how to <mark>secure service communication</mark>.

- [ ] Service calls are authenticated and authorised via a custom mechanism e.g. JWT tokens
- [ ] Services have _Identity and Access Management (IAM)_ roles and policies
- [ ] Failures and retries are handled

### Decide between <mark>control vs automation</mark>.

- [ ] Services are fully managed e.g. Lambda, Elastic BeanStalk, ECS/EKS
- [ ] Services have more access to host machines, e.g. EC2 instances

### Enable as much service <mark>automation</mark> as possible.

- [ ] Machine images are created and ready to spin new instances quickly
- [ ] Instance disk drives have automatic snapshots enabled
- [ ] Application code is packaged as containers or functions with all dependencies
- [ ] The infrastructure is packaged as code and versioned properly

### Determine the service <mark>storage volume types</mark>.

- [ ] Services need highly-durable, long-term persistence like EBS
- [ ] Services can do with cheaper ephemeral storage like Instance Store

### Determine the <mark>kind of writes</mark> services are optimised for.

- [ ] Services are IOPS-intensive and SDDs are more performant
- [ ] Services perform sequential writes for analytics and HDD are more cost-efficient


### Determine how services are <mark>discoverable</mark>.

- [ ] Through an API Gateway
- [ ] Through a service mesh architecture
- [ ] Through hard-coded URLs (maybe not)

### Determine the <mark>logging and monitoring</mark> metrics.

<div class="note" markdown="1">

It's best to define good metrics to track based on the system needs. For example, we might care more about

- CPU load for text-based ML or a service that handles data compression
- GPU for an image-based ML or crypto
- RAM for a search and index engine or a cache
- IOPS for OLTP
- Throughput for big data and log processing

</div>

- [ ] Good metrics are defined
- [ ] Alarms are set with responsible threshold and appropriate channels
- [ ] Visible dashboards are configured and shared
- [ ] Access to logs and alarms are properly secured
