# Service Requirements

The case studies and first-person accounts below reflect the source authors’ experience, as published in 2017.

Services are the applications that customers need and use. In this repository, the term **service** means all the parts that make an application work: the software, the hardware, and the operations that bring it all together. A service may be a single piece of software on a single machine, or many different software components across many machines that work in cooperation to provide a service, or something in between. A service may have direct customers, such as a web-based application, or it may be invisible to most people, such as a database that is used by other services.

Fundamentally, system administration is about providing services. Computers and software are useful only when they are actively providing a service, not sitting in a box. Services do not run themselves; it is a human process to create a service as well as to maintain it. A good service meets customer requirements, is reliable, and is maintainable.

A service begins life as a set of requirements. Customers and stakeholders have needs, and the service is created to fulfill them. These requirements are used to create the technical design. The service is deployed and launched. Launching a service is more complex than one would expect.

It is tempting to think of the launch of a service as the end of this process, but it is really just the beginning. Services usually run much longer than it takes to create them.

A running service requires maintenance and upkeep. As demand grows it is scaled to handle the larger workload. Requirements change over time, and new software releases must be deployed to fix bugs and add features. Because systems fail, disaster planning is part of any service. This includes basics such as data backups and more advanced practices such as service relocation.

Services need to be supported. They must be monitored, so that problems are detected and fixed. There needs to be a team that handles alerts from the monitoring system as well as end-user reports of problems. For many services, the customers can request moves, adds, changes, and deletes. There needs to be a mechanism for them to do so, and a team that is responsible for handling those requests.

In general, this document treats this subject from the perspective of an organization that buys third-party software. The companion volume, *The Practice of Cloud System Administration*, focuses on services where software is developed in-house. These approaches require very different strategies and techniques.

We will discuss services in four parts. This document is an overview of services and service requirements. [Service Planning and Engineering](SERVICE.PLANNING.ENGINEERING.md) covers engineering the solution once the product is selected. [Service Launch: Fundamentals](SERVICE.LAUNCH.FUNDAMENTALS.md) and [Service Launch: DevOps](SERVICE.LAUNCH.DEVOPS.md) examine the service launch process. [Customer Support](CUSTOMER.SUPPORT.md) and [Handling an Incident Report](HANDLING.INCIDENT.REPORT.md) cover ongoing support.

## Services Make the Environment

The richness and utility of the environment we provide to our users depends on the quantity and quality of services we provide.

Services are what distinguish a structured computing environment that is managed by SAs from an environment in which there are one or more stand-alone computers. Homes and very small offices typically have a few services; often they simply rely on their ISP for foundational services. Larger organizations have a rich environment of services: from the applications that run the company, to the foundational services and infrastructure on which they depend. A large organization’s fundamental services are run like an in-house ISP.

**Foundational services** create the platform that other services rely on. Examples include DNS, DHCP, directory services, network access (WAN and LAN), and Internet gateways. While they are generally invisible to users, failures are highly visible because they affect many services.

**Basic services** are user-visible applications that most people have come to expect in an organization. Examples include printing, email, file storage, chat/IM, and VoIP/phone service. Because of their visibility and pervasiveness, people have developed expectations that these services will be reliable and always available.

Lastly, there are **primary applications** and the **back-office systems** that support them. Applications generally drive business functions such as payroll, inventory management, enterprise resource planning (ERP), supply chain management, and so on. Back-office services include the databases and other behind-the-scenes services that support applications. Together they form the company’s critical path: The company cannot function without them.

### Companies No Longer Sell Atoms, They Manage Bits

In the old days computers were not in the critical path to accomplish the business’s objectives. Rather, the computer was viewed as an ancillary device. You could sell a car or perform surgery on a sick patient even if the computer was down. The computer was required after the fact for billing, inventory, and other things after the primary task was complete.

Today matters are much different. Computer-based services are in the critical path. You can’t sell a car today without performing a credit check, entering the order into a supply-chain system, and so on. Surgery requires computer-based scheduling of people, facilities, and equipment; much of the surgical equipment itself is controlled by a computer. The supply chain that brings food to New York City holds about a three-day inventory; if the services that make up the supply chain failed, Manhattan would have to be evacuated lest three million people starve.

All aspects of modern society require that our services are available and performant. Improving a process, whether it is to make it more efficient, better, or more profitable, requires the involvement of the IT department. IT has a duty to be an agent of change and an ethical responsibility to participate in the constant process of change and improvement.

No matter which business a company thinks it is in, it is really in the software and operations business. If its management does not realize this foundational fact, the company is doomed.

## Starting with a Kick-Off Meeting

Creating and launching a new service is difficult. It cannot be done alone or in a vacuum.

Most of the requirements gathering and project management are done via email, IM, teleconference, and video chat. Nevertheless, it is critical to start out with a face-to-face meeting—the kick-off meeting.

A kick-off meeting should have all the stakeholders present. **Stakeholders** are all the key people affected by or involved in the project. Use this meeting to get agreement on the goal of the new service (i.e., which problem is being solved), a timeline for completion, and an approximate budget. You won’t be able to resolve all these issues, but you can bring them out into the open. Delegate each unresolved issue to a participant who will be accountable for its resolution.

Having all the stakeholders see one another’s faces and learn one another’s names has a bonding effect that improves collaboration in the future. Taking the time to do this is an investment in the success of the project. Everyone should introduce themselves and explain their role in the project.

Although painfully low-tech, in-person meetings work better. People ignore email. Phone calls don’t convey people’s visual cues. A lot of people on a conference call press mute and don’t participate. A video conference is sufficient if a physical meeting is impossible or too costly.

Once the kick-off meeting has put everyone on the same page, status meetings can be held by phone and updates provided via email.

## Gathering Written Requirements

The next step is gathering requirements. Requirements are a list of what the service will be able to do. Requirements should list desired functionality, features, and capabilities. Focus on the end goal: what the system will enable people to achieve. The list of requirements guides all the other steps: design, engineering, implementation, testing, and so on. It avoids confusion and finger-pointing. It sets expectations with customers. It can even be a checklist that lets you know when you are done.

Requirements should not include engineering and implementation details. Requirements are not the place to list which language the software will be written in or which font will be used.

When defining the requirements, don’t try to boil the ocean. Some requested features will be too difficult or expensive to implement. Some may not be appropriate for this service to provide, but may be better suited to a different service. Some may be required by too few people to be worth implementing, at least in the initial release.

Some requirements may have external dependencies on other groups, projects, or services. Make sure to flag each of these external dependencies, and have a plan for what to do if the external dependency is not delivered on time.

There are different kinds of requirements. **Customer requirements** describe the features and functionality of the service to be built. **Operational requirements** describe what is needed to make the system work in the organization’s IT infrastructure and be maintainable. Management may have budget requirements, security may have access requirements, and so on.

Requirements are written down. They are not simply agreed to in a verbal discussion, tracked on a dry-erase board in your office, or kept in your head. Writing them down in a shared requirements document has many benefits:

- **Transparency**: Unlike your brain or the dry-erase board in your office, everyone can read the requirements document.
- **Fewer gaps**: Writing down requirements reduces errors caused by a request being misheard or forgotten. People can verify that their request was recorded properly.
- **Fewer misunderstandings**: Two people often think they have verbal agreement when they do not. Seeing the decision in writing verifies that everyone is agreeing to the same thing.
- **Buy-in and approval**: Written requirements provide the ability to get buy-in and management approval in an accountable way. People know what they are agreeing to. People can’t claim they didn’t sign off on the requirements if they’ve literally signed their name on a printout. They may claim they didn’t understand the document or that circumstances and budgets have changed, but at least you have baseline agreement.
- **Fixed scope**: A formal requirements document provides a mechanism for handling additional feature requests that arrive after the agreement has been reached, preventing feature-creep. New requirements need to be formally agreed to, documented as a scope change, and subjected to buy-in and management approval before they can be added to the scope.
- **Accountability**: Accountability is a two-way street. Customers can point to the document when a requirement you’ve agreed to turns out to be missing or incomplete. Documentation of requirements also helps delineate features versus bugs. A bug is a requirement that is incorrectly implemented. A feature request is a new requirement that wasn’t previously agreed to. While a bug should be fixed, a feature request requires approval, resource allocation, and possibly budget approval.

A requirements document does not need to be hundreds of pages long. A bullet list may be sufficient for small projects. The essence of business writing is brevity. A short, concise document is easier to read and understand.

Define terminology early in the document. Getting agreement to the ontology that will be used is very important. **Ontology** is the system of terms and definitions that define the system and its parts. Often during a heated debate one realizes that everyone is using the same words but meaning different things. Pausing to get agreement on terminology helps everyone see eye-to-eye. At that point we often realize we were closer to agreement than we had previously realized.

Annotate the importance of each requirement. Marking each as “must have,” “desired,” or “optional/nice to have” communicates intent, gives the system designers flexibility, and helps implementors prioritize work when deadlines are looming and features must be cut.

### Applied Ontology: Clarifying Definitions

A very inefficient and contentious requirements-gathering process was improved when someone pointed out that everyone had a different definition of a computer. Is a computer the virtual machine, the machine that the virtual machine runs on, or the entity in a IPAM database, which might be out-of-date and describe a machine that used to exist or will exist in the future?

In this company, it was decided that a computer is a physical machine. A node is a physical machine used to host virtual machines. A virtual machine (VM) is a virtual machine that runs on a machine. A host is the IPAM database entry that can describe either a physical machine, a virtual machine, or a node.

Once this ontology was defined, the confusing discussions were greatly reduced. These may not be the definitions used at other sites or in the industry, but they were what was needed for this particular company.

Ontology resolves confusion regarding many things, including the names of various customer roles, types of transactions, and the phases of a process. Ontology is the bedrock of communication.

## Customer Requirements

Services are built for the customers. If a service does not meet the customers’ needs, building the service was a wasted effort. If we don’t gather customer requirements, we don’t know our customers’ needs. We may *think* we know their needs, but the ESP skills of most system administrators are surprisingly overrated.

Gathering customer requirements is an art and a skill. It requires patience and listening, and many iterations going back and forth until the list is complete.

### Describing Features

The requirements should focus on the list of features, stated from the perspective of what the customer should be able to accomplish using business terms, not technical terms. Record the “what,” not the “how.”

It is better to record a requirement such as “the user should be able to send email” than “there should be a button on the left that the user clicks to send email.” The latter assumes a lot about the interface. Why should the designers be required to include a button even though they have a better idea?

This also means not proscribing particular technology. For example, when an email system is being designed, users might request that it be compatible with the IMAP protocol (RFC3501). They should, instead, be stating their needs at a higher level of abstraction. Ask them why they need such a specific thing. For example, maybe they need to be able to read their email from their smartphone and they know that their phone supports IMAP. It would be better to record the list of smartphones that must be supported, since there may be better ways to support that feature than IMAP. Requiring IMAP support, therefore, may over-specify the requirement.

Conversely, specifying IMAP support may under-specify the feature. Imagine the user’s surprise when the IMAP support is available but his or her particular smartphone is unable to access email. The feature is complete as specified—IMAP is supported—but the user is unable to read email. Requesting that this problem be fixed would be a feature request, not a bug. It would be rejected much to the surprise and dismay of the customer. Technically speaking, the product works as requested.

It is this kind of situation that makes perfect sense to a technical person but is frustrating and unreasonable to a typical user. This is one reason why users view IT departments as difficult to deal with. They’re being told they got what they asked for, but in their mind that’s not true: They can’t read email from their phone. Phrasing requirements at the right level of abstraction is one way that we can prevent this problem.

### Questions to Ask

Ask how, why, where, and when, as if you are a journalist conducting an interview.

How do customers intend to use the new service? Why do they need each feature? Where, when, and how will the system be accessed? How critical is the service to them, and which levels of availability and support do they need for the service?

Determine how large the customer base for this service will be and what sort of performance they will expect from it. For example, when building an email system, try to estimate how many emails, both inbound and outbound, will be flowing through the system on peak days, and how much disk space each user would need. How fast should users be able to open, send, and delete messages? And so on.

Another way to record requirements is through use cases that tell a story. As long as the story can be completed by the final design, the use case is sufficiently covered. In Agile methodology, the format is “As a <type of user>, I want <some goal> so that <some reason>.” For example, one might specify: “As a payroll clerk, I want to be able to enter future payroll adjustments so that I don’t need to do them all on the day they take effect.” Another example: “As a user, I want to indicate which folders should not be backed up so that my backup drive isn’t filled up with things I don’t need saved.”

### Service Level Agreements

The requirements document should include the service level agreement (SLA), or at least some general specifications that can be used to build an SLA.

An SLA enumerates the services that will be provided and the level of support they receive. It typically categorizes problems by severity and commits to response times for each category, perhaps based on the time of day and day of the week if the site does not provide 24/7 support. The SLA usually defines an escalation process that increases the severity of a problem if it has not been resolved after a specified time and calls for managers to get involved if problems are getting out of hand. In a relationship in which the customer is paying for a certain service, the SLA usually specifies penalties if the service provider fails to meet a given standard of service. The SLA is always discussed in detail and agreed on by both parties.

The SLA creation process is a forum for the SAs to understand the customers’ expectations and to set them appropriately, so that the customers understand what is and isn’t possible and why. It is also a tool to plan which resources will be required for the project. The SLA should document the customers’ needs and set realistic goals for the SA team in terms of features, availability, performance, and support. It should document future needs and capacity so that all parties understand the growth plans, and their approximate costs. The SLA is a document that the SA team can refer to during the engineering process to make sure that they meet customers’ and their own expectations and to help keep them on track.

### Handling Difficult Requests

Requirements gathering is a collaborative process. The ultimate goal is to find the middle ground between what the customer ideally wants, what is technically possible, what is financially affordable, what can be accomplished by the date that the solution needs to be available, and what the SA team can provide. A feature that will take years to develop is not reasonable for a system that must be deployed next month. A feature that will cost $1 million is not reasonable for a project with a budget that’s in the thousands of dollars. A small company with only one or two SAs will not get 24/7 support, no matter how much the company wants that.

Your job is to educate as much as it is to record features. Don’t become upset when a customer asks for something technically unreasonable; if the customer knew technology as well as you do, the customer would be an SA. Try to understand the end goal and see how that can be achieved instead. Customers might ask for something that is outside the development budget or sounds technically impossible. For example, they may ask for offline access on a system that is highly interactive, or they may want something with a 24-inch screen to fit inside a pants pocket. Rather than say “no,” focus on the need, not the technology. Ask which benefit or need they are trying to fulfill. It may simply be that sales representatives need to access price information when they are in front of their customers, which is usually in places without network access. That’s a reasonable request when recorded that way. The end result may be a single-page price list that is downloaded to the person’s phone, or a laminated price sheet that the sales rep puts in a wallet.

A common request from non-engineers is that a service be always available or have 100 percent uptime. Non-engineers do not realize how unrealistic that is. Some SAs get extremely upset at such requests. This is a moment to step back and take a few deep breaths. You’re going to spend the next few minutes or so educating your customers about system reliability.

Remember that to a layperson, “always” doesn’t mean 24/7; rather, it means that the system works when the user needs it. This may be from 8 AM to 6 PM. Take a moment to explain to the person that there is “always” and then there is “always.”

Explain that the difference between a system that is up 99 percent of the time and 99.9 percent may be in the thousands of dollars. Improving it to 99.99 percent uptime often costs tens of thousands of dollars, while 99.999 percent uptime often costs in the millions of dollars, and 99.9999 percent uptime is more than the company can afford. Write this on a whiteboard so the person can see the progression. Each step is not 10 times more expensive, but rather 10 times 10 times more expensive.

For comparison, explain that WiFi in a coffee shop has about 98 percent uptime, so striving for better than that doesn’t make sense if most users are going to be working remotely.

At this point you have a common basis for understanding and can have a more reasonable discussion about uptime requirements. Break the week into different time slots: office hours, weekends, late nights, and so on. Define different uptime requirements for each of these segments.

You’ll find taking a few minutes to educate a customer is well worth the effort.

## Scope, Schedule, and Resources

A project plan generally has three elements: scope (the features), schedule (the deadline), and resources (the budget and the staffing). If a project is at risk of not making its deadline, one or more of these will have to change. Features will need to be dropped, the deadline will need to be extended, or the budget (in the form of money or staff allocation) will need to be increased.

For example, if the project needs to be complete in time for the Christmas shopping season, the schedule is inflexible—so the features and/or budget needs to be flexible. If the budget is inflexible, it must be possible to remove features or extend the schedule.

If the project is to send humans to the moon and safely return them, the features are inflexible; a one-way trip is not an option. In this case, either the budget or the schedule must be flexible.

Get buy-in from management as to which of these elements are flexible and which are inflexible at the start of the project. The buy-in process helps communicate priorities to all stakeholders, prevents a lot of confusion all around, and helps focus people on the right priorities. It also has the benefit of discovering early on if management is unwilling to be flexible in any of the three areas. In that case, you have time to send out resumes and find a new job because your management is crazy. Again, that’s something you’d rather know early on in the project.

If the project does look like it will be late, and you have agreement that the schedule is more flexible than the features, then you’ve already set expectations with management. Giving bad news is easier when such expectations have already been set. In an ideal situation, the conversation will not be about what to do, but rather will simply request permission to act on a previously made decision.

## Operational Requirements

Operational requirements are the things that make a system maintainable. These are the features required if we are to have a system that is reliable, scalable, and secure. These features are often invisible to the customers—yet reliability is the most visible aspect of a service.

### System Observability

The most fundamental, and therefore most important, key to a manageable system is that you can see what it is doing. That is, it must be observable or we cannot manage it, fix it, or scale it. The system must provide enough visibility and introspection to debug it when there are problems, optimize it when it is slow, and detect when resources are low before they become a problem. We must have situational awareness to reason about it, predict its future behavior, and do capacity planning. We laugh hysterically when salespeople tell us that their system runs itself, or that their company’s goal is to build a system so that it doesn’t need maintenance. Yes, that should be their goal, but until it is achieved vendors must build observable systems.

The most basic kind of visibility is logging. All parts of the system should log events, transitions, and operations. Logging should be adjustable to provide more detail when debugging. The level of detail may include logging every API call or function call.

The next most important visibility is monitoring. It should be possible to monitor not just whether the system is up or down, but also internal resources and timing: How much bandwidth is being used? How much free disk space is available? How long are requests taking?

If the only thing that can be monitored is whether the system is up or down, then there is no way to prevent outages; you can only respond to them. In other words, you will be warned when it is too late to prevent an outage.

The new system should integrate into existing monitoring systems, dashboards, trouble-ticket systems, and other appropriate visibility tools.

See [Monitoring Distributed Systems](../principles/MONITORING.DISTRIBUTED.SYSTEMS.md) for monitoring practices.

### Remote and Central Management

Services should be remotely managed. If you have to be on the console of a machine to do rudimentary maintenance and configuration, each task will be more timeconsuming. This is also an indication that automating management tasks may not be possible.

Servers are generally located in machine rooms where power and cooling can be managed more effectively. Remote management permits servers to be located anywhere. It is less expensive to rent colocation space elsewhere than to build a computer room in your office. It is often also more reliable.

Services that are replicated in many places must be manageable in a centralized fashion. Remote management opens up the possibility of deploying instances of the service around the world. Configuration changes and upgrades should not require manually updating each instance.

Remote management systems should work well over long-distance (highlatency) connections. For example, some remote management utilities fail to work if the management client is in California and the service itself is in Europe. In this situation each network packet takes more than 100 ms to reach its destination. Protocols may time out. User interfaces may work but can be unusably slow. This is not a limit of the technology, but rather bad software engineering. Plenty of APIs and user interfaces work well over high-latency links—and so should management tools.

### Management Over High-Latency Links

When Tom complained to Dell that iDRAC’s web-based user interface ran unacceptably slowly from as little as 12 miles away, he was told that iDRAC wasn’t designed to work at long distances. We’ll assume the representative misspoke, since it would be hilarious if a remote management system wasn’t designed to work well remotely.

### Scaling Up or Out

If a service is successful, more people will want to use it. Over time it will need to scale to handle more users, more transactions, or more capacity. Services generally scale in one of two ways: up or out.

**Scale-up** means getting a bigger system. To process more requests, more users, or more disk space, the main system is replaced with one that is bigger and faster. For example, to scale up a web server so that it can process more requests per second, you might replace the computer with one that has a faster CPU and I/O system.

**Scale-out** means the system is expanded by adding more replicas. For example, a web-based service is often made up of web servers behind a load balancer. Scaling out means adding more redundant web servers (replicas). Each web server scales out the system to handle more requests per second.

### Software Upgrades

A system must have a reasonable way to upgrade the software and firmware. Ideally, it should be possible to test a new software release outside of production and upgrade the service without interrupting it. Typically, commercial software requires some kind of service interruption when it is upgraded. In that case the interruption should be minimized such that it fits in a scheduled maintenance window.

Software is never done. There will always be the need for new software releases, even if every feature is complete and every bug is fixed. New security holes are discovered constantly, and may not even be in the vendor’s code but in a library or framework it used. For example, in April 2014 a major vulnerability in the OpenSSL cryptography library was announced. The bug, termed Heartbleed, existed in thousands of software packages. Any company that thought its software was “done” got a rude awakening.

More importantly, software is malleable and vendors should be providing new features and improvements over time. There’s always room for improvement.

### Software Is Not a Hammer

A hammer is manufactured and is ready to be sold to the customer. At that point the product is complete. Software is never complete. Anyone who disagrees should leave the software business and go back to making hammers. Be wary of vendors that provide upgrades rarely, sparingly, or only begrudgingly. If a security hole is discovered, the vendor should be able to provide a software patch within hours or days, not leaving you to wait for the annual software release. If software releases aren’t frequent, periodic, and scheduled, then this is a sign that the vendor’s management thinks their company is making hammers. Such companies should not receive our support because they fail to support us.

Upgrade processes should exist and be automatable. The upgrade process should integrate into the organization’s software patching automation. It should not involve walking to each desktop machine for a manual update. It should be possible to upgrade some clients, but not all, so that we can mitigate risk by rolling out the upgrade slowly. If all clients have to be upgraded simultaneously, then there is no way to test the upgrade. As the number of clients grows, the concept of upgrading clients at the same time becomes more and more inconceivable.

The more people depend on a service, the more visible a failed upgrade will be. It should be an operational requirement that the service is designed in a way such that it is possible to deploy the service to a test environment for the purpose of both testing the upgrade process and testing the new release itself.

A **production assurance test (PAT)** or **user acceptance test (UAT)** environment can be a good option for preproduction testing. PAT and UAT environments are run at production standard, but upgraded ahead of the main production environment. Some sites select volunteers who use the PAT or UAT environment, rather than the production environment, at all times, to ensure that when an upgrade is rolled out in that environment, it is really used and production tested.

### Environment Fit

The better a service fits into the existing IT environment, the easier it is to adopt it in your environment. Integration issues are reduced and less training or skill development is required. For example, if you use ActiveDirectory, then using a product that interfaces with it will be easier than using a product that has its own directory service. If the service runs on an operating system with which the team is unfamiliar, using it will require not just additional training, but entirely new procedures that have to be developed for tasks such as data backups, account creation, and configuration.

For a small project, it is reasonable to make it an operational requirement that the service fit into the existing OS infrastructure, directory service, and so on. Larger projects require more flexibility. For example, a large SAP ERP deployment is a self-contained environment unto itself. Therefore it is more acceptable that it may introduce a new operating system to your environment. Most likely it will have its own system administration team.

Here are some environmental factors to consider:

- Operating system
- Backup/restore facilities
- Monitoring system
- Network equipment vendor and routing protocols
- Security auditing systems
- Trouble-ticket systems
- Dashboards and consoles
- Network directory services
- DNS and other name services
- Printing systems

### Support Model

A key component in the success of a new service is support processes. Proper support is a requirement that everyone takes for granted, so no one will think to mention it explicitly. This aspect is often forgotten or neglected by the engineers, who are intent on developing the technical solution.

Once the service goes into production, people will start using it. When they have problems with it, they will raise tickets or call the helpdesk. If the helpdesk does not know about the new service, the users of the service will receive very poor support, and will have a bad impression of the service, no matter how well it has been implemented. Define the support model in advance:

- What should the helpdesk personnel do when they receive a ticket related to this service?
- How can they identify that a problem is related to this service?

- Which diagnostic steps should they take?
- Which documentation do they need?
- Which access and authorizations will they need?
- To whom do they escalate the problem, and when?
- Which information should be included in the escalation?
- Will the service have a dedicated support team, or will support for this service be integrated into another team’s portfolio?
- Do any of these support teams need additional resources to take on the support of this service?
- Which training or certifications should the support teams receive before the service goes live?

The project plan will need to include time and budget resources to implement the answers to these questions. Documentation may need to be written, new staff hired, training completed, helpdesk operations updated, monitoring systems expanded, and so on.

### Service Requests

Most services will include standard service requests that people can raise. Depending on the service, these may be access or authorization requests, data changes, configuration changes, resource requests, and so on. A checklist for building requirements might look like this:

- What should people be able to request?
- How do people raise these requests?
- Which changes need to be made to the service request system to support these new service requests?
- Which approvals are required for each of these service requests?
- Who will process the requests?
- Which access and authorizations will that team need?
- Which runbooks does that team need to correctly service the requests?
- To what extent can, or should, these requests be self-service or fully automated?

The answers to these questions will provide some new requirements. For example, depending on whether the requests should be self-service or automated, there are API requirements that need to be taken into consideration. The requirements and dependencies on other systems, such as the service request system, also need to be documented, agreed to, signed-off on, budgeted, and planned.

### Disaster Recovery

Failures happen. Hardware fails, buildings lose power, Internet access gets cut, and so on. Acting like everything will always function perfectly is irrational. Instead, we need to prepare for failures so that we know how to recover from them.

The most basic disaster recovery requirement is that there must be a way to perform backups and restores. This includes total system restores, as well as the ability to restore an individual file or customer data point.

The most common data restore requirement is for something that was deleted by accident. As a result, the operational requirement of restoring individual data points has been implemented in most systems, often with no SA intervention required. For example, the original version of Microsoft Exchange could only back up and restore the entire system. To restore an individual lost email, one needed an otherwise unused machine with enough storage capacity to restore the entire message database. Then the individual email message would be cherry-picked from it. More recent versions of Exchange have the ability to restore an individual’s email messages or a specific message, and most of the time people can help themselves by pulling messages from the recycle bin or “sent” folder before they are cleaned out.

It can be useful to be able to separately back up the system configuration and the user data. Configuration files that are human-readable are preferable to large binary blobs. Being human-readable makes these files easy to store in source code repositories, makes it easy to produce historical diff listings, and so on.

Enterprise applications tend to assume data integrity and resiliency issues are handled at the hardware layer. In other words, working around hardware faults is your problem. Therefore, operational requirements should be specified to meet your needs. For example, you may require that the service run from multiple datacenters, assuming that at least one will be up at any time. If the service will be single-homed, the service can be only as reliable as the datacenter it is in. That may be sufficient for some services.

## Open Architecture

Wherever possible, a new service should be built around an architecture that uses **open standards** and open protocols. This means protocols, file formats, and APIs that are publicly documented so that others can write to those standards and make interoperable products without having to worry about royalties or patent restrictions. Any service with an open architecture can be more easily integrated with other services that follow the same standards.

By contrast, a **closed service** uses standards, protocols, APIs, and file formats that are owned by one company, are controlled by that one company, and do not interoperate with other products. Other products are prevented from using the standard because the standard is not publicly documented, because it requires licensing, or because the vendor forbids it. Vendors use proprietary protocols when they are covering new territory or are attempting to maintain market share by preventing the creation of a level playing field. In the former scenario, the vendor should then work with the appropriate standards bodies to define and document open standards, and many vendors do so.

### The Protocol Versus the Product

SAs should understand the difference between the protocol and the product. For example, there is a difference between the IMAP protocol and the software that implement it. IMAP is not a product but rather a document, written in plain English, that explains how bits are to be transmitted over the wire. This is different from a product that uses IMAP to permit clients to access email. The confusion manifests itself frequently by SAs who conflate the protocol and the software that implements the protocol.

Part of the confusion comes from the overuse of the word “standard.” Companies often refer to a product as being “the standard,” meaning that it has market dominance and is the product to which other products are compared. This usage does not mean there is an open standard which permits other companies to make products that interoperate with the so-called “standard” product.

The source of this confusion is understandable. Before the late 1990s, when “the Internet” became a household term, many people had experience only with protocols that were tied to a particular product and didn’t need to communicate with other companies, because companies were not interconnected as freely as they are now. No Internet? No need to interoperate. This situation gave rise to the notion that a protocol is something that a particular software package implements and does not stand on its own as an independent concept.

Although the Internet has made more people aware of the difference between protocols and products, many vendors still take advantage of customers who lack awareness of open protocols. Such vendors fear the potential for competition and would rather eliminate competition by locking people into systems that make migration to other vendors difficult. These vendors make a concerted effort to blur the difference between the protocol and the product.

Beware of vendors that *embrace and extend* a standard in an attempt to prevent interoperability with competitors. Such vendors do this so they can claim to support a standard without giving their customers the benefits of interoperability. That’s not very *customer oriented*. A famous case of this occurred when Microsoft adopted the Kerberos authentication system as part of ActiveDirectory. This was a good technical decision, as Kerberos is exceptionally secure and robust. However, Microsoft made a gratuitous change to the protocol that Windows clients depended on. Non-Windows clients could talk to any server, but Windows clients could interoperate with only those servers that included the change. Microsoft would not reveal the technical details of the change, so if you had a single Windows client you had to buy all your servers from Microsoft. Microsoft’s decision successfully forced sites to uproot their security infrastructures and replace them with Microsoft products. Thus it enabled Microsoft to claim support for an open protocol but robbed customers of the freedom of choice.

The business case for using open protocols is simple: It creates competition that results in better products for you. By unbundling the client from the server, you can mix any client product with any server product. We have more choices. Vendors now can compete separately at the client level and the server level. We can pick the best of class product in each category.

With closed systems, our hands are tied: We must purchase the client and the server from the same vendor. We have been in many situations where one vendor has a demonstrably better client, but a server that is slow, buggy, and difficult to manage. Meanwhile, another vendor has a bare-bones client but a server that is highly scalable and easy to manage. Since the needs of the many outweigh the needs of the few, or the system administrator, this situation tends to end up with us selecting the first option. If the systems had been based on open protocols, we could have picked the best of both worlds. Instead, the system administrators are left to keep the server limping along.

A better way is to select protocols based on open standards, permitting users and operations to select their own software.

For comparison, the next anecdote illustrates what can happen when customers select a proprietary email system that does not use open protocols but fits their client-side needs.

### Hazards of Proprietary Email Software

A pharmaceutical company based in New Jersey selected a particular proprietary email package for its PC user base after a long evaluation. The selection was based on user interface and features, with little concern for ease of server management, reliability, or scalability.

The system turned out to be very unreliable when scaled to a large user base. The system stored all messages from all users in a single large file that everyone had to have write access to, which was a security nightmare. Frequent data-corruption problems resulted in having to send the email database to the vendor across the Internet for demangling. This meant that potentially sensitive information was being exposed to people outside the company and that the people within the company could have no expectation of privacy for email. It also caused long outages of the email system, because it was unusable while the database was being repaired.

Because of the lack of competition, the vendor considered server management to be a low priority and ignored the requests for server-related fixes and improvements.

Because the system was not based on open protocols, the system support staff could not seek out a replacement server without also replacing thousands of clients at the same time. That was not an option at the time due to retraining and other costs. As a result, the company chose to struggle with the service for many years, since the pain was mostly invisible to the users.

This situation has improved over time, since most applications have migrated to web-based applications. In the old days we had to install a single-purpose application for email, another for calendaring, and more for bug tracking, finance, expense reports, and so on. Now most of those applications are web based, which means we must simply have a compatible web browser. However, even that convenience can be stymied by proprietary plug-ins and extensions such as ActiveX, Silverlight, and Flash, which support only certain browsers on certain operating systems. Given these limitations, it is important to certify web-based applications on all browser/OS combinations that your organization requires.

Open protocols and file formats typically change only in upwardly compatible ways and are widely supported, giving you the maximum product choices and maximum chance of obtaining reliable, interoperable products.

Another benefit from using open systems is that they don’t require gateways to the rest of the world. Gateways are the “glue” that translate between different systems. Although a gateway is a lifesaver when you need to bridge the gap between two incompatible systems, not needing a gateway is even better. Gateways are an additional service that adds to the administrative workload, requiring capacity planning, engineering, monitoring, and ongoing operational support. Reducing the number of services is a good thing.

### Protocol Gateways and Reliability Reduction

In college, Tom’s email system was a proprietary system that was not based on Internet standard protocols, such as SMTP. Instead, the system was sold with a separate service that used a gateway to send email to and from the Internet. The gateway used its proprietary protocol to communicate with the mail server and SMTP to communicate with the rest of the world, and vice versa. The gateway added to the administrative burden of running the system: It was yet another thing to manage, debug, do capacity planning for, and so on.

This gateway was slow, unreliable, and expensive. It seemed that the vendor had engineered the gateway with the assumption that only a tiny fraction of the email traffic would go through the gateway. It had very low throughput. The mail system had many outages, nearly all of which were problems with the gateway.

The vendor had little incentive to improve the gateway, because it let customers communicate with systems that were considered to be the competition.

None of these problems would have arisen if the system had been based on open protocols rather than requiring a gateway.

History repeated itself nearly a decade later when Microsoft’s Exchange mail server was introduced. It used nonstandard protocols and offered gateways for communicating with other sites on the Internet. These gateways added to the list of services that SAs needed to engineer, configure, plan capacity for, scale, and so on. Many of the highly publicized Exchange problems were related to the gateway.

These examples might seem outdated, since today no one now would sell an email system that is ignorant of the Internet. However, it is important to remember these lessons the next time a salesperson tries to sell you a calendar management system, directory service, or other product that ignores Internet and other industry standards but promises excellent gateways at an extra cost or for free. Using standard protocols means using open standards, such as Internet Engineering Task Force (IETF) and Institute of Electrical and Electronic Engineers (IEEE) standards, not vendor-proprietary standards. Vendor-proprietary protocols lead to future headaches. A vendor that offers gateways is probably not using open standards. If you are unsure, directly ask which open standards the gateways interoperate with.

## Summary

A fundamental role of system administration is to provide services to users. Here, we use the term “service” to mean all the parts that make the application work: the software, the hardware, and the operations that bring it all together. In some organizations the IT department provides a rich set of services; in others, very few. Some SA teams are responsible for all services; others are responsible for one of many. Some services form the foundation of all other services, others are basic infrastructure that users require to work and communicate, and still others are applications that drive the company.

When creating a new service, begin by getting agreement from all involved about the end goal, timeline, and approximate budget. Gather requirements. Customer requirements are the attributes and features that provide value to the company. Operational requirements are the features that make it possible to run the service. Requirements should be written down to provide transparency and to assure accountability in agreements and approvals. When gathering customer requirements, specify end results, rather than the technologies or details used to get there. Ask open-ended questions. Define an SLA. Be prepared for impossible-sounding requests that will require you to investigate the need behind the request.

Specifying operational requirements creates a foundation that enables SLA requirements to match what is required to achieve the SLA. A system must be observable, so that it can be monitored and debugged. Other requirements include remote management and the ability for the system to scale, to be upgraded, to survive failures, and to be restored after a disaster. Systems based on open architectures, standards, and protocols encourage vendor competition. Systems should fit into the existing technology environment, including monitoring systems, directory services, and so on.