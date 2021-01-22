---
layout: default
title: Container security
parent: System security
permalink: /security/container
nav_order: 2
---

{% include toc.md %}

## Container security
{: .no_toc }

<div class="note" markdown="1">
Containers have become a popular standard of packaging applications using the [Open Container Initiative](https://opencontainers.org/) specification:

- Operating systems like Ubuntu, CentOS etc.
- Application package managers like `pip`, `npm`, `RubyGems` etc.
- Application code and associated metadata


Unlike VMs and physical servers, containers can be scanned for vulnerabilities _before deployment_. However, they come with security risks, especially opaque third-party container images. On Docker Hub alone there are more than 7M repositories. 11B images are downloaded each month. 
 
 With so many to choose from, selecting one should take into account:

- _Freshness_: old container images can have [known vulnerabilities](https://blog.neuvector.com/article/equifax-data-breach-analysis)
- _Reputability_: obscure images could harbour backdoors and malicious code
- _Performance_: some images are poorly built and eat up system resources, leading to unnecessary cost or crashing other containers



Container security covers several aspects:
- the image and software inside
- the interaction between the container, the host OS and other containers (i.e. malicious code escaping sandbox)

The best practice is to automate the detection and correction of vulnerabilities at container level using automation, e.g. integrating with tools like [Snyk](https://support.snyk.io/hc/en-us/categories/360000583498-Snyk-Container) or directly with tools provided by the cloud platform, like [AWS ECR](https://docs.aws.amazon.com/AmazonECR/latest/userguide/image-scanning.html).

</div>

### Ensure <mark>container security at built phase</mark>.

- [ ] Use code-analysis tools to recognise and report vulnerabilities
- [ ] Harden containers and reduce the surface of attack by removing any unnecessary dependencies like libraries and packages
- [ ] Scan images and registries regularly to detect and fix vulnerabilities before shipping
- [ ] Create guidelines on appropriate responses, considering urgency and impact, to patch vulnerable systems


### Ensure <mark>container security at ship phase</mark>.

- [ ] [Sign images](https://docs.docker.com/engine/security/trust/) to verify the publisher and that no tampering has occurred
- [ ] Enforce access restrictions and monitor access to sensitive images and deployment tools (registries, orchestration tools)
 
### Ensure <mark>container security at runtime phase</mark>.

- [ ] Scan hosts and containers before and during runtime for vulnerabilities
- [ ] Monitor hosts and containers for suspicious processes such as port scanning and root-priviledge escalations
- [ ] Detect lateral movements and sandbox breaches using suspicious network connections to other containers and hosts
- [ ] Lock down containers to exclude external access if not necessary
- [ ] Use encryption to protect sensitive resources accessible by containers like API keys and secrets
- [ ] Continuously audit security settings for containers, hosts and platform/orchestration tools
- [ ] Add the ability to quickly quarantine containers exhibiting suspicious behaviour during investigation
- [ ] Capture packets and events for further forensic logging analysis