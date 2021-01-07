---
layout: default
title: Functional requirements
parent: System requirements
permalink: /requirements/functional
nav_order: 1
---

{% include toc.md %}

## Functional requirements
{: .no_toc }

Map directly to the main user journey in the product.

### Determine the <mark>development stage</mark> of the product.

This clarifies the desired speed of iteration, performance, resilience and maintainability.

- [ ] _Proof of Concept (PoC)_: a small exercise to test an incomplete idea
- [ ] _Prototype_: an idea in early stages of development
- [ ] _Minimum Viable Product (MVP)_: a functional core solution that can be iterated on
- [ ] _Production-ready_: a fully-fledged product, with a large user base
- [ ] _Mission-critical_: a product that must function continuously to ensure business continuity

### Make a list of the main <mark>functional requirements</mark>.

These map to a typical user journey in the application. For example, for an e-commerce website, these are the steps from registration to checkout.


### Determine how many <mark>requests per unit of time</mark> the system handles currently.

The unit of time can be month, day, second.

- [ ] Hundreds
- [ ] Thousands
- [ ] Hundred of thousands
- [ ] In the millions
- [ ] In the billions


### Determine the <mark>geographical location</mark> of the users. 

This influences the choice of network topology and Content Distribution Network (CDN).

- [ ] Users are co-located in a small geography, like a country
- [ ] Users are distributed across the world

### Determine how the system <mark>load fluctuates</mark> in the next six to twelve months.

This informs a choice of architecture that scales relatively easy without major refactoring.

- [ ] The system load will increase slowly
- [ ] The system load will increase by at least an order of magnitude

### Determine if the product has specific <mark>usage patterns</mark>.

This gives an idea about potential cost optimisations.

- [ ] The product has constant use throughout the year
- [ ] The product has bursts of activity, for example at certain times during the day
- [ ] The product has seasonal peaks, for example summer/winter

### Determine the <mark>input data type</mark> sent to the system.

This clarifies if the system processes and aggregates _homogenous_ or _heterogenous_ data.

### Determine the <mark>input data format</mark> sent to the system.

- [ ] Text/JSON/XML
- [ ] Binary

### Determine the <mark>output data type</mark> sent back to the clients.

- [ ] Text
- [ ] Images
- [ ] Audio/Video

### Determine the <mark>output data format</mark> sent back to the clients.

- [ ] Text/JSON/XML
- [ ] Binary

### Determine the <mark>data update frequency</mark> handled by the system.

- [ ] Data changes infrequently and can be processed in batches
- [ ] Data changes in real-time and is streamed continuously

### Determine the <mark>types of clients</mark> supported.

- [ ] Web browsers
- [ ] Smartphones and tablets
- [ ] Desktop applications
- [ ] Wearables
- [ ] IoT devices

### Determine if the system offers <mark>multi-platform consistent user experience</mark>.

- [ ] The user switches seamlessly between devices in the middle of a task
- [ ] The user uses the product on one device at a time (for example, a banking app)

### Determine if the system works <mark>offline</mark>.

For example, like Google Calendar.

- [ ] The user uses the product offline
- [ ] The product is operational with an Internet connection

### Determine if the system supports <mark>real-time collaboration</mark>.

For example, like Google Docs.

- [ ] The user uses the product individually
- [ ] Multiple users are collaborating seamlessly in real time