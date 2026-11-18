# Platform Engineering

This repository is a library of cheat sheets, examples, and readings for building and operating the platforms that support software delivery.

DevOps has evolved into platform engineering as teams turn collaboration, automation, and shared operational responsibility into reusable, self-service capabilities. Platform engineering builds on those DevOps foundations by treating the internal developer platform as a product: a set of tools, services, and supported workflows that help development teams build, deploy, and operate software with less operational overhead.

The goal is to make reliable software delivery easier through consistent environments, automated workflows, and built-in security and observability. Platform teams work with developers to understand their needs, provide documented paths for common tasks, and improve those paths through feedback.

## Platform Capabilities

1. **Developer Experience and Self-Service**
   - Provide documented workflows for creating services, provisioning environments, and deploying applications.
   - Treat developers as platform users and use their feedback to improve usability and reduce repetitive work.

2. **[Automation and Software Delivery](./automation/)**
   - Build reusable continuous integration and delivery pipelines for testing, packaging, and releasing software.
   - Manage infrastructure and configuration as code to make environments consistent, versioned, and repeatable.

3. **[Database Management](./databases/)**
   - Support repeatable database provisioning, schema migrations, and operational maintenance.
   - Incorporate performance monitoring, backups, and recovery into the service lifecycle.

4. **[Observability and Feedback](./logging/)**
   - Provide shared logging, monitoring, and alerting capabilities so teams can understand service health.
   - Use operational feedback and incident findings to improve both applications and the platform.

5. **[Security](./security/)**
   - Integrate security checks, access controls, and secure defaults into platform workflows.
   - Make security practices repeatable through automation and shared tooling.

6. **Reliability and Scalability**
   - Design platform services for fault tolerance, capacity needs, and recovery.
   - Use service level objectives and reliability testing to guide operational decisions.

7. **[Virtualization and Runtime Infrastructure](./virtualization/)**
   - Provide consistent environments through containers, orchestration, and cloud infrastructure.
   - Standardize deployment and resource management so teams can operate services predictably.

## Management, Principles, and Practices

The following collections provide the service management and Site Reliability Engineering (SRE) foundations for building and operating a reliable platform.

- **[Management: Service Management and SRE Teams](./management/README.md)** covers service requirements, planning, resiliency, launches, conversions, disaster recovery, and customer support. It also explores SRE training, interrupt management, operational overload, collaboration, and engagement models. Use this collection to understand platform service ownership and how teams support their users.
- **[Principles: Site Reliability Engineering Principles](./principles/README.md)** covers risk, service level objectives, toil reduction, monitoring distributed systems, automation, release engineering, and simplicity. These principles help platform teams balance delivery speed with reliability and reduce recurring operational work.
- **[Practices: Site Reliability Engineering Practices](./practices/README.md)** covers alerting, on-call operations, troubleshooting, incident response, postmortems, and reliability testing. It also develops load balancing, overload handling, distributed consensus, scheduling, data pipelines, data integrity, and reliable launches. Use this collection to apply reliability engineering to platform services and the workloads they support.

## Further Reading

1. [Linux Directory Structure - Hierarchy](https://eng.libretexts.org/Bookshelves/Computer_Science/Operating_Systems/Linux_-_The_Penguin_Marches_On_(McClanahan)/04%3A_Managing_Linux_Storage/5.12%3A_Linux_Directory_Structure/5.12.01%3A_Linux_Directory_Structure_-_Hierarchy)
2. [Linux Directory Structure (File System Structure)](https://www.thegeekstuff.com/2010/09/linux-file-system-structure/)
3. [How to use GitHub Personal Access Token (PAT) in VSCode](https://linuxpip.org/use-github-personal-access-token-in-vscode/)

## Site Reliability Engineering Foundations

These chapters introduce the SRE approach to service management and explain the production environment behind it. Begin with the responsibilities and engineering principles of SRE, then explore Google’s datacenters, resource management, storage, networking, and the path of a user request.

### Foundational Readings

1. [Introduction](./INTRODUCTION.md)
2. [The Production Environment at Google, from the Viewpoint of an SRE](./GOOGLE.PRODUCTION.ENVIRONMENT.md)

## References

Beyer, Betsy, Chris Jones, Jennifer Petoff, and Niall Richard Murphy, editors. *[Site Reliability Engineering: How Google Runs Production Systems](https://www.oreilly.com/library/view/site-reliability-engineering/9781491929117/)*. 1st ed. O’Reilly Media, 2016. ISBN 9781491929117 (electronic edition).
