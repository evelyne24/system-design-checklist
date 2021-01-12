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


<div class="info" markdown="1">
[Conway's Law](https://en.wikipedia.org/wiki/Conway%27s_law):

> _Any organisation that designs a system will inevitably produce a design whose structure is a copy of the organisation's communication structure_.
</div>

### Determine the system components that are <mark>long-running or event-based</mark>.

<div class="note" markdown="1">

**Monoliths vs microservices vs distributed monoliths**

_"Would you fight 100 duck-sized horses or one horse-sized duck?"_

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

Another great resource is the [Independent System Architecture](https://isa-principles.org/), a collection of best practices for microservices and self-contained systems, as well as challenges. In a nutshell, it advertises for two levels of architecture decisions:

1. **Macro Architecture**: decisions covering all services, e.g. communication, authentication, CI/CD
2. **Micro Architecture**: individual decisions at service level, e.g. runtime environments

Using ISAs principles, organisations can get the best of both worlds: 

- Scale the knowledge and the talent pool
- Increase the number of components and complexity of systems
- Handle different uses cases well, e.g. web vs data science vs high-concurrency
- Prevent _scalable chaos_


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

- [ ] Define the container-based services
- [ ] Define the function-based services

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

- [] Service calls are authenticated and authorised via a custom mechanism e.g. JWT tokens
- [] Services have _Identity and Access Management (IAM)_ roles and policies

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
- [ ] Through hard-coded URLs

### Determine the <mark>logging and monitoring</mark> metrics.

- [ ] CPU, e.g. text-based ML, compression 
- [ ] GPU, e.g. image-based ML, cryptocurrency 
- [ ] RAM, e.g. search engine, cache
- [ ] IOPS, e.g. OLTP 
- [ ] Throughput, e.g. big data analysis, log processing
