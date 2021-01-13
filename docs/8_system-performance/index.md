---
layout: default
title: System performance
permalink: /performance
nav_order: 8
has_toc: true
---

{% include toc.md %}

# Measure system performance
{: .no_toc }

## Software system performance

TODO perf testing

## Software delivery performance

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


## Organisational performance

### Product and process

- [ ] Continuously gather and implement customer feedback
- [ ] Make the flow of work visible through the value stream
    - [ ] Ensure teams have a good understanding and visibility into the flow of work, including the status of products and features in other teams
- [ ] Work in small batches
    - [ ] Ensure teams slice work into small pieces completeable in a week or less
    - [ ] Decompose feature to allow rapid development (short lead times and fast feedback loops)
    - [ ] _Implement this practice at both the product and software level_
- [ ] Foster and enable team experimentation
    - [ ] Build in support and make it safe for teams to experiment new ideas without approval from outside the team
    - [ ] Ensure teams have enough slack to innovate and create new value
    - [ ] _This can only work with the previous three practices in place_

### Lean management and monitoring

- [ ] Have a lightweight change approval process
    - [ ] Encourage pair programming and intra-team code reviews to produce superior code quality
- [ ] Monitor across application and infrastructure to inform business decisions
    - [ ] Use data from application and infrastructure monitoring tools to take action
    - [ ] _Make business decisions beyond paging people when things go wrong_
- [ ] Check the system health periodically
    - [ ] Use appropriate thresholds and rate-of-change warnings
    - [ ] Enable teams to proactively detect and mitigate issues
    - [ ] Move teams from fire-fighting back to innovation when health is restored
- [ ] Limit Work In Progress (WIP)
    - [ ] Explain and promote Lean methodologies
    - [ ] Make constraints visible back to the business, e.g. with Kanban boards
- [ ] Visualise work to monitor quality 
    - [ ] Invest in visual shared displays e.g. shared internal dashboard to monitor quality and WIP
    - [ ] Assign ownership to communication channels in teams and across the business

### Cultural capabilities

- [ ] Support a generative culture 
    - [ ] Ensure pshychological safety at work
    - [ ] Promote high cooperation and trust
    - [ ] Encourage conscious inquiry
    - [ ] Decrease burnout
- [ ] Encourage and support learning
    - [ ] Promote learning as essential for continous progress
    - [ ] Ensure everyone understands that learning is not cost, but investment
    - [ ] Make learning a measure of organisational culture
- [ ] Support and facilitate collaboration among teams
    - [ ] Prevent team silos
    - [ ] Increase interactivity in development, operation, information security etc
- [ ] Provide resources and tools to make work meaningful
    - [ ] Provide opportunities to learn and grow
    - [ ] Monitor team satisfaction
- [ ] Support the engineering leadership
    - [ ] Support leadership to create a vision
    - [ ] Encourage them to be intellectually stimulated
    - [ ] Help them to develop inspirational communication
    - [ ] Invest in engineering management
    - [ ] Acknowledge the importance of personal and team recognition
    - [ ] Celebrate small successes
