---
layout: default
title: System capacity
nav_order: 2
permalink: /capacity
has_toc: true
---

{% include toc.md %}

# Estimate the system capacity
{: .no_toc }

### Determine if the system is skewed towards <mark>reads or writes</mark>.

Compute the read/write ratio, for example 100:1.

<div class="note" markdown="1">

We need to estimate how much data storage is needed and decide how the system e.g. **database** scales to support reading or writing more data.

Scaling a **read-heavy** system makes use of **replication** and **caching**. Replication means having a copy of the same data on multiple nodes to increase the read **throughput** and **availability** if some nodes go down. Caching means keeping a copy of the most frequently read data in memory to minimise **latency** and **bandwidth**.

Scaling a **write-heavy** system uses **partitioning** (**sharding**). Partitioning means splitting the data across multiple nodes when it becomes too large to fit in a single one.

Replication and partitioning have implications on whether the system can offer **strong** or **eventual** **consistency**.

</div>

- [ ] The system is read-heavy
- [ ] The system is write-heavy
- [ ] The system is read/write balanced

### Estimate the <mark>network traffic</mark>, in number of read/write requests per unit of time.

- [ ] Number of requests per month
- [ ] Number of requests per second

### Estimate the <mark>network bandwidth</mark> requirements.

- [ ] The average size of a request in KB/s

### Estimate the <mark>disk storage</mark> requirements

- [ ] Decide how much time the data is expected to be stored
- [ ] Estimate the total storage for the expected duration in MB

### Estimate the <mark>memory requirements</mark> for caching frequently read data

- [ ] Decide a percentage of data to cache, for example 20% of data generates 80% of traffic
- [ ] Calculate the size of the cache

### Create a <mark>summary table</mark> with these estimates

| Name                               | Value | What does this mean? |
| :--------------------------------- | :---- | :------------------- |
| Number of read req/s               |       |                      |
| Number of write req/s              |       |                      |
| Network bandwidth estimates (KB/s) |       |                      |
| Persistent storage estimates (MB)  |       |                      |
| Memory cache estimates (MB)        |       |                      |
