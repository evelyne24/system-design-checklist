---
layout: default
title: Operational requirements
parent: System requirements
permalink: /requirements/operational
nav_order: 2
---

{% include toc.md %}

## Operational requirements
{: .no_toc }

Describe how the system handles operational demands. Examples: failures, data and security requirements.

### Determine how the system should <mark>handle failure</mark>.

- [ ] The system operates with minimal interruption (highly-available)
- [ ] The system operates without any interruption (fault-tolerant)

### Determine if the product provides <mark>Service Level Agreements (SLAs)</mark>.

For ease of calculations, consider the expected uptime in round units. This informs choices for _redundancy_ and _recovery_.

- [ ] 99% (days of downtime per year)
- [ ] 99.9% (hours of downtime per year)
- [ ] 99.999% (minutes of downtime per year)
- [ ] 99.99999% (seconds of downtime per year)
- [ ] 99.9999999% (milliseconds of downtime per year)

### Determine the <mark>security standards</mark> to comply with.

<div class="note" markdown="1">

For example, a financial system might need PCI-DSS, CyberSecurity etc. 

This informs choices for security, auditing logging, monitoring, auditing, restricting dependencies with Common Vulnerabilities and Exposures (CVEs). 

It also helps estimating the amount of storage space required. If the data needs to be persisted for a certain amount of years and the auditing process is not real-time, older data can be archived to infrequent/cold storage, to optimise costs.

</div>

- [ ] The system must preserve logs for a certain period
- [ ] The system requires external pen-testing and auditing
- [ ] Auditing data doesn't need to be retrieved instantly

### Determine the <mark>data protection standards</mark> to comply with.

<div class="note" markdown="1">

For example, a medical system might need HIPAA and GDPR. This informs how Personal Identifiable Information (PII) is treated, for example masked or scrubbed.

The saying is that _the best way to deal with data privacy and security is not to store it at all_. 
Other considerations are data ethics and individual privacy. 

**Data Ethics Canvas**

It's a great [resource](https://theodi.org/article/data-ethics-canvas/) to start with when considering business, product and technical decisions about data.

**Data Reconstruction Theorem**

There's a mathematical theorem which guarantees that every piece of accurate information you release, however small, will inherently violate the privacy of the participants to some degree.

_The trade-off of data privacy is data accuracy_. To mitigate the DRT, a common approach is Differencial Privacy, which basically adds some random jitter to the data to make it _less accurate_.

**Federated Learning**

Another consideration to data privacy is if _raw data_ goes a centralised point for processing, for example, machine learning training. 

Federated learning solves this way by incrementally training a shared model with pulled resources where the model is combined into an average. The raw data never leaves the source, e.g. a smartphone.

</div>

- [ ] The system does not handle PII
- [ ] The system handles PII
- [ ] Data Ethics Canvas considered
- [ ] Differential Privacy considered
- [ ] Federated Learning considered
