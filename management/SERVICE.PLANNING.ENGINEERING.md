# Service Planning and Engineering

The case studies and first-person accounts below reflect the source authors’ experience, as published in 2017.

After a product has been selected we need to engineer how it will be configured and operated in our environment. For example, engineering a Microsoft Exchange service involves determining which hardware will be used, how the software will be installed and configured, how users will access and authenticate to the service, and how various operational tasks such as backups, monitoring, archiving, and account management will be done.

This document focuses on third-party products. How do we design a service around them? One might think this is a matter of following the installation instructions, but many of the decisions are actually left to the individual system administrator.

Both the **service architecture** and the **local engineering plans** need to be defined and documented. The service architecture is the high-level description and the local engineering plan is the specifics for a particular machine or site.

For example, a service architecture for an email system might describe that there will be three regional clusters, each with the capacity to serve the number of users in that region. The plan would describe, generically, the clusters in terms of types of machines, storage, and networking technologies. These details would include which brands and products would be used.

The local engineering plan for each cluster is much more specific. It might include specific product ordering codes, including codes for each cable, connector, and mounting bracket. While the service architecture includes networking speeds and IP address range sizes, the local engineering plan lists the exact IP addresses to be used for each interface.

Typically the service architecture is developed, and then the local engineering plan is created based on that architecture. In reality, though, there is a great deal of interplay between the two. One step in validating the architecture is to write out the local engineering plans to see if anything was forgotten. One or more local engineering plans might be developed and built in a lab to test reality against the plans. What is learned from this process is used to update the architecture. This process is repeated until we have confidence that the architecture is sound and the local engineering plans are optimal.

At the same time the operational model or support model needs to be defined. The support model defines the operational level agreement (OLA) and the service level agreement (SLA) for the service, as well as who supports the various components of the service.

The remainder of this document discusses some general engineering basics and philosophy and then ways to optimize such plans.

This document focuses on designing a generic service.

## General Engineering Basics

Before this document goes into the more sophisticated aspects of service engineering, there are some basic issues that you should be aware of. Much of this includes the kind of engineering decisions that “go without saying,” yet are so crucial that we need to include them. Consider the following to be rules of thumb, often learned the hard way in real-world environments:

- **Follow vendor recommendations.** Determine how many machines are needed, and which type of CPU horsepower, disk space, RAM, network bandwidth, and so on is required.
- **Mirror your boot disk.** Disks fail. Often the most difficult disk to replace is the boot disk; it requires reinstalling all the software. Mirror the boot disk (RAID 1) so that when (not if) a disk fails, the system will keep running and give you time to replace the failed disk. The only exception to this is when the machine can be rebuilt from scratch in an automated fashion and the machine stores no state.
- **Run services on servers, not workstations.** It is bad practice to run a service on a workstation under your desk. There’s nothing more permanent than a temporary solution.
- **Run services from computer rooms, not offices.** Computer rooms also provide physical security, high-speed networks, reliable power, and cooling that is not available in the typical office.
- **Restrict console access.** The general public should not be able to log into the console of a server.

- **Leverage the existing infrastructure.** As much as possible, new services should be integrated into existing systems and processes. The existing monitoring, alerting, and service request systems should be used, as well as backup systems, and configuration and patch management systems.

## Simplicity

When engineering a service, your foremost consideration should be simplicity. Strive to create the simplest solution that satisfies all the requirements. A simple service will be the easiest to maintain, easiest to expand, and easiest to integrate with other systems. Undue complexity leads to confusion, mistakes, and potential difficulty of use and may well make everything slower. Complex systems are also more expensive in setup cost and maintenance costs.

Computer scientist Brian Kernighan famously stated that debugging is, by definition, more difficult than creating a system. Therefore, when you create a system that is the pinnacle of your engineering abilities, you have, by definition, created a system that you cannot debug.

### Case Study: Cornered by Complexity

A small Pennsylvania-based social network web site had an extremely complicated network designed by the company’s lead technical person. It used complex routing protocols in unusual ways to achieve load balancing and rapid failover in case of failure. The system practically ran itself, except when it didn’t. Only its designer understood how it worked and could debug it. This led to a number of problems when this person became overloaded and was pulled in many directions.

The site tried to hire a full-time networking person to offload the designer’s responsibility for maintaining the network. Because the company was located in rural Pennsylvania, the candidates interviewed mostly had corporate network skills, whereas what the site really needed was a combination of ISP-style networking and advanced load-balancing knowledge.

After more than three years of interviewing candidates, the position remained empty. The company had painted itself into a corner.

During the engineering phase, create multiple design proposals. Consider each of them for its simplicity, manageability, cost, and so on. Revise, revise, revise.

Good engineering is not about having the ability to create perfection in the first draft, but rather about being able to iterate over many revisions, improving each one until we have the design we want.

### Case Study: Branch Office Prototypes

The network group at a company with many buildings around the world found it difficult to maintain the ad hoc network solutions that had been deployed at each building. They decided to standardize.

To simplify the design, the team reduced the number of possible configurations to three: small, medium, and large. Next they created a standard template for the equipment and configuration that would be used in each scenario. Each configuration was deployed in a lab and connected as if it was an actual building. Complex routing protocol configurations were verified and documented. Operational procedures were tested and documented, including the processes for turning up a new building, turning one down, performing failovers between ISPs, repairing components, and so on.

In the prototyping stage, the team members kept asking themselves how things could be simplified further. Operational procedures were unified across the three configurations, naming and labeling standards were made uniform, and many more opportunities to simplify were found.

Each simplification made the systems easier to understand and manage.

## Vendor-Certified Designs

Often third-party products have the engineering dictated to you. The vendor proscribes minimum CPU, RAM, and disk requirements. Often vendors publish **certified engineering designs** or **vendor best practices**. There may be multiple recommendations—for example, separate engineering recommendations for use with up to one dozen, one hundred, or one thousand users.

Even if one uses the certified engineering design exactly, some local decisions always need to be made: hardware vendor, model number, specific configurations, IP addresses, network switch connections, deployment racks, optional configuration parameters, and so on. This is the **local engineering design**.

We like when vendors provide best practice engineering designs because they’ve done much of the hard work for us. The designs are usually based on realworld experience. They have been tested in labs and the real world. The vendor has performed load testing to determine the maximum number of users or other capacity. It is best to test these assertions in a lab environment. The best approach is to trust the vendor recommendations, but verify them. Often, however, only larger companies have the resources to do so. Smaller companies just have to trust the vendors.

Straying too far from the vendor’s recommendations will result in the loss of support. The vendor won’t help you if you call customer support with a problem because you are using the product in a configuration the vendor does not certify.

Usually we must engineer how the system will be installed ourselves. Vendorcertified designs are relatively rare. Even when a vendor design exists, creating the local engineering design requires a lot of expertise and effort.

## Dependency Engineering

The reliability of a service is greatly influenced by its dependencies. A system is never more reliable than the most unreliable system it has a hard dependency on. For example, if an application server can be accessed only via an unreliable network, the application will never be more reliable than that network.

The larger and more complex a system becomes, the more dependencies it has. We can manage these dependencies during the engineering process in ways that greatly improve the resulting system’s reliability.

### Primary Dependencies

Primary reliability refers to the core system reliability excluding any external dependencies. As system administrators, we often do not have much control over the reliability of the software itself, especially for third-party products.

What we can control are our decisions about the hardware it runs on. Enterprise software tends to be written assuming the hardware will never fail. In turn, we choose reliability features such as mirrored boot disks, ECC RAM, dual NICs, and so on. These issues are discussed in [General Engineering Basics](SERVICE.PLANNING.ENGINEERING.md#171-general-engineering-basics).

### External Dependencies

We can control many of the external dependencies that influence a service’s reliability.

First we need to know what those dependencies are. A **dependency matrix** is a list of which subsystems depend on other subsystems. Usually the dependency matrix resembles a tree diagram, with each level relying on services in the levels below, and no cross-dependencies. When engineering a new service, you should create a dependency matrix to document the dependencies so that everyone reviewing the plan understands them. Such a matrix can be used to find high-risk dependencies and mitigate them. Once the service is launched, this matrix is useful for operational tasks such as basic debugging as well as disaster planning.

All dependencies are not the same. A **hard dependency** means the dependent service will fail if the other service fails or is unavailable. A **soft dependency** is one that does not directly cause a failure, though it may reduce performance.

For example, an application may have have a hard dependency on an external storage server or SAN. If the SAN dies, the application dies with it.

In contrast, a DNS server is usually a soft dependency. Systems are configured to depend on multiple DNS servers. As long as at least one is reachable, the client is able to perform DNS lookups. There may be a performance penalty, but the client is able to operate. Note that the DNS *service* is usually a hard dependency, but a single DNS *server* is usually a soft dependency.

Applications often have soft dependencies on other applications. An accounting application may depend on another server to provide foreign currency exchange rate information. However, if that server is unavailable, the application may work around it by using cached data, disabling a specific feature, or requiring the data to be manually entered. This kind of soft dependency is called **graceful degradation**.

As we engineer a system and review the dependency matrix, we can improve the reliability by identifying hard dependencies and either eliminate them or reengineer them to be soft dependencies. For example, earlier we mentioned a service that was only as reliable as its least reliable network connection. That network connection could be reengineered to be a dual-connected connection, thereby ensuring the system can survive one network failure at any time.

### NFS Dependencies Outside the Datacenter

The network file system protocol commonly used on Unix systems is called NFS. It has a feature whereby if the server goes down, the clients will freeze until it comes back up. This feature is useful in situations in which one would rather wait than have the client software lose data because a file system magically disappeared. Under this arrangement, an NFS server is a hard dependency for any client.

When Tom was at Bell Labs, a customer configured his desktop machine to be an NFS server and started providing some useful data there. Soon, many machines had automounted the disk volume from his machine, including some very important servers in the datacenter.

Then the customer powered off his desktop machine and left on vacation. All the machines that had accessed the data froze, waiting for the desktop machine to become live again. The SAs had to decide whether to get corporate security to open his office door to boot up his machine or to reboot some very important servers.

In hindsight, the servers should have been configured to mount NFS volumes only from machines directly managed by the SA team, or possibly just configured not to mount from other NFS servers at all.

### Dependency Alignment

Another engineering trick is to realign dependencies into smaller failure domains. A **failure domain** is all the services, locations, and users that are adversely affected when a particular component fails or is taken out of service.

Good engineering requires taking the time to think through how we can shrink failure domains, or divide large failure domains into smaller, isolated failure domains.

### Hardware Failure Domains Suppose three servers were being set up. Each server was made up of the CPU chassis, an external disk chassis, and its monitor. The original plan was to have one power strip for all the CPUs, one for all the disk chassis, and one for all the monitors. This made for a very uniform and elegant wiring diagram. However, it also meant that if, for example, the power strip for the disks died, all three servers would die. A better design was to divide the system into three smaller failure domains: Each power strip would connect to the CPU, disk, and monitor of a particular server. Each combination of power strip, CPU, disk, and monitor would be an isolated failure domain. In the original design the entire lot was one big failure domain.

A similar situation often happens in network engineering. Often a network team sets up VPN servers in locations around the world. At each location there is a redundant pair of VPN servers along with supporting devices such as network switches, power strips, and so on. The engineers configure each half of the pair to be an independent failure domain: one VPN device, power strip, UPS, and switch. A careless design or installer can create cross-dependencies that widen the failure domain to the size of the entire installation.

### Service Failure Domains The next example involves a common best practice used by Microsoft Windows administrators. When a user logs into an ActiveDirectory domain, an MS Windows login script runs that sets up the user’s environment, configuring printers and other needed resources. The best practice is to have this login script download from the same server as the user’s home directory. This aligns the dependency on

the file server with the dependency on that startup script. In fact, everything the script depends on should also be replicated to that server.

By aligning all the dependencies to the same machine that hosts their home directory, we know that the login process will have all of its dependencies available, or the process won’t start at all. If the server with the home directory is unavailable, it doesn’t matter that the other dependencies are also unavailable; the users can’t log in anyway. Without such alignment, a single outage would affect all users. Replicating the script every time it is updated seems like a lot of work, but DFS Replication Service makes such replication automatic.

This pattern is repeated in other places. Consider a server farm that hosts many web servers, each of which depends on database servers. We can minimize the external dependencies of a web site by making sure that the web server and the databases it accesses are all on the same machine, or are all on machines on the same switch. It may be easier to simply land each new web site on whatever servers are available. By taking the time to consider the dependencies, we build a more reliable service.

Such alignments do not require tracing every cable and power cord. We can make matters easier by simply planning ahead. First we engineer each rack to be independent—self-sufficient in its networking and power by having its own network switch and power distribution units.

Now we make sure that all servers for a given service that have hard dependencies on each other are **rack aligned** or located in the same rack. Doing so limits the number of external hard dependencies that the service has and reduces the failure domain of the rack, because there are fewer services in any given rack.

If a service has components that are replicas or are in a master/slave configuration, we make sure that the replicas are in different racks, and that the master and slave are not in the same rack. Doing so means that the service has a soft dependency on any given rack, rather than a hard dependency.

### Location Failure Domains A common design pattern is to make each office self-sufficient for a limited and specific set of services. For example, one might define that if an office loses connectivity to the rest of the company, the following services should still work: DNS, DHCP, ActiveDirectory, home directories, email, and printing. Service would degrade in predictable ways in this scenario: Email sent outside of the office would queue up until connectivity was repaired, and so on.

Once these requirements are defined, we can engineer those services to depend on only local services. All other services can rely on services outside of the office.

When introducing a new service, we must decide if it will be part of the selfcontained realm. For example, a new VoIP service might be so fundamental to the company’s operations and safety that it would be part of the self-contained list.

The same applies to applications. A new accounting-related service would be engineered to depend on only services in the same building in which it is deployed. If all of its users are in a single office, it might be deployed in a computer room in that office.

Sometimes, however, self-sufficient sites might not be economical. For example, some companies might set up hundreds of tiny sales offices, often with fewer than a dozen people in each. It would be cost-prohibitive to have each office set up with its own local services. Instead each office may be designed with no servers at all, relying on regional hub datacenters to provide DNS, DHCP, and all other services. With this approach, the amount of server equipment needed in each office is eliminated. If the office would be dead in the water when its WAN connection is down, having services working may be unimportant.

### Catch-22 Dependencies

A startup web company found itself short of disk space and, to save money, exported some free disk space from a Unix desktop via NFS. This disk ended up being mounted on all the servers, since it provided a common component. After a block-wide power failure that outlasted the UPS, the company’s network would not come up. The workstation couldn’t finish booting without the DNS server running, and the DNS server required the NFS partition to be available to complete its own boot process. Shortly afterward, the company hired an SA.

## Decoupling Hostname from Service Name

Picking the names for machines often becomes a hotly contested debate rivaling the controversies over topics such as Apple versus Microsoft, Vim versus Emacs versus Sublime, and whether sandwiches should be cut lengthwise or diagonally.

We won’t enter into this debate. However, once you’ve picked a naming scheme, we have one bit of advice: Decouple the purpose of the machine from the name of the machine.

For example, consider an email service running on a machine called mail. What do you do when it is time to replace the mail server with a new one? You could call the new machine newmail or mail2, but then users would need to reconfigure their email clients, which would be a hassle. You could call the new machine mail and rename the old machine to be oldmail, but now everyone would start referring to the new host, possibly before it is ready.

A better choice is to design your systems to use aliases for the service name. For example, name the machines simply server1, server2, server3, and so on, but provide aliases such as mail, calendar, wiki, and so on. These aliases can be changed to point at new machines as needed.

In DNS, aliases are configured using their CNAME record, which lists the actual machine name, or A and AAAA records, which list the IP address of the machine. The benefit of using a CNAME is that it does not need to be updated if the IP address changes.

Very old software packages might not support the use of aliases. We consider this to be a bug. Also note that SSL certificates are based on the hostname that appears in the URL, not the hostname. Therefore web-based services that use HTTPS need additional planning.

### Case Study: Splitting the Central Machine

As a small company, Synopsys started with the typical configuration of one central administrative machine. It was the Network Information Service (NIS) master, DNS master, time server, print server, console server, email server, SOCKS relay, token-card authentication server, boot server, NetApp admin host, file server, Columbia Appletalk Protocol (CAP) server, and more. It was also the only head—keyboard and monitor—in the machine room, so it was the machine used to remotely access all other machines. Plus SAs used it to check their email rather than run back to their desks.

As the SA group grew and new SAs were working at this machine’s console, a particular error began to happen. Occasionally someone using the administrative machine to access other hosts would get confused and shut down the administrative machine instead of the intended host. Because everything relied on the central machine, this accident effectively brought down the whole company network at once.

The time had come to split the functionality of the machine across multiple servers, not only because of those occasional slips but also because the machine was becoming increasingly unreliable and overloaded. At this point, the central machine had so many services running on it that just figuring out what they all were was a large task in itself.

Some services moved relatively easily because they were associated with a service-based name. Others were more difficult because they were tied to IP addresses.

Likewise, do not hardcode services to particular IP addresses. Listing an IP address in configuration files leads to trouble. List hostnames instead. If the IP address of a machine must be changed, we can change it in one place—DNS— and no configuration files must be altered. There are countless situations where the IP address space of a particular VLAN must change, usually due to mergers, reorganizations, or the need to physically move machines. In each case the project is made unnecessarily complex due to hardcoded IP addresses. If a service doesn’t work, debugging becomes a hunt for the mystery location of a hardcoded IP address buried deep in a configuration file.

There are some exceptions to this rule. First, DNS clients must be configured using IP addresses, due to the Catch-22 that would happen if DNS clients needed to use DNS to find the IP address of a DNS server. NICs and network equipment are another exception for similar reasons. However, there are still a couple of ways that you mitigate against an eventual need to change IP addresses. Look for mechanisms that allow you to use a different service-based IP address in the configuration files, rather than the primary IP address of the machine itself.

### Using IP Aliases

There are some other cases where IP addresses are commonly hardcoded into configurations. For example, IP addresses are often used in firewall rules. Likewise, the IP addresses of DHCP servers are often found in the router configurations. Routers are configured to relay broadcast DHCP requests from a particular network to a specified set of DHCP servers. This is called a DHCP relay or IP helper configuration.

DHCP servers can usually be configured with a second IP address (often known as a secondary IP address or an IP alias) that is used for the service, and can be easily moved to another machine. A similar approach can often be used for other services that require firewall rules, with the secondary IP address being used for the service and in the firewall rules, making it much easier to move the service to another machine, along with the IP alias. When this secondary IP address is a loopback address that is advertised onto the network as host route, then it is truly independent of the VLAN, and can be easily moved, even to a completely different datacenter.

## Support

During the planning and engineering phase, consider how the service will be supported during its lifetime. Support includes technical aspects such as detecting problems, patching, upgrading, performing backups, and scaling the service. It also includes people and processes, such as identifying and training the support groups, developing mechanisms for customers to report problems and submit requests (and how those problems and requests should be handled), and providing documentation for the customers and the support teams.

You need to know when there are problems with the service and when you should be thinking about scaling it up—therefore the service must be monitored. You will need to put some thought and effort into engineering the appropriate monitoring solution. You should also consider which requests users may make in relation to this service, and how those will be handled. Which automation and processes should be in place? Which documentation is needed to support the service?

### Monitoring

It isn’t a service if it isn’t monitored. If there is no monitoring, then you’re just running software. A service should be monitored for availability, problems, performance, and capacity-planning. See [Monitoring Distributed Systems](../principles/MONITORING.DISTRIBUTED.SYSTEMS.md) for monitoring practices.

The helpdesk, or front-line support group, must be automatically alerted to problems with the service so that they can start fixing them before too many people are affected. A customer who notices a major problem with a service and has to call in to report it before the service provider discovers and begins to fix the problem is getting a very low standard of service. Customers do not like to feel that they are the only ones paying attention to problems in the system. Conversely, problems you can detect and fix before they are noticed are like trees that fall in the forest with no one around to hear them. For example, if an outage happens over the weekend and you are alerted in time to fix it before Monday morning, your customers don’t even need to know that anything went wrong. (In this case, you should announce by email that the problem has been resolved, so that you receive credit.)

Likewise, the SA group should monitor the service on an ongoing basis from a capacity-planning standpoint. Depending on the service, capacity planning can include network bandwidth, server performance, transaction rates, licenses, and physical-device availability. As part of any service, SAs can reasonably be expected to anticipate and plan for growth. To do so effectively, usage monitoring needs to be built in as a part of the service.

During the planning and engineering phase of the project, you need to define what should be monitored, and how. You need to define which events should trigger alerts, and what the priority of those alerts should be. You need to work with the monitoring team to understand how this service gets integrated into the existing tools, and which work is required on both sides to make that happen.

You also need to define the process for adding new components into the monitoring system as the system is scaled up or upgraded. Likewise, you need to define how components are removed from the monitoring system as they are retired or repurposed. Ideally these changes should be automated, and driven from the inventory system. However, that means that this automation needs to be developed, and the processes need to be clearly defined and documented.

### Support Model

The support model needs to specify who supports the various components of the service, and what the OLAs and SLAs for each component are. The OLA and SLA for the service need to take into account the OLAs and SLAs of the constituent parts, as well as the design of the service itself.

For example, if a service has components in multiple datacenters around the world, who provides hands-on support in the datacenters, and what are their SLAs? What are the vendors’ SLAs for the various hardware components—do these differ in different locations? What is the SLA for WAN connectivity? Who provides local network support, and who provides WAN support? Who supports the OS that the application runs on? Who supports the application itself? What are the processes, contact information, SLAs, and escalation paths for all of these groups?

In a small or midsize company, many of these roles will be performed by the same people, who all know each other. In large companies, there will be different teams supporting each component, they won’t all know each other, and the processes and communication paths may not be clear. Make sure that all of this information is tracked down and documented in advance, rather than finding out in the midst of an incident that you do not know how to contact the on-site support staff in a remote location.

### Service Request Model

For most services, the local engineering plan should also include a service request model (SRM). The SRM defines which requests users can make relating to this service, who can make those requests, how they make those requests, which approvals are required, who acts on the requests, which change control policies apply, and which SLA is used for turning around those requests.

Because each service is different, work with the vendor and stakeholders to define which tasks are required, document them, and then practice them in a test environment. Try to estimate the volume of requests of each type, and understand from the stakeholders what would trigger a request of each type and what kind of turnaround time they would expect on each request.

Consider whether it makes sense to automate these requests. For example, if a user-add request will always be generated as a result of a new hire, can you link to the HR system, so that these requests are automated? This will eliminate the need for someone to remember to submit that request, and someone else to process it. Likewise, user-deletes should occur automatically when someone leaves the company, and perhaps when someone moves to a different role in a group that does not need access. Alternatively, moves could generate access review tasks for the new manager and the service owner.

### Documentation

As part of support planning, operational procedures must be defined. These are different for every service, but generally include backups and restores, business continuity or disaster recovery plans, tasks related to onboarding new users, disconnecting users who are leaving, performing periodic tasks, and anything else required to keep the service running.

For each routine task, there must be a runbook that details how the task should be performed. Define the runbooks by performing each task in a test environment. What is learned through this process may lead to changes in the architecture or local engineering plan. For example, you might discover that backup and restore procedures were not considered until after the architecture was defined. This may require changes to the architecture. Alternatively, what is learned may be a minor point, but one that requires changes in the local engineering plan—for example, to define that each machine must have an additional fiber interface on a dedicated backups network. This is an iterative process.

The documentation must also include the steps to take when debugging the service. This documentation should cover which data needs to be gathered, and how, as well as steps to examine that data and use it to debug the problem. There should be documentation for the helpdesk telling these personnel how to perform basic health checks on the service, verify problems that might be reported by end users or the monitoring system, and debug and resolve the most common problems. [Effective Troubleshooting](../practices/EFFECTIVE.TROUBLESHOOTING.md) describes a systematic approach to investigating problems. The documentation should describe how to follow those steps for this service.

The helpdesk documentation should also specify which information needs to be gathered and passed on to the next support level if the issue remains unresolved and needs to be escalated. The documentation for the more senior SA team must include the vendor’s contact information and support contract number, all the components that the service depends on, and contact information for all the other teams that support those components. The contact information for other teams should be provided through a link to a well-known team support page that each team maintains itself, rather than duplicating information that may get stale into many different support documents.

## Summary

Service engineering involves designing how a product will be configured and used in your environment. With a simple service, it is a matter of installing the software. Of course, most services are not simple. In such a case, we must design the service architecture and local engineering plans.

The service architecture is the high-level design. It describes how the parts will work together, which resources are required, and so on. Local engineering plans describe specifics for a particular device, such as its model, name, and IP address.

While a lot of science and methodology goes into such engineering, a few rules of thumb are helpful: Follow vendor recommendations, mirror your boot disk, use service-class hardware, don’t run a service on a machine under your desk, restrict console access, and leverage the existing infrastructure as much as possible.

Service architecture should strive for simplicity. Revise the design over and over, tweaking and improving it. Consider how the dependencies will affect the service. Consider alternative plans that reduce dependencies by examining failure domains. We can align dependencies to make a system less susceptible to failures. We can align along hardware failure domains, service failure domains, and location failure domains.

When possible, do not expose actual hostnames to users. Instead, expose hostname aliases that can be moved to other machines. Do not hardcode IP addresses unless doing so is unavoidable.

The service architecture should be supportable. It should indicate how the system will be monitored, how it will be accessed for administrative purposes, and so on. Documentation should include a runbook for common tasks, expected failure modes, and debugging typical problems.
