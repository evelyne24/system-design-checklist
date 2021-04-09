---
layout: default
title: Databases
parent: System layers
permalink: /layers/database
nav_order: 5
---

{% include toc.md %}

## Databases
{: .no_toc }

_WIP_

### Ensure <mark>data consistency</mark> requirements.

### Ensure <mark>data partitioning</mark> requirements.

### Ensure <mark>data replication and synchronization</mark> requirements.

### Consider <mark>decomposing a monolithic database</mark> into domain-driven schemas.

<div class="note" markdown="1">
There are several problems with shared databases at scale:
1. **Prevents information hiding**, making it unclear _who uses what_ data across a monolithic application or a distributed monolith.
2. Reading and writing directly to a shared database **makes the database schema become a public-facing contract that can't change** without pain.
3. **Breaks the principles of Domain-Driven design**, because microservices can't totally encapsulate their own data storage and retrieval mechanisms.
4. Having the same data controlled by multiple services leads to **lack of business logic cohesion** because behaviour and state can become inconsistent across services.

However, there are situations when a monolithic database makes sense, for example:
* When it's used as a **read-only, static reference**, i.e. a schema holding **highly stable data structures** such as post codes, country codes etc.
* In a **Database-as-a-Service pattern**, where a service exposes a database as a read-only endpoint for multiple consumers. 💡 To find out more about this pattern, read [Monolith to Microservices](https://samnewman.io/books/monolith-to-microservices/) by Sam Newman.

</div>

- [ ] Consider using a database view projecting a subset of an underlying schema, as the first step towards information hiding
- [ ] Consider using a service to wrap around a database, to stop adding data to the underlying schema while decomposition is in progress
- [ ] Consider using a database-as-a-service as an external, read-only endpoint to abstract away the internal database from consumers/clients

### Consider <mark>architecture patterns</mark> to improve scaling, security and performance.

<div class="note" markdown="1">

💡 I prefer Microsoft Azure's collection of [cloud design patterns](https://docs.microsoft.com/en-us/azure/architecture/patterns/) explained in plain English.
</div>

- [ ] [Command and Query Responsibility Segregation (CQRS) pattern](https://docs.microsoft.com/en-us/azure/architecture/patterns/cqrs)
- [ ] [Event Sourcing pattern](https://docs.microsoft.com/en-us/azure/architecture/patterns/event-sourcing)
- [ ] [Materialised View pattern](https://docs.microsoft.com/en-us/azure/architecture/patterns/materialized-view)