---
layout: default
title: System testing
permalink: /testing
nav_order: 9
has_toc: true
---

{% include toc.md %}

# System testing

{: .no_toc }

_WIP_

<div class="note" markdown="1">

Testing is the verification of the correctness of a system and simulation of failure modes. Testing is essential. [Research](https://research.cs.queensu.ca/home/ahmed/home/teaching/CISC880/F18/papers/failure_analysis_osdi14.pdf) shows that:

* _"92% of catastrophic system failures are the result of incorrect handling of non-fatal error explicitly signalled in software"_
* _"In 58% of the catastrophic failures, the underlying faults could have easily been detected through simple testing of error handling code"_

**The limits of testing**

As E. Dijkstra pointed out, _testing can show the presence of bugs, not their absence_. That's because complex systems fails in complex ways and no amount of testing can predict all the types of failure.

The main limitations of testing are:

- Tests validate the behaviour of the system against a specified set of input, hence it's used to surface the _known unknowns_.
- Most testing still happens in heavily controlled pre-release, non-production environments, hence the testing conditions are not the same.

Post-release, production "testing" is what monitoring and observability are about.

**Testing and deploying strategies**

There are several strategies that can be used to deploy new applications (or versions) to production, to minimise the impact of defects and help testing. Each strategy has pros and cons.

1. **Recreate**: Version 1 is terminated before Version 2 is deployed.

    | Pros                                | Cons                                 |
    | ----------------------------------- | ------------------------------------ |
    | Easy to setup                       | Downtime                             |
    | Application state renewed in one go | 100% impact from a defective version |

2. **Incremental**: Version 2 is slowly rolled out and replacing Version 1.

    | Pros                                 | Cons                                  |
    | ------------------------------------ | ------------------------------------- |
    | Defects don't propagate to all users | Full rollout takes time and resources |
    | Graceful data rebalancing            | Supporting multiple versions          |

3. **Blue/Green**: Version 2 is released alongside Version 1 and traffic is redirected.

    | Pros                                | Cons                            |
    | ----------------------------------- | ------------------------------- |
    | Application state renewed in one go | Expensive, double the resources |
    | Instant rollout                     | Harder to transfer state        |

4. **Canary**: Version 2 is released to a small subset of users before full rollout.

    | Pros                                 | Cons         |
    | ------------------------------------ | ------------ |
    | Application state renewed in one go  | Slow rollout |
    | Defects don't propagate to all users |
    | Instant rollout for some users       |              |

5. **A/B testing**: Version 2 is released to a small subset of users under specific conditions.

    | Pros                                 | Cons                            |
    | ------------------------------------ | ------------------------------- |
    | Instant rollout for some users       | Harder to troubleshoot sessions |
    | Defects don't propagate to all users |                                 |

6. **Shadowing**: Version 2 is deployed alongside Version 1 and receives real traffic but doesn't impact the response.

    | Pros                              | Cons                            |
    | --------------------------------- | ------------------------------- |
    | Performance testing in production | Expensive, double the resources |
    | No impact to users                | Complex to setup                |

</div>

### Ensure the system employs appropriate testing strategies in the <mark>pre-release phase</mark>.

- [ ] Linting
- [ ] Static analysis
- [ ] Unit tests
- [ ] Fuzz tests
- [ ] Functional tests
- [ ] Stress tests
- [ ] Regression tests
- [ ] Contract/API tests
- [ ] Smoke tests
- [ ] UI/UX tests
- [ ] Penetration tests

### Ensure the system employs appropriate testing strategies in the <mark>deployment phase</mark>.

- [ ] Integration tests
- [ ] Load tests
- [ ] Soak tests
- [ ] Config tests
- [ ] Shadowing


### Ensure the system employs appropriate testing strategies in the <mark>release phase</mark>.

- [ ] Feature flags
- [ ] Monitoring and exception tracking
- [ ] Traffic shaping, e.g. canary, red/blue

### Ensure the system employs appropriate testing strategies in the <mark>post-release phase</mark>.

- [ ] Tracing
- [ ] Profiling
- [ ] Logging and monitoring
- [ ] Chaos testing
- [ ] Auditing
- [ ] Pen-testing
- [ ] Dynamic exploration and troubleshooting