# Chapter 21

## Service Conversions

Source: [The.Practice.of.System.and.Network.Administration-Vol.I-2017-21.pdf](./The.Practice.of.System.and.Network.Administration-Vol.I-2017-21.pdf), PDF pages 1–13.

The case studies and first-person accounts below reflect the source authors’ experience, as published in 2017.

Sometimes, you need to convert your customer base from an existing service to a new replacement service. The existing system may not be able to scale or may have been declared to be at “end of life” by the vendor, requiring you to evaluate new systems. Or, your company may have merged with a company that uses different products, and both parts of the new company need to integrate their services with each other. Perhaps your company is spinning off a division into a new, separate company, and you need to replicate and split the services and networks so that each part is fully self-sufficient. Whatever the reason, converting customers from one service to another is a task that SAs often face.

As with many things in system and network administration, your goal should be for the conversion to go smoothly and be completely invisible to your customers. To achieve or even approach that goal, you need to plan the project very carefully. This document describes some of the areas to consider in that planning process.

As with many high-level system administration tasks, a successful conversion depends on having a solid infrastructure in place. Rolling out a change to the whole company can be a very visible project, particularly if there are problems. You can decrease the risk and visibility of problems by rolling out the change slowly, starting with the SAs and then the most suitable customers. With any change you make, be sure that you have a backout plan and can revert quickly and easily to the pre-conversion state, if necessary.

We have seen how an automated patching system can be used to roll out software updates, and we’ve seen how to build a service, including some of the ways to make it easier to upgrade and maintain ([Service Launch: DevOps](SERVICE.LAUNCH.DEVOPS.md)). These techniques can be instrumental parts of your roll-out plan.

Communication plays a key role in performing a successful conversion. It is never wise to change something without making sure that your customers know what is happening and have told you of their concerns and timing constraints.

This document touches on each of those areas, along with two general approaches to conversions. You need to plan every step of a conversion well in advance to pull it off successfully with minimum impact on your customers. This document should shape your thinking in that planning process.

##### An Invisible Change

When AT&T split off Lucent Technologies, the Bell Labs research division was split in two. The SAs who looked after that division had to split the Bell Labs network so that the people who were to be part of Lucent would not be able to access any AT&T services, and vice versa. Some time after the split had been completed, one of the researchers asked when it was going to happen. He was very surprised when he was told that it had been completed already, because he had not noticed that anything had changed. The SAs considered this the best indicator of their success.

### 21.1 Minimizing Intrusiveness

When planning the conversion roll-out, pay close attention to how the conversion impacts the customer. Aim for the conversion process to disrupt the customer as little as possible. Try to make it seamless.

Does the conversion require a service interruption? If so, how can you minimize the time that the service is unavailable? When is the best time to schedule the interruption in service so that it has the least impact?

Does the conversion require changes on each customer’s workstation? If so, how many changes will there be, how long will they take to implement, and can you organize the conversion so that the customer is disturbed only once?

Does the conversion require that the customers change their work methods in any way? For example, will they have to use new client software? Can you avoid changing the client software? If not, do the customers need training? Sometimes training is a larger project than the conversion itself. Are the customers comfortable with the new software? Are their SAs and the helpdesk familiar enough with the new and the old software that they can help with any questions the customers might have? Have the helpdesk scripts ([Defining Processes for Staff](CUSTOMER.SUPPORT.md#277-defining-processes-for-staff)) been updated?

Look for ways to perform the change without interrupting the service, without visiting each customer, and without changing the workflow or user interface. Make sure that the support organization is ready to provide full support for the new product or service before you roll it out. Remember, your goal is for the conversion to be so smooth that your customers may not even realize that it has happened. If you can’t minimize intrusiveness, at least you can make the intrusion fast and well organized.

##### The Rioting Mob Technique

When AT&T was splitting into AT&T, Lucent, and NCR, Tom’s SA team was responsible for splitting the Bell Labs networks in Holmdel, New Jersey. At one point, every host needed to be visited to perform several changes, including changing its IP address. A schedule was announced that listed which hallways of offices would be converted on which day. Mondays and Wednesdays were used for conversions; Tuesdays and Thursdays were for fixing problems that arose; Fridays were unscheduled, in the hope that the changes wouldn’t cause any problems that would make the SAs lose sleep on the weekends.

On conversion days, the team used what they called the “rioting mob technique.” At 9 AM, the SAs would stand at one end of the hallway. They’d psych themselves up, often by chanting, and move down the hallways in pairs. Two pairs were PC technicians, and two pairs were Unix technicians—one set for the left side of the hallway and another for the right side. As the technicians went from office to office, they shoved out the inhabitants and went machine to machine, making the needed changes. Sometimes machines were particularly difficult or had problems. Rather than trying to fix the issue themselves, the technicians called on a senior team member to solve the problem and they moved on to the next machine. Meanwhile, a final pair of people stayed at command central, where SAs could phone in requests for IP addresses and provide updates to the host, inventory, and other databases.

The next day was spent cleaning up anything that had broken and then discussing the issues to refine the process. A brainstorming session revealed what had gone well and what needed improvement. The technicians decided that it would be better to make one pass through the hallway, calling in requests for IP addresses, giving customers a chance to log out, and identifying nonstandard machines for the senior SAs to focus on. On the second pass through the hallway, everyone had the IP addresses they needed, and things went more smoothly. Soon, they could do two hallways in the morning and all the cleanup in the afternoon.

The brainstorming session between each conversion day was critical. What the technicians learned in the first session inspired radical changes in the process. Eventually, the brainstorming sessions were not gathering any new information; the breather days became planning sessions for the next day. Many times, a conversion day went smoothly and was completed by lunchtime, and the problems resolved by the afternoon. The breather day became a normal workday.

Consolidating all of an individual customer’s disruption to a single day was a big success. Customers were expecting some kind of outage but would have found it unacceptable if the outage had been prolonged or split up over many instances.

The complete story is documented in Limoncelli, Reingold, Narayan & Loura (1997).

### 21.2 Layers Versus Pillars

A conversion project, like any project, is divided into discrete tasks, some of which have to be performed for every customer. For example, with a conversion to new calendar software, the new client software must be rolled out to all the desktops, accounts need to be created on the server, and existing schedules must be converted to the new system. As part of the project planning for the conversion, you need to decide whether to perform these tasks in layers or in pillars.

- With the *layers* approach, you perform one task for all the customers before moving on to the next task and doing that for all of the customers.
- With the *pillars* approach, you perform all the required tasks for each customer at once, before moving on to the next customer.

Think of baking a large cake for a dozen people versus baking 12 cupcakes, one at a time. You’d want to bake one big cake (the layers approach). But suppose instead you were making omelets. People would want different things in their omelets—it wouldn’t make sense to make just one big one (the pillars approach).

Tasks that are not intrusive to the customer, such as creating the accounts in the calendar server, can be safely performed in layers. However, tasks that are intrusive for a customer should be performed in pillars. Examples of more intrusive tasks are installing client software that requires a reboot or getting the customer to initialize accounts on a new system.

With the pillars approach, you need to schedule one appointment with the customer, rather than several. By performing all the tasks at once, you disturb each customer only once. Even if it is for a slightly longer time, a single intrusion is typically less disruptive to your customer’s work than many small intrusions.

A hybrid approach achieves the best of both worlds: Group all the customervisible interruptions into as few periods as possible. Make all other changes silently.

##### Case Study: Pillars Versus Layers at Bell Labs

When AT&T split off Lucent Technologies and Bell Labs was divided in two, many changes needed to be made to each desktop to convert it from a Bell Labs machine to either a Lucent Bell Labs machine or an AT&T Labs machine. Very early on, the SA team responsible for implementing the split realized that a pillars approach would be used for most changes, but that sometimes the layers approach would be best. For example, the layers approach was used when building a new web proxy. The new web proxies were constructed and tested, and then customers were switched to their new proxies. However, more than 30 changes had to be made to every Unix desktop, and it was determined that they should all be made in one visit, with one reboot, to minimize the disruption to the customer.

There was great risk in that approach. What if the last desktop was converted and then the SAs realized that one of those changes was made incorrectly on every machine? To reduce this risk, sample machines with the new configuration were placed in public areas, and customers were invited to try them out. This way, the SAs were able to find and fix many problems before the big changes were implemented on each customer workstation. This approach also helped the customers become comfortable with the changes.

Some customers were particularly fearful because they lacked confidence in the SA team. These customers were gently escorted to the public machines and asked to log in, and problems were debugged in real time. This calmed customers’ fears and increased their confidence.

E-commerce sites, while looking monolithic from the outside, can also think about their conversions in terms of layers and pillars. A small change or even a new software release can be rolled out in pillars, one host at a time, if the change interoperates with the older systems. Changes that are easy to do in batches, such as imports of customer data, can be implemented in layers. This is especially true of nondestructive changes, such as copying data to new servers.

### 21.3 Vendor Support

When doing large conversions, make sure that you have vendor support. Contact the vendor to find out if there are any pitfalls. This can prevent major problems. If you have a good relationship with a vendor, that vendor should be willing to be involved in the planning process, sometimes even lending personnel. If not, the vendor may be willing to make sure that its technical support hotline is properly staffed on your conversion day or that someone particularly knowledgeable about your environment is available.

Don’t be afraid to reveal your plans to a vendor. There is rarely a reason to keep such plans secret. Don’t be afraid to ask the vendor to suggest which days of the week are best for receiving support. It can’t hurt to ask the vendor to assign a particular person from its support desk to review the plans as they are being made so that the vendor will be better prepared if you do call during the upgrade with a problem. Good vendors would rather review your plans early than discover that a customer has a problem halfway through an upgrade that involves unsupported practices.

### 21.4 Communication

Although the guiding principle for a conversion is that it be invisible to the customer, you still have to communicate the conversion plan to your customers. Indeed, communicating a conversion far in advance is critical.

By communicating with the customers about the conversion, you will find people who use the service in ways you did not know about. You will need to support them and their use of the new system. Any customers who use the system extensively should be involved early in the project to make sure that their needs will be met. You should find out about any important deadlines that your customers have or any other times when the system needs to be absolutely stable.

Customers need to know what is taking place and how the change is going to affect them. They need to be able to ask questions about how they will perform their tasks in the new system and they need to have all their concerns addressed. Customers need to know in advance whether the conversion will require service outages, changes to their machines, or visits to their offices.

Even if you expect the conversion to go seamlessly, with no interruption or visible change for the customers, they still need to know that it is happening. Use the information you’ve gained to schedule it for minimum impact, just in case something goes wrong. If everything goes smoothly, the pre- and post-conversion communication helps to improve the customers’ perception and visibility of the SA team. If you don’t communicate the change, they will just assume that you are not doing anything. If you communicate the change and something fails, at least the customers know what, when, and why you were doing something, and they will be more tolerant of the resulting disruption.

Have the high-level goals for the conversion planned and written out in advance; it is common for customers to try to add new functionality or new services as requirements during an upgrade planning process. Adding new items increases the complexity of the conversion. Strike a balance between the need to maintain functionality and the desire to improve services.

### 21.5 Training

Related to communication is training. If any aspect of the user experience is going to change, training should be provided. This is true whether the menus are going to be slightly different or entirely new workflows will be required.

Most changes are small and can be brought to people’s attention via email. However, for roll-outs of large, new systems, training is critical to the success of their introduction to an organization. The less technical the customers, the more important that training be included in your roll-out plans.

Creating and providing the actual training is usually out of scope for the SA team doing the service conversion, but SAs may need to support training efforts. Work closely with the customers and management driving the conversion to discover any plans for training support well in advance. Nontechnical customers may not realize the level of effort required by SAs to set up a training room with 5 to 15 workstations, special demo machines and services, or special firewall rules.

### 21.6 Gradual Roll-Outs

When performing a roll-out, whether it is a conversion, a new service, or an update to an existing service, you should do so gradually to minimize the potential impact of any failures.

Using the “one, some, many” technique, you would start by converting your own system to the new service. Test and perfect the conversion process, and test and perfect the new service before converting any other systems. When you cannot find any more problems, convert a few of your coworkers’ desktops; debug and fix any problems that arise from that process and have them test the new system. Expand the test group to cover all the SAs before starting on your customers. When you have successfully converted the SAs, start with small groups of customers and increase the group size over time.

There are a few strategies for picking groups of customers. Random selection is one method. However, when possible be selective: start with customers who are the least risk averse, most technical, or most able to give high-quality feedback. At Bell Labs one team requested to be the first group on roll-outs. They had a particularly good relationship with the IT team. They accepted the risks because they prided themselves on being so cutting edge. They were also highly technical and diligently submitted bug reports. A school district rolling out a new student information system rolled out the grade-tracking subsystem to the high school a year before rolling it out to the rest of the district; the lesson planning subsystem was enabled for the science department a year before anyone else saw it because its members were more tech-savvy than the other departments and could support themselves during the buggy first few rounds.

Likewise, reserve the most risk-averse users for the end of the roll-out process. Some teams may be more risk averse due to business requirements or their temperament. The CEO and anyone involved in approving your budget should be last so that they see systems only after the bugs have been worked out.

##### Upgrading Google Servers

Google’s server farm includes thousands of computers; the precise number is an industry secret. When upgrading thousands of redundant servers, Google has massive amounts of automation that first upgrades a single host, then 1 percent of the hosts, and finally batches of hosts, until all are upgraded. Between each set of upgrades, testing is performed, and an operator has the opportunity to halt and revert the changes if problems are found. Sometimes, the gap of time between batches is hours; at other times, days.

### 21.7 Flash-Cuts: Doing It All at Once

In a flash-cut, all users are moved to a new service at the same time. You should never do this.

Flash-cuts are highly risky. If there is a problem with the new service, all users will be exposed to the problem. The conversion will go much more smoothly if you can convert a few willing test subjects to the new system first. Avoiding a flash-cut may mean budgeting in advance for duplicate hardware so that you will have a pool of conversion machines to run in parallel. When you prepare your budget request, remember to think about how you will perform the conversion roll-out.

In other cases, you may be able to use features of your existing technology to slowly roll out the conversion. For example, if you are renumbering a network or splitting a network, you might use an IP multinetting network in conjunction with DHCP to permit unconverted and converted hosts to operate on the same VLAN simultaneously without the need for additional hardware.

Alternatively, you may be able to make both old and new services available simultaneously and encourage people to switch during the overlap period. That way, they can try out the new service, get used to it, report problems, and switch back to the old service if they prefer. Such a strategy gives your customers an “adoption” period. This approach is commonly used in the telephone industry when a change in phone number or area code is introduced. For a few months, both the old and new numbers work. In the following few months, the old number gives an error message that refers the caller to the new number. Then the old number stops working, and sometime later it becomes available for reallocation.

The more complex a change from one service to another, the more tempting it is to do a flash-cut. This is counter-intuitive. For example, transitioning a university-wide email service from one system to another is very complex. There are many moving parts: DNS, client configuration, the server itself, data migration, and so on. It becomes very tempting to simply declare a “flag day” when the old service will disappear, messages will be migrated to the new system, and the new system will go live. We’ve seen this very example many times. All of them ended in failure: The process to migrate messages took longer than expected, the new system was incompatible with a critical application, performance was slow, training the entire organization about how to use the new system became impossible to do at scale, and so on. In every one of these cases, the team was left scratching their collective head, unable to figure out how their excellent plan had failed.

All of these problems would have been prevented if they had converted small groups one at a time rather than doing a flash-cut. The migration process is complex, but can be improved after each batch of users is processed. The process can be paused to fix scaling and performance issues. Training the first group teaches you how to better train future groups.

While flash-cuts seem like a way to reduce complexity, the truth is that they increase the risk of failure. Flash-cuts should be avoided at all costs, even if this means going back to the drawing board and reengineering the design to let it permit gradual conversions.

##### Physical Network Conversion

When a midsize company converted its network wiring from thin Ethernet to 10Base-T, it divided the problem into two main preparatory components and had a different group attack each part of the project planning. The first group had to get the new physical wiring layer installed in the wiring closets and cubicles. The second group had to make sure that every machine in the building was capable of supporting 10Base-T, by adding a card or adapter.

The first group ran all the wires through the ceiling and terminated them in the wiring closets. Next, the group went through the building and pulled the wires down from the ceiling, terminated them in the cubicles and offices, and tested them, visiting each cubicle or office only once.

When both groups had finished their preparatory work, they gradually went through the building, moving people to offices with new wiring but leaving the old cabling in place so that they could switch back if there were problems.

This conversion was done well from the point of view of avoiding a flashcut and converting people over gradually. However, the customers found it too intrusive because they were interrupted three times: once for wiring to their work areas, once for the new network hardware in their machines, and finally for the actual conversion. Although it would have been very difficult to coordinate, and would have required extensive planning, the teams could have visited each cubicle together and performed all the work at once. Realistically, though, this would have complicated and delayed the project too much.

Having better communication initially would have made things easier on the customers, letting them know all the benefits of the new wiring, apologizing in advance for the need to disturb them three times (one of which would require a reboot), and scheduling the disturbances for more convenient times. Customers find interruptions less of an annoyance if they understand what is going on, have some control over the scheduling, and know what they are going to get out of it ultimately.

Sometimes, a conversion or a part of a conversion must be performed simultaneously for everyone. For example, if you are converting from one corporate-wide calendar server to another, where the two systems cannot communicate and exchange information, you may need to convert everyone at once; otherwise, people on the old system will not be able to schedule meetings with people on the new system, and vice versa.

Performing a successful flash-cut requires a lot of careful planning and some comprehensive testing, including load testing. Persuade a few key users of that system to test the new system with their daily tasks before making the switch. If you get the people who use the system the most heavily to test the new one, you are more likely to find any problems with it before it goes live, and the people who rely on it the most will have become comfortable with it before they have to start using it in earnest. People use the same tools in different ways, so more testers will gain you better feature-test coverage.

For a flash-cut, two-way communication is particularly critical. Make sure that all your customers know what is happening and when, and that you know and have addressed their concerns in advance of the cutover.

If a flash-cut is truly unavoidable, find ways to do testing without a launch. As described in [Service Launch: DevOps](SERVICE.LAUNCH.DEVOPS.md), set up a lab environment and practice, practice, practice.

##### Phone Number Conversion

In 2000, British Telecom converted the city of London from two area codes to one and lengthened the phone numbers from seven digits to eight, in one large number change. Numbers that were of the form (171) xxx-xxxx became (20) 7xxx-xxxx, and numbers that were of the form (181) xxx-xxxx became (20) 8xxx-xxxx. More than six months before the designated cutover date, the company started advertising the change; also, the new area code and new phone number combination started working. For a few months after the designated cutover date, the old area codes in combination with the old phone numbers continued to work, as is usual with telephone number changes.

However, local calls to London numbers beginning with a 7 or an 8 went from seven to eight digits overnight. Because this sudden change was certain to cause confusion, British Telecom telephoned every single customer who would be affected by the change to explain, person to person, what the change meant and to answer any questions that their customers might have. Now that’s customer service!

### 21.8 Backout Plan

When rolling out a conversion, it is critical to have a backout plan. A *conversion,* by definition, means removing one service and replacing it with another. If the new service does not work correctly, the customer has been deprived of one of the tools that he or she uses to perform work, which may seriously affect the person’s productivity.

If a conversion fails, you need to be able to restore the customer’s service quickly to the state it was in before you made any changes and then go away, figure out why it failed, and fix it. In practical terms, this means that you should leave both services running simultaneously, if possible, and have a simple, automated way of switching someone between the two services.

Bear in mind that the failure may not be instantaneous or may not be discovered for a while. It could be a result of reliability problems in the software, it could be caused by capacity limitations, or it may be a feature that the customer uses infrequently or only at certain times of the year or month. So you should leave your backout mechanism in place for a while, until you are certain that the conversion has been completed successfully. How long? For critical services, one significant reckoning period is a good idea, such as a fiscal quarter for a company, or a semester for a university.

#### 21.8.1 Instant Roll-Back

When performing a conversion, it is nice if your backout plan enables you to instantly roll everything back to a known working state if a problem is discovered. That way, any customer disruption resulting from a problem with the new system can be minimized.

How you provide instant roll-back depends on the conversion that you are performing. One component of providing instant roll-back might be to leave the old systems in place. If you are simply pointing customers’ clients to a new server, you can switch back and forth by changing a single DNS record. To make DNS updates happen more rapidly, set the time to live (TTL) field to a lower value—5 minutes, perhaps—well in advance of making the switch. Then, when things are stable, set the TTL back to whatever value is usually in place.

Another approach that achieves instant roll-back is to perform the conversion by stopping one service and starting another. In some cases, you may have two client applications on the customers’ machines—one that uses the old system and another that uses the new one. This approach works especially well when the new service has been running for tests on a different port than the existing service.

For services that are behind a load balancer, you may be able to bring up one or more new instances that are running the new service, and direct a particular group of customers to those instances. This approach enables a quick roll-back while also providing a slow staged roll-out, minimizing the impact of any problems.

Sometimes, the change being made is an upgrade of a software package to a newer release. If the old software is retained in a dormant state on the server while the new software is in use, you can instantly perform a roll-back by switching to the old software. Vendors can do a lot to make this difficult, but some are very good about making it easy. For example, if versions 1.2 and 1.3 of a server get installed in /opt/example-1.2 and /opt/example-1.3, respectively, but a symbolic link /opt/example points to the version that is in use, you can roll back the change by simply repointing the single symbolic link.

#### 21.8.2 Decision Point

A major difficulty with backout plans is deciding when to execute them. When a conversion goes wrong, the technicians tend to promise that things will work with “one more change,” but management tends to push toward starting the backout plan. It is essential to have decided in advance the point at which the backout plan will be put into use. For example, one might decide ahead of time that if the conversion isn’t completed within 2 hours of the start of the next business day, then the backout plan must be executed. Obviously, if in the first minutes of the conversion one meets insurmountable problems, it can be better to back out of what’s been done so far and reschedule the conversion. However, getting a second opinion can be useful. What is insurmountable to you may be an easy task for someone else on your team.

When an upgrade has failed, there is a big temptation to keep trying more and more things to fix it. We know we have a backout plan, and we know we promised to start reverting if the upgrade wasn’t complete by a certain time, but we keep on saying, “I just want to try one more thing.” It is natural to want to keep trying. It’s a good thing, actually. Most likely, we got where we are today by not giving up in the face of insurmountable problems. However, when a maintenance window is ending and we need to revert, we need to revert. Often, our egos won’t let us, which is why it can be useful to designate someone outside the process, such as our manager, to watch the clock and make us stop when we said we would stop.

Revert. There will be more time to try again later.

### 21.9 Summary

A successful conversion project requires lots of advance planning and a solid infrastructure. Customers will judge the conversion by how organized you were, how well you were able to minimize adverse impact to them, and whether the new system “just works.”

A conversion requires many individual steps. With the layers approach, you perform one step for all the customers before moving on to the next step. With the pillars approach, you perform all the required tasks for a particular customer before moving on to the next customer. Ideally a hybrid approach can be used where all the intrusive steps are grouped together (pillars) so customers receive one big intrusive service interruption, and the remaining steps are layered.

The principles for roll-outs of any kind, updates, new services, or conversions are the same. Test changes before they are deployed. Deploy slowly to small groups, then to larger groups. Use vendor support when needed. Have a backout plan, and decide ahead of time the criteria by which the backout plan will be activated. Avoid flash-cuts at all costs.

### Exercises

1. Explain the pillars versus layers metaphor and how you can use it to minimize the adverse impacts of service conversions on customers. 2. Which conversions can you foresee in your network’s future? Choose one, and build a plan for performing it with minimum customer impact. 3. Try to add an instant roll-back option to the plan you developed in Exercise 2.
