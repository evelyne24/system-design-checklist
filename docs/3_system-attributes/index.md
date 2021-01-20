---
layout: default
title: System attributes
permalink: /attributes
nav_order: 3
---

{% include toc.md %}

# Decide the desirable system attributes
{: .no_toc }

After collecting and clarifying the requirements from the product and engineering team, list the desired system attributes. Consider the trade-offs for each.

<div class="note" markdown="1">

One trade-off is whether we care to design a system which is **scalable** (high availability and throughput, low latency) or is **strongly consistent** and **transactional**. 

Another trade-off is between **maintainability** and **speed of iteration**. That's a measure of **organisational and software delivery performance**, e.g. team culture and handling tech debt, not only the choice of technology.

</div>

### **1. Reliability**

The system functions correctly in case of failures and malicious attacks with very little or no downtime. It does so by self-healing in case of network errors, infrastructure problems, malicious attacks and system load.

### **2. Scalability**

The system handles increased load without degraded performance. It does so by automatically scaling out for peak demands and scaling in when demand decreases, in all necessary components like infrastructure, application services and data stores.

### **3. Maintainability**

The system is simple to operate and evolve over time without major code refactorings. It does so with a coherent design, reusability of components, simplified monitoring, deployment and administration. 

### **4. Efficiency**

The system makes use of available resources and optimises cost. It does so by handling scaling in and out according to demand, predicting usage patterns and achieving economies of scale with techniques like reserved infrastructure and scale-to-zero.

### **5. Security**

The system guarantees confidenciality, integrity and protection against malicious attacks. It does so by being vigilant to quickly detect and remediate vulnerabilities.

### **6. Observability**

The internal state of the system can be inferred from knowledge of its external outputs. It does so by providing metrics, traces, logs and alerts in an aggregated way to ease troubleshooting and allows replication and simulation of real-life failures.

### **7. Consistency (UX)**

The system offers a consistent user experience on multiple platforms. When users switch devices, they can continue where they left off. It does so by maintaining the user state with the appropriate mechanisms like caching and real-time communication with client devices.

### **8. Idempotency**

The system handles repeat requests without unwanted side-effects. For example, if a payment fails, it can be retried without losing money. It does so by using idempotency keys for state-changing requests and using appropriate techniques like exponential backoff and random jitter for handling retries.

### **9. Transactionality**

The system must handle _Online Analytical Processing (OLTP)_.

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

