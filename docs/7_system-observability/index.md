---
layout: default
title: System observability
permalink: /observability
nav_order: 7
has_toc: true
---

# System observability

{: .no_toc }

_WIP_

<div class="note" markdown="1">

🤔 **Testing** vs **Observability** vs **Monitoring**.

**Testing** is about _verifying the correctness of a system_, to ensure that it meets the functional and non-functional specification mentioned in a requirement document. Testing has historically been done _before_ release and on non-production environments. This trend is slowly changing so fiddling with live traffic has become less of an anti-pattern and more of a "design for failure" good practice.

**Monitoring** is about _defining_, _collecting_ and _aggregating_ a set of metrics that describe how the system performs. For example, it could be metrics about the system load (CPU, memory), response time, number of 4xx/5xx errors per unit of time etc. These metrics can looked at almost real-time or queried afterwards. Actions such as alarms can be added whenever metrics go below or above a certain acceptable threshold. When defining metrics, we start with an already good idea about **what** we're interested in monitoring (_known unknowns_).

**Observability** is a superset of monitoring and more about situations where **we don't know what to look for** in our system (_unknown unknowns_). For example, we're aware that our end users are experiencing _a situation_ and we want to understand what's happening in the system at that particular moment and _why_.

My analogy is that monitoring is like general medicine (_'Is the patient healthy?'_) while observability is like forensics (_'Why is the patient having a seizure?'_). Another is logging vs debugging/tracing.

👀 My favourite "observability manifesto" is from [Distributed Systems Observability](https://www.oreilly.com/library/view/distributed-systems-observability/9781492033431/) by Cindy Sridharan:

> "In its most complete sense, observability is a property of a system that has been designed, built, tested, deployed, operated, monitored, maintained and evolved in acknowledgement of the following facts:

> - **No complex system is ever fully healthy.**
> - **Distributed systems are pathologically unpredictable.**
> - **It's impossible to predict the myriad states of partial failure various parts of the system might end up in.**
> - **Failure needs to be embraced at every phase, from system design to implementation, testing, deployment and finally, operation.**
> - **Ease of debugging is a cornerstone for the maintenance and evolution of robust systems.**"


Both observability and monitoring are useful to:

1. Understand the health of a system: _Is the system functioning well, within acceptable load?_
2. Support business decision-making: _Do we understand enough about spike patterns to inform marketing/sales?_
3. Help engineers debug production systems, in order to diagnose, solve and prevent problems: _Is our payment service double-charging customers?_
4. Provide auditing for security certifications and pen-testing: _Are our systems vulnerable to attacks?_

The **more automation and observability**, the **less 🚨 panic and fear 🚨** when a problem occurs and the **more ♻️ sustainable on-call rotations ♻️**, because we'll know:

1. _Where_ to find the data we need for investigation.
2. _Who_ has access, responsibility and ownership of that data.
3. _How_ to react to incidents and escalate when a complex issue could impact customers.

It's best to separate the code and tools for observability and monitoring, outside the main codebase, to avoid introducing side-effects and 🪲🐛.

💡 **Implementing Monitoring and Observability**

There are different ways to implement monitoring and observability:

1. **White box monitoring**
2. **Black box monitoring**
3. **Instrumentation**

**The three pillars of observability**

1.  **Logs**: immutable, timestamped record of events in the system, in plaintext, binary (for replication) or structured (`JSON`).

    | Pros                                                                         | Cons                                                                     |
    | ---------------------------------------------------------------------------- | ------------------------------------------------------------------------ |
    | Provides highly granular information in a single service                     | Not easy to track across a distributed system                            |
    | Easy to generate                                                             | Enriched events can leak sensitive data                                  |
    | Many tools to collect, aggregate, filter and visualise e.g. Logstash, Kibana | Adds overhead, if too many, uncompressed or synchronous                  |
    |                                                                              | Order is not always guaranteed, so can't rely on them to debug real-time |

2.  **Traces**: representation of related events that encode an end-to-end request through a distributed system.

    - A trace provides visibility into both the path and structure of a request identified by a global ID.
    - It's represented as a directed graph of _spans_ and edges called _references_.
    - When the execution of a request reaches a certain span (or hop), an async record is emitted with metadata to a _collector_ which can reconstruct the entire flow of execution.

    | Pros                                     | Cons                                                       |
    | ---------------------------------------- | ---------------------------------------------------------- |
    | Understand the lifecycle of a request    | Hard to retrofit into an existing system                   |
    | Less heavy than logs because of sampling | Harder to integrate with external frameworks and libraries |
    | Debug across services                    |                                                            |
    | Part of the data plane of service meshes |                                                            |

3.  **Metrics**: numeric representation of data measured over intervals of time

    | Pros                                                                                           | Cons                                                                                                       |
    | ---------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------- |
    | Can help predict system behaviour                                                              | Harder to scope per request                                                                                |
    | Reflect historical trends                                                                      | Hard to correlate with logs, unless using a UID (a high cardinality label), which affects database indices |
    | Great for visualising in dashboards                                                            | Insufficient to understand the lifecycle of a request across multiple subsystems                           |
    | Application scale doesn't affect their overhead and storage                                    |                                                                                                            |
    | Aggregation, sampling, summarization gives a better picture of overall system health than logs |                                                                                                            |
    | Actionable, e.g. with alerts                                                                   |                                                                                                            |

> **"The goal of observability is not to collect logs, metrics and traces. It's to build a culture of engineering based on facts and feedback."** - Brian Knox, Digital Ocean


🛠 **Observability and monitoring tools**

This is, by no means, an exhaustive list, just some of the tools I know of.

- [OpenTelemetry](https://opentelemetry.io/)
- [NewRelic](https://newrelic.com/)
- [PagerDuty](https://www.pagerduty.com/)
- [DataDog](https://www.datadoghq.com/)
- [OpsGenie](https://www.atlassian.com/software/opsgenie)
- [Prometheus](https://prometheus.io/)
- [Cortex](https://cortexmetrics.io/)
- [Honeycomb](https://www.honeycomb.io/)
- [Sentry](https://sentry.io/)
- [VictoriaMetrics](https://victoriametrics.com/)
- [AWS tools](https://aws.amazon.com/products/management-and-governance/use-cases/monitoring-and-observability/)

</div>

## Signals

<div class="note" markdown="1">
There are many ways to define signals to be monitored and acted upon, like the ones based on Google's SRE practices:

1. **Four Golden Signals**, by Rob Ewaschuk: **Latency**, **Errors**, **Traffic** and **Saturation**.
2. **USE Methodology**, by Brendan Gregg: **Utilisation**, **Saturation** and **Errors**
3. **RED Method**, by Tom Walkie: **Request rate**, **Error rate** and **Duration of request**
</div>

### Ensure your system is designed to be observable and testable ["in a realistic manner"](https://www.oreilly.com/library/view/distributed-systems-observability/9781492033431/).

- [ ] The system is designed in a way that actionable failures can be discovered in testing.
- [ ] The system can be deployed incrementally.
- [ ] The system can be rolled back (and forward) if some key metrics deviate from the baseline.
- [ ] Post-release, the system reports enough health and behaviour data so that it can be understood, debugged and evolved.

### Ensure your system monitors the <mark>Four Golden Signals</mark>.

For more information, read the [Google's SRE book](https://sre.google/sre-book/monitoring-distributed-systems/#xref_monitoring_golden-signals).

- [ ] **Latency**: the time it takes to serve (or fail to serve) a request
- [ ] **Traffic**: how much demand the system handles, e.g. requests per second
- [ ] **Errors**: e.g. `HTTP` 5xx, 4xx or 200 but with the wrong response
- [ ] **Saturation**: how 'full' the system is, based on the most constrained resource, e.g. memory or disk throughput

### Ensure your system's is monitored ["in the simplest way, but no simpler"](https://sre.google/sre-book/monitoring-distributed-systems/#xref_monitoring_golden-signals).

- [ ] Instead of averaging, monitor the tail of latencies by bucketing them into a histogram, e.g. requests below 10ms, below 100ms, etc.
- [ ] Set the right time frame granularity for each metric, e.g. per-second CPU measurements might be too granular and costly.
- [ ] Avoid collecting non-actionable signals (noise) and regularly trim the ones that are rarely exercised.
- [ ] Aim to decrease the _variability_ of your latency if you can't (and shouldn't) decrease it everywhere

## Goals and metrics

<div class="note" markdown="1">

**The Three Principles of Customer Reliability Engineering (CRE)**

1. Reliability is the most important feature.
2. **Users**, not monitoring, decide reliability.
3. What **reliability** should aim for:

    + **Software -> 99.9%**, 
    + **Operations -> 99.99%**
    + **Business -> 99.999%**

Let's just pause here and let the [Google SRE](https://sre.google/resources/practices-and-processes/art-of-slos/) wisdom sink in:

> ***"We believe that architecting a system carefully and following best practices for reliability, like running in multiple globally distributed regions, means that the system has the potential to achieve three nines over a long time horizon.*** 

> ***To reach four nines, it's not enough to have talented developers and well-engineered software. You also need an operations team whose primary goal is to sustain system reliability through both reactive, like well-trained incident response and proactive engineering, things like removing bottlenecks, automating processes, and isolating failure domains.*** 

> ***Everyone assumes that they'll need five nines at first. After all, reliability is the most important feature of a system. But reaching these levels of reliability actually requires sacrificing many other aspects of the system, like flexibility and release velocity.***

> ***Every component must be automated such that changes are rolled out gradually and failures are detected quickly and rolled back without human involvement.*** 

> ***Each additional nine makes your system 10 times more reliable than before. But as a rough rule of thumb, it also costs your business 10 times more. Engineering a system to have reliability as its top priority means making many hard choices with wide ranging consequences to the business, and in most cases, the cost-benefit analysis just doesn't add up."*** 

**Goals and metrics come from empathy for the user**

Every goal should be set with the user journey in mind and **empathy** for their feelings and behaviours. 
_What do I mean by empathy?_ Become or imagine you're one of your users.

They're searching for something to buy on your website. Their excitement is at peak, they're giddy like on Christmas Day, waiting to see the results (["rewards of the hunt"](https://returnofthemack.medium.com/viable-reward-remember-share-hooked-by-nir-eyal-ec096a20c8e7)), so they can choose from your products. Therefore, it makes sense to prioritise reducing the latency and response time for searching and browsing pages.

Once they found what they're looking for, their excitement and impatience starts to fade and by the time they're on the checkout page, they're actually slower and more cautions, so it might be acceptable and even unnoticeable to them if the checkout page is slightly slower than the search one.

This, of course, it's just an example. You really need to _understand your users_.

</div>

## Alarms and fatigue

<div class="note" markdown="1">
Alerts are about _systems and automation_ as much as they are about _humans_. 

The more we unburden the humans at work, the more they can focus on writing exciting code and innovative solutions - which is what _high performing teams_ do, as opposed to putting out infrastructure fires. 

</div>

## Logging and auditing

## Disaster recovery
