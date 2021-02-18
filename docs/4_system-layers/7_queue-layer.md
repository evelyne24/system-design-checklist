---
layout: default
title: Message queues
parent: System layers
permalink: /layers/queue
nav_order: 7
---

{% include toc.md %}

## Message queues
{: .no_toc }

_WIP_

### Determine if and why the system needs <mark>asynchronous communication</mark>.

<div class="note" markdown="1">

**Message queues architectures**

| ![Event-driven architecture]({{ "assets/images/event-driven-architecture.jpg" | absolute_url }}) |
| :---------------------------------------------------------------------: |
|                       _Event-driven architecture_                       |

| ![Web-Queue-Worker architecture]({{ "assets/images/web-queue-worker-architecture.jpg" | absolute_url }}) |
| :-----------------------------------------------------------------------------: |
|                         _Web-queue-worker architecture_                         |

**Event-driven architectures:**

- Events are delivered in near real-time and the application instances respond to them as they occur, without blocking each other.
- They can use a _pub/sub_ or _streaming architecture_.
- In a _pub/sub_ architecture, the infrastructure tracks subscriptions and sends events to each subscriber. After the message is received, it can't be replayed. New subscribers don't see the event.
- In a _streaming_ architecture, events ordered and written to a log. Instances don't subscribe to the stream, they can read any part of it and advance through it. New clients can join at any point in time and replay events.

The consumer can process events in at least three ways:

1. _Simple processing_: every event triggers an action in the consumer, e.g. sending an email.
2. _Complex processing_: the consumer processes a series of events, looks for patterns and does an action, e.g. aggregate transactions over a time window, generate a notification if the average crosses a certain threshold.
3. _Event stream processing_: the consumer ingests a feed of events and sends it to stream processors that process and transforms the stream, e.g. analytics and IoT workloads.

**Web-Queue-Worker architectures**:

- Common in front-ends sending client requests to workers that perform _resource-intensive_, _long-running background or batch jobs_, e.g. a single-page web app calling an email or sms service.
- This is a relatively simple architecture, with clear separation of concerns, decoupled and easy to scale independently. However, without careful consideration, both the front-end web application and the backend workers can easily become monoliths difficult to maintain.
- For scalability, the workers can _share state in a distributed external cache_.

</div>

- [ ] The system consists of producers generating event streams consumed by other components e.g. real-time processing, high volume, high velocity data from IoT etc.
- [ ] The system has long-running workflows or batch operations, e.g. a Web-Queue-Worker architecture
- [ ] The system has multiple decoupled components that need to communicate
- [ ] The system has request spikes and must level the load
- [ ] The system requires high throughput and scalability
- [ ] The system requires clear separation of concerns
- [ ] The system requires multiple teams to manage and scale it independently

### Decide if <mark>multiple consumers process the same message</mark> or point-to-point.

- [ ] Each message is consumed by a single receiver (point-to-point).
- [ ] Each message can be consumed more than once.

### Decide if the communication is <mark>push-based or pull-based</mark>.

TODO

### Decide if the <mark>order of the messages</mark> is important.

TODO

### Decide if the communication is <mark>one-way or bi-rectional</mark>.

- [ ] The sender doesn't need a response from the receiver
- [ ] The sender expects an acknowledgement for receiving and processing the message

### Decide what to do with <mark>malformed messages and resource failures</mark>.

- [ ] The system has a limit of retries
- [ ] The system has a delay in between retries, e.g. exponential back-off
- [ ] The system has other queues to handle unprocessable messages, e.g. dead-letter queue

### Decide how to maintain <mark>consistency in a two-phase commit</mark>.

For example, between a message queue and a database.


