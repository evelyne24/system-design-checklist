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

For example, a financial system might need PCI-DSS, CyberSecurity etc. This informs choices for security, auditing logging, monitoring, auditing.  

It also informs about the amount of storage space required, for example, if the data needs to be persisted for a certain amount of years.

More over, if the auditing process doesn't require real-time acces, after a while, older data can be archived and moved to cold storage, to optimise storage costs.

</div>

- [ ] The system must preserve logs for a certain period
- [ ] The system requires external pen-testing and auditing
- [ ] Auditing data doesn't need to be retrieved instantly

### Determine the <mark>data privacy standards</mark> to comply with.

For example, a medical system might need HIPAA and GDPR. This informs how Personal Identifiable Information (PII) is treated, for example masked or scrubbed.

- [ ] The system does not handle PII
- [ ] The system handles PII

### Determine if this system has <mark>cost constraints</mark>.
