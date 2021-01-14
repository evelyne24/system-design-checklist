---
layout: default
title: Extended requirements
parent: System requirements
permalink: /requirements/extended
nav_order: 3
---

{% include toc.md %}


## Extended requirements
{: .no_toc }

Describe secondary functionality important for operations but not necessarily visible to the end-users. Examples: collecting metrics and analytics.

### Determine if the system uses <mark>data for business decisions</mark>.

- [ ] The business needs real-time analytics
- [ ] The business uses analytics but not real-time

### Determine if the business serves <mark>data to other third-parties</mark>. 

This informs choices for the API design. For example, API protocols, throttling, secure access etc.

- [ ] The business provides external APIs to third-party developers
- [ ] The business does not provide external APIs

### Determine if this system has <mark>cost constraints</mark>.

### Check if there are other <mark>specific requirements</mark> omitted in this list.
