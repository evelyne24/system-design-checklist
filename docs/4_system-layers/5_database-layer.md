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

### Ensure <mark>data consistency</mark> requirements.

<div class="note" markdown="1">

**Transactions** are logical units made of multiple read and write operations.

It must be in one of two states:
1. _Committed_ to the database in case it succeeds
2. _Rolled back_ in case of failure

**OLTP vs OLAP**

OLTP systems offer strong **ACID** guarantees.

- _Atomicity_: all transactions succeed or are rolled-back
- _Consistency_: after each transaction the system is structurally sound
- _Isolation_: transactions don't interfere with one another and appear to run sequentially
- _Durability_: commited transactions are permanent even after system failures

By contrast, most Online Analytical Processing (OLAP) systems offer **BASE** guarantees.
- _Basic Availability_: the databases work most of the time
- _Soft State_: data stores don't offer reading consistency across replicas
- _Eventual Consistency_: at some point the data stores become consistent

**Consistency vs scalability**

_ACID_ vs _BASE_ is mostly about _consistency_ vs _scalability_.

**Consistency** means if the data on the nodes is the same at any point in time.

- _Strong consistency_ guarantees that when we read data after we successfully complete a write we see the value just written.

- _Weak consistency_ means that when the data is replicated on multiple nodes, depending which node we read from, we might not see that value immediately.

- _Eventual consistency_ is a type of weak consistency which guarantees that if we wait a while, _eventually_ the data on the nodes will converge to be the same.

_We cannot guarantee both because of the CAP theorem_.

**The CAP Theorem**

A distributed system can provide only _two out of three_ guarantees:

1.  **Consistency**: every read receives the most recent write or an error
2.  **Availability**: every request receives a response
3.  **Partition tolerance**: the system operates even if messages are dropped between its nodes

</div>

- [ ] Consider ACID transaction model when strong consistency is a key requirements
- [ ] Consider BASE transaction model when high availability is more important than strong consistency

### Ensure <mark>data replication and partitioning</mark> requirements.

- [ ] Choose the partition key to avoid hot data that might prevent uniform load distribution

### Consider <mark>concurrency</mark> requirements.

<div class="note" markdown="1">

**Optimistic vs pessimistic concurrency**

- _Optimistic_: before updating the data, the application checks if the cached data has changed since last retrieved. If it's the same, the data is updated. If it's not, the application needs to decide if/how to update it, either via automatic conflict resolution or asking the user to solve the conflict.
- _Pessimistic_: upon data retrieval, the application locks the cache to prevent other instances from changing the same data.

**Trade-offs**

- Optimistic concurrency is suitable for less frequent updates or ones with less likelihood of collisions.
- Pessimistic concurrency is suitable for transactionality, e.g. if multiple items need to be updated at the same time in a consistent manner. However, locking impacts latency.

**Conflict resolution**

Conflict resolution is extremely tricky. Even companies like Amazon get it wrong, like in the case of a famous bug they had where users sometimes saw deleted items being added back to the shopping cart. Read more in [this AWS whitepaper](https://abl.gtu.edu.tr/hebe/AblDrive/69276048/w/Storage/104_2011_1_601_69276048/Downloads/m10.pdf).

Examples where conflicts can happen:

- apps with locally cached data on multiple user devices syncing state, e.g. calendars, reminders, notes etc.
- collaborative software with several users concurrently modifying the same data, e.g. Google Docs, Notion, Miro etc.
- distributed databases and caches with multiple replicas

**When and how conflict strategies are applied:**

1. _On write_: a background process that detects conflicts in replicated databases and allows the application to execute a bit of code.
2. _On read_: every time a conflict is detected, the data is stored as another version. When the data is read, all the versions are returned and the application (potentially through UX) is asked to solve them.

**Automatic conflict resolution strategies:**

1. Conflict-free replicated datatypes ([CRDTs](https://crdt.tech/)) for sets, maps, lists, counters etc.
2. [Mergeable persistent data structures](https://www.researchgate.net/profile/Tom_Mens/publication/3188238_A_State-of-the-Art_Survey_on_Software_Merging/links/5724e1b608ae586b21dbc5d4/A-State-of-the-Art-Survey-on-Software-Merging.pdf) similar to Git version control with a three-way merge function
3. [Operational transformation algorithm](https://en.wikipedia.org/wiki/Operational_transformation) used by Google Docs for concurrent editing an ordered list of items like a text document.

</div>

- [ ] Ensure _read-after-write_ consistency, e.g. if the user reloads the page they will see any updates they've submitted themselves
- [ ] Ensure _monotonic reads_, e.g. if the user requests the same data multiple times in a row, they won't see the data moving back in time because of stale updates
- [ ] Consider _optimistic_ (conflict resolution) vs _pessimistic concurrency_ (locking), i.e. ensuring that updates made by one instance of your services don't overwrite changes made by another


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