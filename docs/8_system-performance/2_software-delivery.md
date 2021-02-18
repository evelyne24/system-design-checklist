---
layout: default
title: Delivery performance
parent: System performance
permalink: /performance/software
nav_order: 2
has_toc: true
---

## Software delivery performance
{: .no_toc }

_WIP_

### Continuous delivery
- [ ] Use version control for all production artifacts
- [ ] Automate the deployment process
- [ ] Implement continuous integration
- [ ] Use trunk-based development methods
  - [ ] Have fewer than 3 active branches per repo
  - [ ] Have less than a day branch lifetime
  - [ ] Have few or no code freezes
- [ ] Implement test automation
- [ ] Support test data management
    - [ ] Increase the ability to acquire test data on demand
    - [ ] Increase the ability to condition test data into the pipeline
    - [ ] Minimise the amount of test data needed to run automated tests
- [ ] Shift left on security
    - [ ] Integrate data security, regulation and compliance into the design and testing phases
    - [ ] Periodically review infosec, OWASP and industry guidelines
    - [ ] Disseminate knowledge upwards and downwards in the team
    - [ ] Automate monitoring the security of third-party libraries and packages
- [ ] Implement Continuous Delivery
    - [ ] Create fast feedback loops on the quality and deployability of the system
    - [ ] Ensure parity between development stages e.g. dev, test, staging, production

### Architecture capabilities

- [ ] Use a loosely coupled architecture
    - [ ] Ensure a team can test and deploy on demand without requiring orchestration from other teams
- [ ] Architect for empowered teams
    - [ ] Allow teams to choose their own tools and languages at micro level, as they are the skilled practitioners closest to the code
    - [ ] Ensure teams adhere to macro guidelines to prevent enropy and chaos