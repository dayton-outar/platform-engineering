# SRE Supplementary Material

Source: [Site.Reliability.Engineering-2016-32.pdf](./Site.Reliability.Engineering-2016-32.pdf), PDF pages 17–18, 36–84.

The first-person accounts and Google-specific practices below reflect the source authors’ perspective in 2016.

## PART V Conclusions

Having covered much ground in terms of how SRE works at Google, and how the principles and practices we’ve developed might be applied to other organizations in our field, it now seems appropriate to turn our view to *[Lessons Learned from Other Industries](LESSONS.OTHER.INDUSTRIES.md)* to examine how SRE’s practices compare to other industries where reliability is critically important.

Finally, Google’s VP for Site Reliability Engineering, Benjamin Lutch, writes about SRE’s evolution over the course of his career in his [conclusion](CONCLUSION.md), examining SRE through the lens of some observations on the aviation industry.

## APPENDIX A Availability Table

Availability is generally calculated based on how long a service was unavailable over some period. Assuming no planned downtime, Table A-1 indicates how much downtime is permitted to reach a given availability level.

*Table A-1. Availability table*

| Availability level | Allowed unavailability per year | Per quarter | Per month | Per week | Per day | Per hour |
| --- | --- | --- | --- | --- | --- | --- |
| 90% | 36.5 days | 9 days | 3 days | 16.8 hours | 2.4 hours | 6 minutes |
| 95% | 18.25 days | 4.5 days | 1.5 days | 8.4 hours | 1.2 hours | 3 minutes |
| 99% | 3.65 days | 21.6 hours | 7.2 hours | 1.68 hours | 14.4 minutes | 36 seconds |
| 99.5% | 1.83 days | 10.8 hours | 3.6 hours | 50.4 minutes | 7.20 minutes | 18 seconds |
| 99.9% | 8.76 hours | 2.16 hours | 43.2 minutes | 10.1 minutes | 1.44 minutes | 3.6 seconds |
| 99.95% | 4.38 hours | 1.08 hours | 21.6 minutes | 5.04 minutes | 43.2 seconds | 1.8 seconds |
| 99.99% | 52.6 minutes | 12.96 minutes | 4.32 minutes | 60.5 seconds | 8.64 seconds | 0.36 seconds |
| 99.999% | 5.26 minutes | 1.30 minutes | 25.9 seconds | 6.05 seconds | 0.87 seconds | 0.04 seconds |

Using an aggregate unavailability metric (i.e., "*X*% of all operations failed”) is more useful than focusing on outage lengths for services that may be partially available— for instance, due to having multiple replicas, only some of which are unavailable— and for services whose load varies over the course of a day or week rather than remaining constant.

See Equations 3-1 and 3-2 in [Embracing Risk](../principles/EMBRACING.RISK.md) for calculations.

## APPENDIX B A Collection of Best Practices for Production Services

***Written by Ben Treynor Sloss***

***Edited by Betsy Beyer***

### Fail Sanely

Sanitize and validate configuration inputs, and respond to implausible inputs by *both* continuing to operate in the previous state *and* alerting to the receipt of bad input. Bad input often falls into one of these categories:

*Incorrect data*

Validate both syntax and, if possible, semantics. Watch for empty data and partial or truncated data (e.g., alert if the configuration is *N*% smaller than the previous version).

*Delayed data*

This may invalidate current data due to timeouts. Alert well before the data is expected to expire.

Fail in a way that preserves function, possibly at the expense of being overly permissive or overly simplistic. We’ve found that it’s generally safer for systems to continue functioning with their previous configuration and await a human’s approval before using the new, perhaps invalid, data.

#### Examples

In 2005, Google’s global DNS load- and latency-balancing system received an empty DNS entry file as a result of file permissions. It accepted this empty file and served NXDOMAIN for six minutes for all Google properties. In response, the system now performs a number of sanity checks on new configurations, including confirming the presence of virtual IPs for *google.com*, and will continue serving the previous DNS entries until it receives a new file that passes its input checks.

In 2009, incorrect (but valid) data led to Google marking the entire Web as containing malware [May09]. A configuration file containing the list of suspect URLs was replaced by a single forward slash character (/), which matched all URLs. Checks for dramatic changes in file size and checks to see whether the configuration is matching sites that are believed unlikely to contain malware would have prevented this from reaching production.

### Progressive Rollouts

Nonemergency rollouts *must* proceed in stages. Both configuration and binary changes introduce risk, and you mitigate this risk by applying the change to small fractions of traffic and capacity at one time. The size of your service or rollout, as well as your risk profile, will inform the percentages of production capacity to which the rollout is pushed, and the appropriate time frame between stages. It’s also a good idea to perform different stages in different geographies, in order to detect problems related to diurnal traffic cycles and geographical traffic mix differences.

Rollouts should be supervised. To ensure that nothing unexpected is occurring during the rollout, it must be monitored either by the engineer performing the rollout stage or—preferably—a demonstrably reliable monitoring system. If unexpected behavior is detected, roll back first and diagnose afterward in order to minimize Mean Time to Recovery.

### Define SLOs Like a User

Measure availability and performance in terms that matter to an end user. See [Service Level Objectives](../principles/SERVICE.LEVEL.OBJECTIVES.md) for more discussion.

#### Example

Measuring error rates and latency at the Gmail client, rather than at the server, resulted in a substantial reduction in our assessment of Gmail availability, and prompted changes to both Gmail client and server code. The result was that Gmail went from about 99.0% available to over 99.9% available in a few years.

### Error Budgets

Balance reliability and the pace of innovation with error budgets (see [Motivation for Error Budgets](../principles/EMBRACING.RISK.md#motivation-for-error-budgets)), which define the acceptable level of failure for a service, over some period; we often use a month. A budget is simply 1 minus a service’s SLO; for instance, a service with a 99.99% availability target has a 0.01% “budget” for unavailability. As long as the service hasn’t spent its error budget for the month through the background rate of errors plus any downtime, the development team is free (within reason) to launch new features, updates, and so on.

If the error budget is spent, the service freezes changes (except urgent security and bug fixes addressing any cause of the increased errors) until either the service has earned back room in the budget, or the month resets. For mature services with an SLO greater than 99.99%, a quarterly rather than monthly budget reset is appropriate, because the amount of allowable downtime is small.

Error budgets eliminate the structural tension that might otherwise develop between SRE and product development teams by giving them a common, data-driven mechanism for assessing launch risk. They also give both SRE and product development teams a common goal of developing practices and technology that allow faster innovation and more launches without “blowing the budget.”

### Monitoring

Monitoring may have only three output types:

*Pages*

A human must do something *now*

*Tickets*

A human must do something within a few days

*Logging*

No one need look at this output immediately, but it’s available for later analysis if needed.

If it’s important enough to bother a human, it should either *require* immediate action (i.e., page) or be treated as a bug and entered into your bug-tracking system. Putting alerts into email and hoping that someone will read all of them and notice the important ones is the moral equivalent of piping them to */dev/null*: they will eventually be ignored. History demonstrates this strategy is an attractive nuisance because it can work for a while, but it relies on eternal human vigilance, and the inevitable outage is thus more severe when it happens.

### Postmortems

Postmortems (see [Postmortem Culture: Learning from Failure](../practices/POSTMORTEM.CULTURE.LEARNING.FAILURE.md)) should be blameless and focus on process and technology, not people. Assume the people involved in an incident are intelligent, are well intentioned, and were making the best choices they could given the information they had available at the time. It follows that we can’t “fix” the people, but must instead fix their environment: e.g., improving system design to avoid entire classes of problems, making the appropriate information easily available, and automatically validating operational decisions to make it difficult to put systems in dangerous states.

### Capacity Planning

Provision to handle a simultaneous planned and unplanned outage, without making the user experience unacceptable; this results in an "*N* + 2” configuration, where peak traffic can be handled by *N* instances (possibly in degraded mode) while the largest 2 instances are unavailable:

- Validate prior demand forecasts against reality until they consistently match. Divergence implies unstable forecasting, inefficient provisioning, and risk of a capacity shortfall.

- Use load testing rather than tradition to establish the resource-to-capacity ratio: a cluster of *X* machines could handle *Y* queries per second three months ago, but can it still do so given changes to the system?

- Don’t mistake day-one load for steady-state load. Launches often attract more traffic, while they’re also the time you especially want to put the product’s best foot forward. See [Reliable Product Launches at Scale](../practices/RELIABLE.PRODUCT.LAUNCHES.SCALE.md) and the [Launch Coordination Checklist](SRE.SUPPLEMENTARY.MATERIAL.md#appendix-e-launch-coordination-checklist).

### Overloads and Failure

Services should produce reasonable but suboptimal results if overloaded. For example, Google Search will search a smaller fraction of the index, and stop serving features like Instant to continue to provide good quality web search results when overloaded. Search SRE tests web search clusters beyond their rated capacity to ensure they perform acceptably when overloaded with traffic.

For times when load is high enough that even degraded responses are too expensive for all queries, practice graceful load shedding, using well-behaved queuing and dynamic timeouts; see [Handling Overload](../practices/HANDLING.OVERLOAD.md). Other techniques include answering requests after a significant delay (“tarpitting”) and choosing a consistent subset of clients to receive errors, preserving a good user experience for the remainder.

Retries can amplify low error rates into higher levels of traffic, leading to cascading failures (see [Addressing Cascading Failures](../practices/ADDRESSING.CASCADING.FAILURES.md)). Respond to cascading failures by dropping a fraction of traffic (including retries!) upstream of the system once total load exceeds total capacity.

Every client that makes an RPC must implement exponential backoff (with jitter) for retries, to dampen error amplification. Mobile clients are especially troublesome because there may be millions of them and updating their code to fix behavior takes a significant amount of time—possibly weeks—and requires that users install updates.

### SRE Teams

SRE teams should spend no more than 50% of their time on operational work (see [Eliminating Toil](../principles/ELIMINATING.TOIL.md)); operational overflow should be directed to the product development team. Many services also include the product developers in the on-call rotation and ticket handling, even if there is currently no overflow. This provides incentives to design systems that minimize or eliminate operational toil, along with ensuring that the product developers are in touch with the operational side of the service. A regular production meeting between SREs and the development team (see [Communication and Collaboration in SRE](COMMUNICATION.COLLABORATION.SRE.md)) is also helpful.

We’ve found that at least eight people need to be part of the on-call team, in order to avoid fatigue and allow sustainable staffing and low turnover. Preferably, those on-call should be in two well-separated geographic locations (e.g., California and Ireland) to provide a better quality of life by avoiding nighttime pages; in this case, six people at each site is the minimum team size.

Expect to handle no more than two events per on-call shift (e.g., per 12 hours): it takes time to respond to and fix outages, start the postmortem, and file the resulting bugs. More frequent events may degrade the quality of response, and suggest that something is wrong with (at least one of) the system’s design, monitoring sensitivity, and response to postmortem bugs.

Ironically, if you implement these best practices, the SRE team may eventually end up out of practice in responding to incidents due to their infrequency, making a long outage out of a short one. Practice handling hypothetical outages (see [Disaster Role Playing](ACCELERATING.SRES.ON.CALL.md#disaster-role-playing)) routinely and improve your incident-handling documentation in the process.

## APPENDIX C Example Incident State Document

**Shakespeare Sonnet++ Overload: 2015-10-21** Incident management info: *http://incident-management-cheat-sheet*

*(Communications lead to keep summary updated.)* **Summary**: Shakespeare search service in cascading failure due to newly discovered sonnet not in search index.

**Status**: active, incident #465

**Command Post(s)**: #shakespeare on IRC

**Command Hierarchy** *(all responders)*

- Current Incident Commander: jennifer

— Operations lead: docbrown

— Planning lead: jennifer

— Communications lead: jennifer

- Next Incident Commander: *to be determined*

*(Update at least every four hours and at handoff of Comms Lead role.)* **Detailed Status** (last updated at 2015-10-21 15:28 UTC by jennifer)

**Exit Criteria:**

- New sonnet added to Shakespeare search corpus **TODO**

- Within availability (99.99%) and latency (99%ile < 100 ms) SLOs for 30+ minutes **TODO**

**TODO list and bugs filed:**

- Run MapReduce job to reindex Shakespeare corpus **DONE**

- Borrow emergency resources to bring up extra capacity **DONE**

- Enable flux capacitor to balance load between clusters (Bug 5554823) **TODO**

**Incident timeline** *(most recent first: times are in UTC)*

- 2015-10-21 15:28 UTC jennifer

— Increasing serving capacity globally by 2x

- 2015-10-21 15:21 UTC jennifer

— Directing all traffic to USA-2 sacrificial cluster and draining traffic from other

clusters so they can recover from cascading failure while spinning up more tasks

— MapReduce index job complete, awaiting Bigtable replication to all clusters

- 2015-10-21 15:10 UTC martym

— Adding new sonnet to Shakespeare corpus and starting index MapReduce

- 2015-10-21 15:04 UTC martym

— Obtains text of newly discovered sonnet from *shakespeare-discuss@* mailing

list

- 2015-10-21 15:01 UTC docbrown

— Incident declared due to cascading failure

- 2015-10-21 14:55 UTC docbrown

— Pager storm, ManyHttp500s in all clusters

## APPENDIX D Example Postmortem

Shakespeare Sonnet++ Postmortem (incident #465)

Date: 2015-10-21

**Authors**: jennifer, martym, agoogler

**Status**: Complete, action items in progress

**Summary**: Shakespeare Search down for 66 minutes during period of very high interest in Shakespeare due to discovery of a new sonnet.

**Impact**:[^p47-1] Estimated 1.21B queries lost, no revenue impact.

**Root Causes**:[^p47-2] Cascading failure due to combination of exceptionally high load and a resource leak when searches failed due to terms not being in the Shakespeare corpus. The newly discovered sonnet used a word that had never before appeared in one of Shakespeare’s works, which happened to be the term users searched for. Under normal circumstances, the rate of task failures due to resource leaks is low enough to be unnoticed.

**Trigger**: Latent bug triggered by sudden increase in traffic.

**Resolution**: Directed traffic to sacrificial cluster and added 10x capacity to mitigate cascading failure. Updated index deployed, resolving interaction with latent bug. Maintaining extra capacity until surge in public interest in new sonnet passes. Resource leak identified and fix deployed.

**Detection**: Borgmon detected high level of HTTP 500s and paged on-call.

**Action Items**:[^p48-3]

*Table D-1. Action items (generated table number)*

| **Action Item** | **Type** | **Owner** | **Bug** |
| --- | --- | --- | --- |
| Update playbook with instructions for responding to cascading failure | mitigate | jennifer | n/a **DONE** |
| Use flux capacitor to balance load between clusters | prevent | martym | Bug 5554823 **TODO** |
| Schedule cascading failure test during next DiRT | process | docbrown | n/a **TODO** |
| Investigate running index MR/fusion continuously | prevent | jennifer | Bug 5554824 **TODO** |
| Plug file descriptor leak in search ranking subsystem | prevent | agoogler | Bug 5554825 **DONE** |
| Add load shedding capabilities to Shakespeare search | prevent | agoogler | Bug 5554826 **TODO** |
| Build regression tests to ensure servers respond sanely to queries of death | prevent | clarac | Bug 5554827 **TODO** |
| Deploy updated search ranking subsystem to prod | prevent | jennifer | n/a **DONE** |
| Freeze production until 2015-11-20 due to error budget exhaustion, or seek exception due to grotesque, unbelievable, bizarre, and unprecedented circumstances | other | docbrown | n/a **TODO** |

#### Lessons Learned

##### What went well

- Monitoring quickly alerted us to high rate (reaching ~100%) of HTTP 500s

- Rapidly distributed updated Shakespeare corpus to all clusters

##### What went wrong

- We’re out of practice in responding to cascading failure

- We exceeded our availability error budget (by several orders of magnitude) due to the exceptional surge of traffic that essentially all resulted in failures

##### Where we got lucky[^p49-4]

- Mailing list of Shakespeare aficionados had a copy of new sonnet available

- Server logs had stack traces pointing to file descriptor exhaustion as cause for crash

- Query-of-death was resolved by pushing new index containing popular search term

#### Timeline

[^p49-5]

2015-10-21 *(all times UTC)*

- 14:51 News reports that a new Shakespearean sonnet has been discovered in a Delorean’s glove compartment

- 14:53 Traffic to Shakespeare search increases by 88x after post to */r/shakespeare* points to Shakespeare search engine as place to find new sonnet (except we don’t have the sonnet yet)

- 14:54 **OUTAGE BEGINS** — Search backends start melting down under load

- 14:55 docbrown receives pager storm, ManyHttp500s from all clusters

- 14:57 All traffic to Shakespeare search is failing: see *http://monitor/shakespeare? end_time=20151021T145700*

- 14:58 docbrown starts investigating, finds backend crash rate very high

- 15:01 **INCIDENT BEGINS** docbrown declares incident #465 due to cascading failure, coordination on #shakespeare, names jennifer incident commander

- 15:02 someone coincidentally sends email to *shakespeare-discuss@* re sonnet discovery, which happens to be at top of martym’s inbox

- 15:03 jennifer notifies *shakespeare-incidents@* list of the incident

- 15:04 martym tracks down text of new sonnet and looks for documentation on corpus update

- 15:06 docbrown finds that crash symptoms identical across all tasks in all clusters, investigating cause based on application logs

- 15:07 martym finds documentation, starts prep work for corpus update

- 15:10 martym adds sonnet to Shakespeare’s known works, starts indexing job

- 15:12 docbrown contacts clarac & agoogler (from Shakespeare dev team) to help with examining codebase for possible causes

- 15:18 clarac finds smoking gun in logs pointing to file descriptor exhaustion, confirms against code that leak exists if term not in corpus is searched for

- 15:20 martym’s index MapReduce job completes

- 15:21 jennifer and docbrown decide to increase instance count enough to drop load on instances that they’re able to do appreciable work before dying and being restarted

- 15:23 docbrown load balances all traffic to USA-2 cluster, permitting instance count increase in other clusters without servers failing immediately

- 15:25 martym starts replicating new index to all clusters

- 15:28 docbrown starts 2x instance count increase

- 15:32 jennifer changes load balancing to increase traffic to nonsacrificial clusters

- 15:33 tasks in nonsacrificial clusters start failing, same symptoms as before

- 15:34 found order-of-magnitude error in whiteboard calculations for instance count increase

- 15:36 jennifer reverts load balancing to resacrifice USA-2 cluster in preparation for additional global 5x instance count increase (to a total of 10x initial capacity)

- 15:36 **OUTAGE MITIGATED**, updated index replicated to all clusters

- 15:39 docbrown starts second wave of instance count increase to 10x initial capacity

- 15:41 jennifer reinstates load balancing across all clusters for 1% of traffic

- 15:43 nonsacrificial clusters’ HTTP 500 rates at nominal rates, task failures intermittent at low levels

- 15:45 jennifer balances 10% of traffic across nonsacrificial clusters

- 15:47 nonsacrificial clusters’ HTTP 500 rates remain within SLO, no task failures observed

- 15:50 30% of traffic balanced across nonsacrificial clusters

- 15:55 50% of traffic balanced across nonsacrificial clusters

- 16:00 **OUTAGE ENDS**, all traffic balanced across all clusters

- 16:30 **INCIDENT ENDS**, reached exit criterion of 30 minutes’ nominal performance

#### Supporting information:

[^p51-6]

- Monitoring dashboard, *http://monitor/shakespeare?end_time=20151021T160000&duration=7200*

## APPENDIX E Launch Coordination Checklist

This is Google’s original Launch Coordination Checklist, circa 2005, slightly abridged for brevity:

**Architecture**

- Architecture sketch, types of servers, types of requests from clients

- Programmatic client requests

**Machines and datacenters**

- Machines and bandwidth, datacenters, N+2 redundancy, network QoS

- New domain names, DNS load balancing

**Volume estimates, capacity, and performance**

- HTTP traffic and bandwidth estimates, launch “spike,” traffic mix, 6 months out

- Load test, end-to-end test, capacity per datacenter at max latency

- Impact on other services we care most about

- Storage capacity

**System reliability and failover**

- What happens when:

— Machine dies, rack fails, or cluster goes offline

— Network fails between two datacenters

- For each type of server that talks to other servers (its backends):

— How to detect when backends die, and what to do when they die

— How to terminate or restart without affecting clients or users

— Load balancing, rate-limiting, timeout, retry and error handling behavior

- Data backup/restore, disaster recovery

**Monitoring and server management**

- Monitoring internal state, monitoring end-to-end behavior, managing alerts

- Monitoring the monitoring

- Financially important alerts and logs

- Tips for running servers within cluster environment

- Don’t crash mail servers by sending yourself email alerts in your own server code

**Security**

- Security design review, security code audit, spam risk, authentication, SSL

- Prelaunch visibility/access control, various types of blacklists

**Automation and manual tasks**

- Methods and change control to update servers, data, and configs

- Release process, repeatable builds, canaries under live traffic, staged rollouts

**Growth issues**

- Spare capacity, 10x growth, growth alerts

- Scalability bottlenecks, linear scaling, scaling with hardware, changes needed

- Caching, data sharding/resharding

**External dependencies**

- Third-party systems, monitoring, networking, traffic volume, launch spikes

- Graceful degradation, how to avoid accidentally overrunning third-party services

- Playing nice with syndicated partners, mail systems, services within Google

**Schedule and rollout planning**

- Hard deadlines, external events, Mondays or Fridays

- Standard operating procedures for this service, for other services

## APPENDIX F Example Production Meeting Minutes

**Date**: 2015-10-23

**Attendees**: agoogler, clarac, docbrown, jennifer, martym

**Announcements**:

- Major outage (#465), blew through error budget

**Previous Action Item Review**

- Certify Goat Teleporter for use with cattle (bug 1011101)

— Nonlinearities in mass acceleration now predictable, should be able to target

accurately in a few days.

**Outage Review**

- New Sonnet (outage 465)

— 1.21B queries lost due to cascading failure after interaction between latent bug

(leaked file descriptor on searches with no results) + not having new sonnet in corpus + unprecedented & unexpected traffic volume

— File descriptor leak bug fixed (bug 5554825) and deployed to prod

— Looking into using flux capacitor for load balancing (bug 5554823) and using

load shedding (bug 5554826) to prevent recurrence

— Annihilated availability error budget; pushes to prod frozen for 1 month

unless docbrown can obtain exception on grounds that event was bizarre & unforeseeable (but consensus is that exception is unlikely) **Paging Events**

- AnnotationConsistencyTooEventual: paged 5 times this week, likely due to cross-regional replication delay between Bigtables.

— Investigation still ongoing, see bug 4821600

— No fix expected soon, will raise acceptable consistency threshold to reduce

unactionable alerts

**Nonpaging Events**

- None

**Monitoring Changes and/or Silences**

- AnnotationConsistencyTooEventual, acceptable delay threshold raised from 60s to 180s, see bug 4821600; TODO(martym).

**Planned Production Changes**

- USA-1 cluster going offline for maintenance between 2015-10-29 and 2015-11-02.

— No response required, traffic will automatically route to other clusters in

region.

**Resources**

- Borrowed resources to respond to sonnet++ incident, will spin down additional server instances and return resources next week

- Utilization at 60% of CPU, 75% RAM, 44% disk (up from 40%, 70%, 40% last week)

**Key Service Metrics**

- **OK** 99ile latency: 88 ms < 100 ms SLO target [trailing 30 days]

- **BAD** availability: 86.95% < 99.99% SLO target [trailing 30 days]

**Discussion / Project Updates**

- Project Molière launching in two weeks.

**New Action Items**

- TODO(martym): Raise AnnotationConsistencyTooEventual threshold.

- TODO(docbrown): Return instance count to normal and return resources.

## Bibliography

[Ada15] Bram Adams, Stephany Bellomo, Christian Bird, Tamara Marshall-Keim, Foutse Khomh, and Kim Moir, “The Practice and Future of Release Engineering: A Roundtable with Three Release Engineers”, *IEEE Software*, vol. 32, no. 2 (March/ April 2015), pp. 42–49.

[Agu10] M. K. Aguilera, “Stumbling over Consensus Research: Misunderstandings and Issues”, in *Replication*, Lecture Notes in Computer Science 5959, 2010.

[All10] J. Allspaw and J. Robbins, *Web Operations: Keeping the Data on Time*: O’Reilly, 2010.

[All12] J. Allspaw, “Blameless PostMortems and a Just Culture”, blog post, 2012.

[All15] J. Allspaw, “Trade-Offs Under Pressure: Heuristics and Observations of Teams Resolving Internet Service Outages”, MSc thesis, Lund University, 2015.

[Ana07] S. Anantharaju, “Automating web application security testing”, blog post, July 2007.

[Ana13] R. Ananatharayan et al., “Photon: Fault-tolerant and Scalable Joining of Continuous Data Streams”, in *SIGMOD ’13*, 2013.

[And05] A. Andrieux, K. Czajkowski, A. Dan, et al., “Web Services Agreement Specification (WS-Agreement)”, September 2005.

[Bai13] P. Bailis and A. Ghodsi, “Eventual Consistency Today: Limitations, Extensions, and Beyond”, in *ACM Queue*, vol. 11, no. 3, 2013.

[Bai83] L. Bainbridge, “Ironies of Automation”, in *Automatica*, vol. 19, no. 6, November 1983.

[Bak11] J. Baker et al., “Megastore: Providing Scalable, Highly Available Storage for Interactive Services”, in *Proceedings of the Conference on Innovative Data System Research*, 2011.

[Bar11] L. A. Barroso, “Warehouse-Scale Computing: Entering the Teenage Decade”, talk at 38th Annual Symposium on Computer Architecture, video available online, 2011.

[Bar13] L. A. Barroso, J. Clidaras, and U. Hölzle, *The Datacenter as a Computer: An Introduction to the Design of Warehouse-Scale Machines, Second Edition*, Morgan & Claypool, 2013.

[Ben12] C. Bennett and A. Tseitlin, “Chaos Monkey Released Into The Wild”, blog post, July 2012.

[Bla14] M. Bland, “Goto Fail, Heartbleed, and Unit Testing Culture”, blog post, June 2014.

[Boc15] L. Bock, *Work Rules!*, Twelve Books, 2015.

[Bol11] W. J. Bolosky, D. Bradshaw, R. B. Haagens, N. P. Kusters, and P. Li, “Paxos Replicated State Machines as the Basis of a High-Performance Data Store”, in *Proc. NSDI 2011*, 2011.

[Boy13] P. G. Boysen, “Just Culture: A Foundation for Balanced Accountability and Patient Safety”, in *The Ochsner Journal*, Fall 2013.

[Bra15] VM Brasseur, “Failure: Why it happens & How to benefit from it”, YAPC 2015.

[Bre01] E. Brewer, “Lessons From Giant-Scale Services”, in *IEEE Internet Computing*, vol. 5, no. 4, July / August 2001.

[Bre12] E. Brewer, “CAP Twelve Years Later: How the “Rules” Have Changed”, in *Computer*, vol. 45, no. 2, February 2012.

[Bro15] M. Brooker, “Exponential Backoff and Jitter”, on *AWS Architecture Blog*, March 2015.

[Bro95] F. P. Brooks Jr., “No Silver Bullet—Essence and Accidents of Software Engineering”, in *The Mythical Man-Month*, Boston: Addison-Wesley, 1995, pp. 180–186.

[Bru09] J. Brutlag, “Speed Matters”, on *Google Research Blog*, June 2009.

[Bul80] G. M. Bull, *The Dartmouth Time-sharing System*: Ellis Horwood, 1980.

[Bur99] M. Burgess, *Principles of Network and System Administration*: Wiley, 1999.

[Bur06] M. Burrows, “The Chubby Lock Service for Loosely-Coupled Distributed Systems”, in *OSDI ’06: Seventh Symposium on Operating System Design and Implementation*, November 2006.

[Bur16] B. Burns, B. Grant, D. Oppenheimer, E. Brewer, and J. Wilkes, “Borg, Omega, and Kubernetes” in *ACM Queue*, vol. 14, no. 1, 2016.

[Cas99] M. Castro and B. Liskov, “Practical Byzantine Fault Tolerance”, in *Proc. OSDI 1999*, 1999.

[Cha10] C. Chambers, A. Raniwala, F. Perry, S. Adams, R. Henry, R. Bradshaw, and N. Weizenbaum, “FlumeJava: Easy, Efficient Data-Parallel Pipelines”, in *ACM SIGPLAN Conference on Programming Language Design and Implementation*, 2010.

[Cha96] T. D. Chandra and S. Toueg, “Unreliable Failure Detectors for Reliable Distributed Systems”, in *J. ACM*, 1996.

[Cha07] T. Chandra, R. Griesemer, and J. Redstone, “Paxos Made Live—An Engineering Perspective”, in *PODC ’07: 26th ACM Symposium on Principles of Distributed Computing*, 2007.

[Cha06] F. Chang et al., “Bigtable: A Distributed Storage System for Structured Data”, in *OSDI ’06: Seventh Symposium on Operating System Design and Implementation*, November 2006.

[Chr09] G. P. Chrousous, “Stress and Disorders of the Stress System”, in *Nature Reviews Endocrinology*, vol 5., no. 7, 2009.

[Clos53] C. Clos, “A Study of Non-Blocking Switching Networks”, in *Bell System Technical Journal*, vol. 32, no. 2, 1953.

[Con15] C. Contavalli, W. van der Gaast, D. Lawrence, and W. Kumari, “Client Subnet in DNS Queries”, *IETF Internet-Draft*, 2015.

[Con63] M. E. Conway, “Design of a Separable Transition-Diagram Compiler”, in *Commun. ACM* 6, 7 (July 1963), 396–408.

[Con96] P. Conway, “Preservation in the Digital World”, report published by the Council on Library and Information Resources, 1996.

[Coo00] R. I. Cook, “How Complex Systems Fail”, in *Web Operations*: O’Reilly, 2010.

[Cor12] J. C. Corbett et al., “Spanner: Google’s Globally-Distributed Database”, in *OSDI ’12: Tenth Symposium on Operating System Design and Implementation*, October 2012.

[Cra10] J. Cranmer, “Visualizing code coverage”, blog post, March 2010.

[Dea13] J. Dean and L. A. Barroso, “The Tail at Scale”, in *Communications of the ACM*, vol. 56, 2013.

[Dea04] J. Dean and S. Ghemawat, “MapReduce: Simplified Data Processing on Large Clusters”, in *OSDI’04: Sixth Symposium on Operating System Design and Implementation*, December 2004.

[Dea07] J. Dean, “Software Engineering Advice from Building Large-Scale Distributed Systems”, Stanford CS297 class lecture, Spring 2007.

[Dek02] S. Dekker, “Reconstructing human contributions to accidents: the new view on error and performance”, in *Journal of Safety Research*, vol. 33, no. 3, 2002.

[Dek14] S. Dekker, *The Field Guide to Understanding “Human Error”*, 3rd edition: Ashgate, 2014.

[Dic14] C. Dickson, “How Embracing Continuous Release Reduced Change Complexity”, presentation at USENIX Release Engineering Summit West 2014, video available online.

[Dur05] J. Durmer and D. Dinges, “Neurocognitive Consequences of Sleep Deprivation”, in *Seminars in Neurology*, vol. 25, no. 1, 2005.

[Eis16] D. E. Eisenbud et al., “Maglev: A Fast and Reliable Software Network Load Balancer”, in NSDI ’16: 13th USENIX Symposium on Networked Systems Design and Implementation, March 2016.

[Ere03] J. R. Erenkrantz, “Release Management Within Open Source Projects”, in *Proceedings of the 3rd Workshop on Open Source Software Engineering*, Portland, Oregon, May 2003.

[Fis85] M. J. Fischer, N. A. Lynch, and M. S. Paterson, “Impossibility of Distributed Consensus with One Faulty Process”, *J. ACM*, 1985.

[Fit12] B. W. Fitzpatrick and B. Collins-Sussman, *Team Geek: A Software Developer’s Guide to Working Well with Others*: O’Reilly, 2012.

[Flo94] S. Floyd and V. Jacobson, “The Synchronization of Periodic Routing Messages”, in IEEE/ACM Transactions on Networking, vol. 2, issue 2, April 1994, pp. 122–136.

[For10] D. Ford et al, “Availability in Globally Distributed Storage Systems”, in *Pro‐ ceedings of the 9th USENIX Symposium on Operating Systems Design and Implementation*, 2010.

[Fox99] A. Fox and E. A. Brewer, “Harvest, Yield, and Scalable Tolerant Systems”, in *Proceedings of the 7th Workshop on Hot Topics in Operating Systems*, Rio Rico, Arizona, March 1999.

[Fow08] M. Fowler, “GUI Architectures”, blog post, 2006.

[Gal78] J. Gall, *SYSTEMANTICS: How Systems Really Work and How They Fail*, 1st ed., Pocket, 1977.

[Gal03] J. Gall, *The Systems Bible: The Beginner’s Guide to Systems Large and Small*, 3rd ed., General Systemantics Press/Liberty, 2003.

[Gaw09] A. Gawande, *The Checklist Manifesto: How to Get Things Right*: Henry Holt and Company, 2009.

[Ghe03] S. Ghemawat, H. Gobioff, and S-T. Leung, “The Google File System”, in *19th ACM Symposium on Operating Systems Principles*, October 2003.

[Gil02] S. Gilbert and N. Lynch, “Brewer’s Conjecture and the Feasibility of Consistent, Available, Partition-Tolerant Web Services”, in *ACM SIGACT News*, vol. 33, no. 2, 2002.

[Gla02] R. Glass, *Facts and Fallacies of Software Engineering*, Addison-Wesley Professional, 2002.

[Gol14] W. Golab et al., “Eventually Consistent: Not What You Were Expecting?”, in *ACM Queue*, vol. 12, no. 1, 2014.

[Gra09] P. Graham, “Maker’s Schedule, Manager’s Schedule”, blog post, July 2009.

[Gup15] A. Gupta and J. Shute, “High-Availability at Massive Scale: Building Google’s Data Infrastructure for Ads”, in *Workshop on Business Intelligence for the Real Time Enterprise*, 2015.

[Ham07] J. Hamilton, “On Designing and Deploying Internet-Scale Services”, in *Pro‐ ceedings of the 21st Large Installation System Administration Conference*, November 2007.

[Han94] S. Hanks, T. Li, D. Farinacci, and P. Traina, “Generic Routing Encapsulation over IPv4 networks”, *IETF Informational RFC*, 1994.

[Hic11] M. Hickins, “Tape Rescues Google in Lost Email Scare”, in *Digits*, *Wall Street Journal*, 1 March 2011.

[Hix15a] D. Hixson, “Capacity Planning”, in *;login:*, vol. 40, no. 1, February 2015.

[Hix15b] D. Hixson, “The Systems Engineering Side of Site Reliability Engineering”, in *;login:* vol. 40, no. 3, June 2015.

[Hod13] J. Hodges, “Notes on Distributed Systems for Young Bloods”, blog post, 14 January 2013.

[Hol14] L. Holmwood, “Applying Cardiac Alarm Management Techniques to Your On-Call”, blog post, 26 August 2014.

[Hum06] J. Humble, C. Read, D. North, “The Deployment Production Line”, in *Pro‐ ceedings of the IEEE Agile Conference*, July 2006.

[Hum10] J. Humble and D. Farley, *Continuous Delivery: Reliable Software Releases through Build, Test, and Deployment Automation*: Addison-Wesley, 2010.

[Hun10] P. Hunt, M. Konar, F. P. Junqueira, and B. Reed, “ZooKeeper: Wait-free coordination for Internet-scale systems”, in *USENIX ATC*, 2010.

[IAEA12] International Atomic Energy Agency, “Safety of Nuclear Power Plants: Design, SSR-2/1”, 2012.

[Jai13] S. Jain et al., “B4: Experience with a Globally-Deployed Software Defined WAN”, in *SIGCOMM ’13*.

[Jon15] C. Jones, T. Underwood, and S. Nukala, “Hiring Site Reliability Engineers”, in *;login:*, vol. 40, no. 3, June 2015.

[Jun07] F. Junqueira, Y. Mao, and K. Marzullo, “Classic Paxos vs. Fast Paxos: Caveat Emptor”, in *Proc. HotDep ’07*, 2007.

[Jun11] F. P. Junqueira, B. C. Reid, and M. Serafini, “Zab: High-performance broadcast for primary-backup systems.”, in *Dependable Systems & Networks (DSN), 2011 IEEE/IFIP 41st International Conference* on 27 Jun 2011: 245–256.

[Kah11] D. Kahneman, *Thinking, Fast and Slow*: Farrar, Straus and Giroux, 2011.

[Kar97] D. Karger et al., “Consistent hashing and random trees: distributed caching protocols for relieving hot spots on the World Wide Web”, in *Proc. STOC ’97*, 29th annual ACM symposium on theory of computing, 1997.

[Kem11] C. Kemper, “Build in the Cloud: How the Build System Works”, *Google Engineering Tools* blog post, August 2011.

[Ken12] S. Kendrick, “What Takes Us Down?”, in *;login:*, vol. 37, no. 5, October 2012.

[Kinc09] Kincaid, Jason. “T-Mobile Sidekick Disaster: Danger’s Servers Crashed, And They Don’t Have A Backup.” *Techcrunch*. n.p., 10 Oct. 2009. Web. 20 Jan. 2015, *http://techcrunch.com/2009/10/10/t-mobile-sidekick-disaster-microsofts-servers-crashed-and-they-dont-have-a-backup*.

[Kin15] K. Kingsbury, “The trouble with timestamps”, blog post, 2013.

[Kir08] J. Kirsch and Y. Amir, “Paxos for System Builders: An Overview”, in *Proc. LADIS ’08*, 2008.

[Kla12] R. Klau, “How Google Sets Goals: OKRs”, blog post, October 2012.

[Kle06] D. V. Klein, “A Forensic Analysis of a Distributed Two-Stage Web-Based Spam Attack”, in *Proceedings of the 20th Large Installation System Administration Conference*, December 2006.

[Kle14] D. V. Klein, D. M. Betser, and M. G. Monroe, “Making *Push On Green* a Reality”, in *;login:*, vol. 39, no. 5, October 2014.

[Kra08] T. Krattenmaker, “Make Every Meeting Matter”, in *Harvard Business Review*, February 27, 2008.

[Kre12] J. Kreps, “Getting Real About Distributed System Reliability”, blog post, 19 March 2012.

[Kri12] K. Krishan, “Weathering The Unexpected”, in *Communications of the ACM*, vol. 55, no. 11, November 2012.

[Kum15] A. Kumar et al., “BwE: Flexible, Hierarchical Bandwidth Allocation for WAN Distributed Computing”, in *SIGCOMM ’15*.

[Lam98] L. Lamport, “The Part-Time Parliament”, in *ACM Transactions on Computer Systems 16, 2*, May 1998.

[Lam01] L. Lamport, “Paxos Made Simple”, in *ACM SIGACT News* 121, December 2001.

[Lam06] L. Lamport, “Fast Paxos”, in *Distributed Computing* 19.2, October 2006.

[Lim14] T. A. Limoncelli, S. R. Chalup, and C. J. Hogan, *The Practice of Cloud System Administration: Designing and Operating Large Distributed Systems, Volume 2*: Addison-Wesley, 2014.

[Loo10] J. Loomis, “How to Make Failure Beautiful: The Art and Science of Postmortems”, in *Web Operations*: O’Reilly, 2010.

[Lu15] H. Lu et al, “Existential Consistency: Measuring and Understanding Consistency at Facebook”, in *SOSP* ’15, 2015.

[Mao08] Y. Mao, F. P. Junqueira, and K. Marzullo, “Mencius: Building Efficient Replicated State Machines for WANs”, in *OSDI ’08*, 2008.

[Mas43] A. H. Maslow, “A Theory of Human Motivation”, in *Psychological Review* 50(4), 1943.

[Mau15] B. Maurer, “Fail at Scale”, in *ACM Queue*, vol. 13, no. 12, 2015.

[May09] M. Mayer, "*This site may harm your computer* on every search result?!?!”, blog post, January 2009.

[McI86] M. D. McIlroy, “A Research Unix Reader: Annotated Excerpts from the Programmer’s Manual, 1971–1986”.

[McN13] D. McNutt, “Maintaining Consistency in a Massively Parallel Environment”, presentation at USENIX Configuration Management Summit 2013, video available online.

[McN14a] D. McNutt, “Accelerating the Path from Dev to DevOps”, in *;login:*, vol. 39, no. 2, April 2014.

[McN14b] D. McNutt, “The 10 Commandments of Release Engineering”, presentation at 2nd International Workshop on Release Engineering 2014, April 2014.

[McN14c] D. McNutt, “Distributing Software in a Massively Parallel Environment”, presentation at USENIX LISA 2014, video available online.

[Mic03] Microsoft TechNet, “What is SNMP?”, last modified March 28, 2003, *https:// technet.microsoft.com/en-us/library/cc776379%28v=ws.10%29.aspx*.

[Mea08] D. Meadows, *Thinking in Systems*: Chelsea Green, 2008.

[Men07] P. Menage, “Adding Generic Process Containers to the Linux Kernel”, in *Proc. of Ottawa Linux Symposium*, 2007.

[Mer11] N. Merchant, “Culture Trumps Strategy, Every Time”, in *Harvard Business Review*, March 22, 2011.

[Moc87] P. Mockapetris, “Domain Names - Implementation and Specification”, *IETF Internet Standard*, 1987.

[Mol86] C. Moler, “Matrix Computation on Distributed Memory Multiprocessors”, in *Hypercube Multiprocessors 1986*, 1987.

[Mor12a] I. Moraru, D. G. Andersen, and M. Kaminsky, “Egalitarian Paxos”, *Carnegie Mellon University Parallel Data Lab Technical Report CMU-PDL-12-108*, 2012.

[Mor14] I. Moraru, D. G. Andersen, and M. Kaminsky, “Paxos Quorum Leases: Fast Reads Without Sacrificing Writes”, in *Proc. SOCC ’14*, 2014.

[Mor12b] J. D. Morgenthaler, M. Gridnev, R. Sauciuc, and S. Bhansali, “Searching for Build Debt: Experiences Managing Technical Debt at Google”, in *Proceedings of the 3rd Int’l Workshop on Managing Technical Debt*, 2012.

[Nar12] C. Narla and D. Salas, “Hermetic Servers”, blog post, 2012.

[Nel14] B. Nelson, “The Data on Diversity”, in *Communications of the ACM*, vol. 57, 2014.

[Nic12] K. Nichols and V. Jacobson, “Controlling Queue Delay”, in *ACM Queue*, vol. 10, no. 5, 2012.

[Oco12] P. O’Connor and A. Kleyner, *Practical Reliability Engineering*, 5th edition: Wiley, 2012.

[Ohn88] T. Ohno, *Toyota Production System: Beyond Large-Scale Production*: Productivity Press, 1988.

[Ong14] D. Ongaro and J. Ousterhout, “In Search of an Understandable Consensus Algorithm (Extended Version)”.

[Pen10] D. Peng and F. Dabek, “Large-scale Incremental Processing Using Distributed Transactions and Notifications”, in *Proc. of the 9th USENIX Symposium on Operating System Design and Implementation*, November 2010.

[Per99] C. Perrow, *Normal Accidents: Living with High-Risk Technologies*, Princeton University Press, 1999.

[Per07] A. R. Perry, “Engineering Reliability into Web Sites: Google SRE”, in *Proc. of LinuxWorld 2007*, 2007.

[Pik05] R. Pike, S. Dorward, R. Griesemer, S. Quinlan, “Interpreting the Data: Parallel Analysis with Sawzall”, in *Scientific Programming Journal* vol. 13, no. 4, 2005.

[Pot16] R. Potvin and J. Levenberg, “The Motivation for a Monolithic Codebase: Why Google stores billions of lines of code in a single repository”, in *Communications of the ACM*, forthcoming July 2016. Video available on YouTube.

[Roo04] J. J. Rooney and L. N. Vanden Heuvel, “Root Cause Analysis for Beginners”, in *Quality Progress*, July 2004.

[Sai39] A. de Saint Exupéry, *Terre des Hommes* (Paris: Le Livre de Poche, 1939, in translation by Lewis Galantière as *Wind, Sand and Stars*.

[Sam14] R. R. Sambasivan, R. Fonseca, I. Shafer, and G. R. Ganger, “So, You Want To Trace Your Distributed System? Key Design Insights from Years of Practical Experience”, Carnegie Mellon University Parallel Data Lab Technical Report CMU-PDL-14-102, 2014.

[San11] N. Santos and A. Schiper, “Tuning Paxos for High-Throughput with Batching and Pipelining”, in *13th Int’l Conf. on Distributed Computing and Networking*, 2012.

[Sar97] N. B. Sarter, D. D. Woods, and C. E. Billings, “Automation Surprises”, in *Handbook of Human Factors & Ergonomics*, 2nd edition, G. Salvendy (ed.), Wiley, 1997.

[Sch14] E. Schmidt, J. Rosenberg, and A. Eagle, *How Google Works*: Grand Central Publishing, 2014.

[Sch15] B. Schwartz, “The Factors That Impact Availability, Visualized”, blog post, 21 December 2015.

[Sch90] F. B. Schneider, “Implementing Fault-Tolerant Services Using the State Machine Approach: A Tutorial”, in *ACM Computing Surveys*, vol. 22, no. 4, 1990.

[Sec13] Securities and Exchange Commission, “Order In the Matter of Knight Capital Americas LLC”, file 3-15570, 2013.

[Sha00] G. Shao, F. Berman, and R. Wolski, “Master/Slave Computing on the Grid”, in *Heterogeneous Computing Workshop*, 2000.

[Shu13] J. Shute et al., “F1: A Distributed SQL Database That Scales”, in *Proc. VLDB 2013*, 2013.

[Sig10] B. H. Sigelman et al., “Dapper, a Large-Scale Distributed Systems Tracing Infrastructure”, Google Technical Report, 2010.

[Sin15] A. Singh et al., “Jupiter Rising: A Decade of Clos Topologies and Centralized Control in Google’s Datacenter Network”, in *SIGCOMM ’15*.

[Skel13] M. Skelton, “Operability can Improve if Developers Write a Draft Run Book”, blog post, 16 October 2013.

[Slo11] B. Treynor Sloss, “Gmail back soon for everyone”, blog post, 28 February 2011.

[Tat99] S. Tatham, “How to Report Bugs Effectively”, 1999.

[Ver15] A. Verma, L. Pedrosa, M. R. Korupolu, D. Oppenheimer, E. Tune, and J. Wilkes, “Large-scale cluster management at Google with Borg”, in *Proceedings of the European Conference on Computer Systems*, 2015.

[Wal89] D. R. Wallace and R. U. Fujii, “Software Verification and Validation: An Overview”, *IEEE Software*, vol. 6, no. 3 (May 1989), pp. 10, 17.

[War14] R. Ward and B. Beyer, “BeyondCorp: A New Approach to Enterprise Security”, in *;login:*, vol. 39, no. 6, December 2014.

[Whi12] J. A. Whittaker, J. Arbon, and J. Carollo, *How Google Tests Software*: Addison-Wesley, 2012.

[Woo96] A. Wood, “Predicting Software Reliability”, in *Computer*, vol. 29, no. 11, 1996.

[Wri12a] H. K. Wright, “Release Engineering Processes, Their Faults and Failures”, (section 7.2.2.2) PhD Thesis, University of Texas at Austin, 2012.

[Wri12b] H. K. Wright and D. E. Perry, “Release Engineering Practices and Pitfalls”, in *Proceedings of the 34th International Conference on Software Engineering (ICSE ’12)*. (IEEE, 2012), pp. 1281–1284.

[Wri13] H. K. Wright, D. Jasper, M. Klimek, C. Carruth, Z. Wan, “Large-Scale Automated Refactoring Using ClangMR”, in *Proceedings of the 29th International Conference on Software Maintenance (ICSM ’13)*, (IEEE, 2013), pp. 548–551.

[Zoo14] ZooKeeper Project (Apache Foundation), “ZooKeeper Recipes and Solutions”, in ZooKeeper 3.4 documentation, 2014.

## Index

```text
Symbols
/varz HTTP handler, 109
A
abusive client behavior, 382
access control, 89
ACID datastore semantics, 287, 341
acknowledgments, xxi-xxiv
adaptive throttling, 250
Ads Database, 73-75
AdSense, 31
aggregate availability equation, 27, 477
aggregation, 114, 180
agility vs. stability, 97
  (see also software simplicity)
Alertmanager service, 119
alerts
  defined, 56
  false-positive, 180
  software for, 18
     (see also Borgmon; time-series monitor‐
       ing)
anacron, 316
Apache Mesos, 15
App Engine, 146
archives vs. backups, 343
asynchronous distributed consensus, 289
atomic broadcast systems, 295
attribution policy, xx
automation
  applying to cluster turnups, 75-81
  vs. autonomous systems, 67
  benefits of, 67-70
  best practices for change management, 10
  Borg example, 81
  cross-industry lessons, 467
  database example, 73-75
  Diskerase example, 85
  focus on reliability, 83
  Google's approach to, 70
  hierarchy of automation classes, 72
  recommendations for enacting, 84
  specialized application of, 79
  use cases for, 70-73
automation tools, 194
autonomous systems, 67
Auxon case study, 207-209, 211-213
availability, 38, 341
  (see also service availability)
availability table, 477
B
B4 network, 15
backend servers, 19, 231
backends, fake, 204
backups (see data integrity)
Bandwidth Enforcer (BwE), 17
barrier tools, 193, 195, 293
batch processing pipelines, 350
batching, 282, 302, 329
Bazel, 90
best practices
  capacity planning, 482
  for change management, 10
  error budgets, 481
  failures, 479
  feedback, 174
  for incident management, 166
```

```text
  monitoring, 481
  overloads and failure, 483
  postmortems, 171-172, 482
  reward systems, 174
  role of release engineers in, 88
  rollouts, 480
  service level objectives, 480
  team building, 483
bibliography, 501
Big Data, 327
Bigtable, 17, 31, 65
bimodal latency, 273
black-box monitoring, 55, 59, 120
blameless cultures, 170
Blaze build tool, 90
Blobstore, 17, 342
Borg, 14-16, 81-83, 329
Borg Naming Service (BNS), 16
Borgmon, 108-123
  (see also time-series monitoring)
  alerting, 18, 118
  configuration, 121
  rate() function, 116
  rules, 114-118
  sharding, 119
  timeseries arena, 111
  vectors, 112-113
break-glass mechanisms, 201
build environments, 190
business continuity, 337
Byzantine failures, 290, 304
C
campuses, 14
canarying, 34, 155, 189, 380
CAP theorem, 286
CAPA (corrective and preventative action), 465
capacity planning
  approaches to, 105
  best practices for, 482
  Diskerase example, 85
  distributed consensus systems and, 307
  drawbacks of "queries per second", 248
  drawbacks of traditional plans, 208
  further reading on, 106
  intent-based (see intent-based capacity
     planning)
  mandatory steps for, 11
  preventing server overload with, 266
  product launches and, 376
  traditional approach to, 207
cascading failures
  addressing, 280-283
  causes of, 260-265
  defined, 259, 308
  factors triggering, 276
  overview of, 283
  preventing server overload, 265-276
  testing for, 278-280
     (see also overload handling)
change management, 10
  (see also automation)
change-induced emergencies, 153-155
changelists (CLs), 20
Chaos Monkey, 196
checkpoint state, 195
cherry picking tactic, 89
Chubby lock service, 18, 291
  planned outage, 39, 47
client tasks, 231
client-side throttling, 249
clients, 19
clock drift, 287
Clos network fabric, 14
cloud environment
  data integrity strategies, 341, 356
  definition of data integrity in, 340
  evolution of applications in, 341
  technical challenges of, 344
clusters
  applying automation to turnups, 75-81
  cluster management solution, 329
  defined, 14
code samples, xx
cognitive flow state, 409
cold caching, 274
colocation facilities (colos), 85
Colossus, 16
command posts, 164
communication and collaboration
  blameless postmortems, 171
  case studies, 432-439
  importance of, 440
  with Outalator, 181
  outside SRE team, 437
  position of SRE in Google, 425
  production meetings (see production meet‐
     ings)
```

```text
  within SRE team, 430
company-wide resilience testing, 106
compensation structure, 128
computational optimization, 209
configuration management, 93, 153, 201, 277
configuration tests, 188
consensus algorithms
  Egalitarian Paxos, 302
  Fast Paxos, 301, 320
  improving performance of, 296
  Multi-Paxos, 303
  Paxos, 290, 303
  Raft, 298, 302
  Zab, 302
     (see also distributed consensus systems)
consistency
  eventual, 287
  through automation, 67
consistent hashing, 227
constraints, 208
Consul, 291
consumer services, identifying risk tolerance of,
  28-31
continuous build and deployment
  Blaze build tool, 90
  branching, 90
  build targets, 90
  configuration management, 93
  deployment, 93
  packaging, 91
  Rapid release system, 90, 91
  testing, 90
  typical release process, 92
contributors, xxii-xxiv
coroutines, 327
corporate network security, 106
correctness guarantees, 335
correlation vs. causation, 136
costs
  availability targets and, 30, 32
  direct, 4
  of failing to embrace risk, 25
  indirect, 4
  of sysadmin management approach, 4
CPU consumption, 248, 262, 383
crash-fail vs. crash-recover algorithms, 290
cron
  at large scale, 325
  building at Google, 319-325
  idempotency, 316
  large-scale deployment of, 317
  leader and followers, 321
  overview of, 326
  Paxos algorithm and, 320-325
  purpose of, 315
  reliability applications of, 316
  resolving partial failures, 322
  storing state, 324
  tracking cron job state, 319
  uses for, 315
cross-industry lessons
  Apollo 8, xvii
  comparative questions presented, 459
  decision-making skills, 469-470
  Google's application of, 470
  industry leaders contributing, 460
  key themes addressed, 459
  postmortem culture, 465-467
  preparedness and disaster testing, 462-465
  repetitive work/operational overhead, 467
current state, exposing, 138
D
D storage layer, 16
dashboards
  benefits of, 57
  defined, 55
data analysis, with Outalator, 181
data integrity
  backups vs. archives, 343
  case studies in, 360-366
  conditions leading to failure, 346
  defined, 339
  expanded definition of, 340
  failure modes, 349
  from users’ perspective, 345
  overview of, 368
  selecting strategy for, 341-343, 356
  SRE approach to, 349-360
  SRE objectives for, 344-349
  SRE principles applied to, 367-368
  strict requirements, 340
  technical challenges of, 344
data processing pipelines
  business continuity and, 337
  challenges of uneven work distribution, 328
  challenges to periodic pattern, 328
  drawbacks of periodic, 329-332
```

```text
  effect of big data on, 328
  monitoring problems, 331-332
  origin of, 327
  overview of, 338
  pipeline depth, 328
  simple vs. multiphase pipelines, 328
  Workflow system, 333, 335
data recovery, 359
datacenters
  backbone network for, 15
  data validation, 357
  load balancing, 231-246
  topology of, 14
datastores
  ACID and BASE, 287, 341, 347
  reliable replicated, 292
Decider, 74
decision-making skills, 469
defense in depth, for data integrity, 349, 361,
  367
demand forecasting, 11
dependency hierarchies, 58, 263
deployment, 93
  (see also continuous build and deployment)
development environment, 19
development/ops split, 4
DevOps, 7
Direct Server Response (DSR), 228
disaster recovery tools, 195
disaster role playing, 401
disaster testing, 462-465
  Disaster and Recovery Testing (DiRT), 462
disk access, 303
Diskerase process, 85
distractibility, 411
distributed consensus systems
  benefits of, 285
  coordination, use in, 293
  deploying, 304-312
  locking, use in, 286
  monitoring, 312
  need for, 285
  overview of, 313
  patterns for, 291-295
  performance of, 296-304
  principles, 289
  quorum composition, 310
  quorum leasing technique, 299
     (see also consensus algorithms)
distributed periodic scheduling (see cron)
DNS (Domain Name System)
  EDNS0 extension, 225
  load balancing using, 224-227
DoubleClick for Publishers (DFP), 437-439
drains, 277
DTSS communication files, 327
dueling proposers situation, 298
durability, 38
E
early detection for data integrity, 356
  (see also data integrity)
Early Engagement Model, 448-451
“embarrassingly parallel” algorithms, 328
embedded engineers, 417-423
emergency preparedness, 361
  cross-industry lessons, 462
emergency response
  change-induced emergencies, 153-155
  essential elements of, 151
  Five Whys, 140, 487
  guidelines for, 10
  initial response, 151
  lessons learned, 158
  overview of, 159
  process-induced emergencies, 155
  solution availability, 158
  test-induced emergencies, 152
encapsulation, 228
endpoints, in debugging, 138
engagements (see SRE engagement model)
error budgets
  benefits of, 35
  best practices for, 481
  forming, 34
  guidelines for, 8
  motivation for, 33
error rates, 38, 60
Escalator, 178
ETL pipelines, 327
eventual consistency, 287
executor load average, 253
F
failures, best practices for, 479
  (see also cascading failures)
fake backends, 204
false-positive alerts, 180
```

```text
feature flag frameworks, 381
file descriptors, 263
Five Whys, 140, 487
flow control, 233
FLP impossibility result, 290
Flume, 328
fragmentation, 229
G
gated operations, 89
Generic Routing Encapsulation (GRE), 228
GFE (Google Frontend), 21, 232
GFS (Google File System), 76, 293, 318-319,
  354
global overload, 248
Global Software Load Balancer (GSLB), 18
Gmail, 65, 360
Google Apps for Work, 29
Google Compute Engine, 38
Google production environment
  best practices for, 479-484
  complexity of, 205
  datacenter topology, 14
  development environment, 19
  hardware, 13
  Shakespeare search service, 20-22
  software infrastructure, 19
  system software, 15-19
Google Workflow system
  as model-view-controller pattern, 334
  business continuity and, 337
  correctness guarantees, 335
  development of, 333
  stages of execution in, 335
graceful degradation, 267
GTape, 360
H
Hadoop Distributed File System (HDFS), 16
handoffs, 164
“hanging chunk” problem, 329
hardware
  managing failures, 15
  software that “organizes”, 15-19
  terminology used for, 13
health checks, 281
healthcare.gov, 103
hermetic builds, 89
hierarchical quorums, 311
high-velocity approach, 24, 88
hotspotting, 236
I
idempotent operations, 78, 316
incident management
  best practices for, 166
  effective, 161
  formal protocols for, 130
  incident management process, 153, 163
  incident response, 104
  managed incident example, 165
  roles, 163
  template for, 485
  unmanaged incident example, 161
  when to declare an incident, 166
infrastructure services
  identifying risk tolerance of, 31
  improved SRE through automation, 69
integration proposals, 89
integration tests, 186, 201
intent-based capacity planning
  Auxon implementation, 211-213
  basic premise of, 209
  benefits of, 209
  defined, 209
  deploying approximation, 214
  driving adoption of, 215-217
  precursors to intent, 210
  requirements and implementation, 213
  selecting intent level, 210
  team dynamics, 218
interrupts
  cognitive flow state and, 409
  dealing with, 407
  dealing with high volumes, 412
  determining approach to handling, 408
  distractibility and, 411
  managing operational load, 408
  on-call engineers and, 412
  ongoing responsibilities, 413
  polarizing time, 411
  reducing, 413
  ticket assignments, 413
IRC (Internet Relay Chat), 164
J
jobs, 16
Jupiter network fabric, 14
```

```text
L
labelsets, 112
lame duck state, 234
latency
  defined, 341
  measuring, 38
  monitoring for, 60
launch coordination
  checklist, 373-380, 493
  engineering (LCE), 370, 384-387
     (see also product launches)
lazy deletion, 349
leader election, 286, 292
lease systems, 294
Least-Loaded Round Robin policy, 243
level of service, 37
  (see also service level objectives (SLOs))
living incident documents, 164
load balancing
  datacenter
     datacenter services and tasks, 231
     flow control, 233
     Google's application of, 231
     handling overload, 247
     ideal CPU usage, 232, 248
     lame duck state, 234
     limiting connections pools, 235-240
     packet encapsulation, 228
     policies for, 240-246
     SRE software engineering dynamics, 218
  distributed consensus systems and, 307
  frontend
     optimal solutions for, 223
     using DNS, 224-227
     virtual IP addresses (VIPs), 227
  policy
     Least-Loaded Round Robin, 243
     Round Robin, 241
     Weighted Round Robin, 245
load shedding, 267
load tests, 383
lock services, 18, 293
logging, 138
Lustre, 16
M
machines
  defined, 13, 56
  managing with software, 15
majority quorums, 304
MapReduce, 328
mean time
  between failures (MTBF), 184, 199
  to failure (MTTF), 10
  to repair (MTTR), 10, 68, 184
memory exhaustion, 263
Mencius algorithm, 302
meta-software, 70
Midas Package Manager (MPM), 91
model-view-controller pattern, 334
modularity, 100
Moiré load pattern in pipelines, 331
monitoring distributed systems
  avoiding complexity in, 62
  benefits of monitoring, 56, 107
  best practices for, 481
  blackbox vs. whitebox, 59, 120
  case studies, 65-66
  challenges of, 64, 107
  change-induced emergencies, 154
  creating rules for, 63
  four golden signals of, 60
  guidelines for, 9
  instrumentation and performance, 61
  monitoring philosophy, 64
  resolution, 62
  setting expectations for, 57
  short- vs. long-term availability, 66
  software for, 18
  symptoms vs. causes, 58
  terminology, 55
  valid monitoring outputs, 10
     (see also Borgmon; time-series monitor‐
       ing)
Multi-Paxos protocol, 297, 303
  (see also consensus algorithms)
multi-site teams, 127
multidimensional matrices, 112
multiphase pipelines, 328
MySQL
  migrating, 73-75, 437
  test-induced emergencies and, 152
N
N + 2 configuration, 22, 210-212, 266, 482
negative results, 144
Network Address Translation, 228
network latency, 300
```

```text
network load balancer, 227
network partitions, 287
Network Quality of Service (QoS), 157, 252
network security, 106
networking, 17
NORAD Tracks Santa website, 369
number of “nines”, 38, 477
O
older releases, rebuilding, 89
on-call
  balanced on-call, 127
  benefits of, 132
  best practices for, 393, 400-405
  compensation structure, 128
  continuing education, 406
  education practices, 392, 395
  formal incident-management protocols, 130
  inappropriate operational loads, 130
  initial learning experiences, 394
  learning checklists, 403
  overview of, 125, 406
  resources for, 129
  rotation schedules, 126
  shadow on-call, 405
  stress-reduction techniques, 128
  target event volume, 8
  targeted project work, 397
  team building, 391
  time requirements, 128
  training for, 395-401
  training materials, 397
  typical activities, 126
one-phase pipelines, 328
open commenting/annotation system, 171
operational load
  cross-industry lessons, 467
  managing, 408
  ongoing responsibilities, 408
  types of, 407
operational overload, 130
operational underload, 132
operational work (see toil)
out-of-band checks and balances, 342, 357
out-of-band communications systems, 154
outage tracking
  baselines and progress tracking, 177
  benefits of, 182
  Escalator, 178
  Outalator, 178-182
Outalator
  aggregation in, 180
  benefits of, 178
  building your own, 179
  incident analysis, 181
  notification process, 178
  reporting and communication, 181
  tagging in, 180
overhead, 49
overload handling
  approaches to, 247
  best practices for, 483
  client-side throttling, 249
  load from connections, 257
  overload errors, 253
  overview of, 258
  per-client retry budget, 254
  per-customer limits, 248
  per-request retry budget, 254
  product launches and, 383
  request criticality, 251
  retrying requests, 254
     (see also retries, RPC)
  utilization signals, 253
     (see also cascading failures)
P
package managers, 91
packet encapsulation, 228
Paxos consensus algorithm
  Classic Paxos algorithm, 301
  disk access and, 303
  Egalitarian Paxos consensus algorithm, 302
  Fast Paxos consensus algorithm, 301, 320
  Lamport’s Paxos protocol, 290
     (see also consensus algorithms)
performance
  efficiency and, 12
  monitoring, 61
performance tests, 186
periodic pipelines, 328
periodic scheduling (see cron)
persistent storage, 303
Photon, 305
pipelining, 302
planned changes, 277
policies and procedures, enforcing, 89
post hoc analysis, 58
```

```text
postmortems
  benefits of, 169
  best practices for, 171-174, 482
  collaboration and sharing in, 171
  concept of, 169
  cross-industry lessons, 465-467
  example postmortem, 487-490
  formal review and publication of, 171
  Google's philosophy for, 169
  guidelines for, 8
  introducing postmortem cultures, 172
  on-call engineering and, 400
  ongoing improvements to, 175
  rewarding participation in, 174
  triggers for, 170
privacy, 341
proactive testing, 159
problem reports, 136
process death, 276
process health checks, 281
process updates, 277
process-induced emergencies, 155
Prodtest (Production Test), 76
product launches
  best practices for, 480
  defined, 370
  development of Launch Coordination Engi‐
     neering (LCE), 384-387
  driving convergence and simplification, 374
  launch coordination checklists, 373-380,
     493
  launch coordination engineering, 370
  NORAD Tracks Santa example, 369
  overview of, 387
  processes for, 372
  rate of, 370
  techniques for reliable, 380-384
production environment (see Google produc‐
  tion environment)
production inconsistencies
  detecting with Prodtest, 76
  resolving idempotently, 78
production meetings, 426-430
  agenda example, 497
production probes, 202
Production Readiness Review process (see SRE
  engagement model)
production tests, 187
protocol buffers (protobufs), 19, 202
Protocol Data Units, 229
provisioning, guidelines for, 11
PRR (Production Readiness Review) model,
  442, 444-448
push frequency, 34
push managers, 413
Python’s safe_load, 201
Q
“queries per second” model, 248
Query of Death, 276
queuing
  controlled delay, 267
  first-in, first-out, 267
  last-in, first-out, 267
  management of, 266, 294
queuing-as-work-distribution pattern, 295
quorum (see distributed consensus systems)
R
Raft consensus protocol, 298, 302
  (see also consensus algorithms)
RAID, 354
Rapid automated release system, 90, 91
read workload, scaling, 298
real backups, 343
real-time collaboration, 171
recoverability, 347
recovery, 359
recovery systems, 345
recursion (see recursion)
recursive DNS servers, 225
recursive separation of responsibilities, 163
redundancy, 347, 354
Reed-Solomon erasure codes, 354
regression tests, 186
release engineering
  challenges of, 87
  continuous build and deployment, 90-94
  defined, 87
  instituting, 95
  philosophy of, 88-90
  the role of release engineers, 87
  wider application of, 95
reliability testing
  amount required, 184
  benefits of, 204
  break-glass mechanisms, 201
  canary tests, 189
```

```text
  configuration tests, 188
  coordination of, 197
  creating test and build environments, 190
  error budgets, 8, 33-35, 481
  expecting test failure, 199-200
  fake backend versions, 204
  goals of, 183
  importance of, xvi
  integration tests, 186, 201
  MTTR and, 184
  performance tests, 186
  proactive, 159
  production probes, 202
  production tests, 187
  regression tests, 186
  reliability goals, 25
  sanity testing, 186
  segregated environments and, 198
  smoke tests, 186
  speed of, 196
  statistical tests, 196
  stress tests, 188
  system tests, 186
  testing at scale, 192-204
  timing of, 187
  unit tests, 185
reliable replicated datastores, 292
Remote Procedure Call (RPC), 19, 138, 252
  bimodal, 273
  deadlines
     missing, 271
     propagating, 267, 272
     queue management, 266, 294
     selecting, 271
  retries, 268-271
  RPC criticality, 251
     (see also overload handling)
replicas
  adding, 307
  drawbacks of leader replicas, 308
  location of, 306, 310
  number deployed, 304
replicated logs, 305
replicated state machine (RSM), 291
replication, 347, 354
request latency, 38, 60
request profile changes, 277
request success rate, 27
resilience testing, 106
resources
  allocation of, 14, 16
  exhaustion, 261
  limits, 278
     (see also capacity planning)
restores, 355
retention, 348
retries, RPC
  avoiding, 254
  cascading failures due to, 268
  considerations for automatic, 270
  diagnosing outages due to, 271
  handling overload errors and, 254
  per-client retry budgets, 254
  per-request retry budgets, 254
reverse engineering, 398
reverse proxies, 157
revision history, 351
risk management
  balancing risk and innovation, 25
  costs of, 25
  error budgets, 33-36, 481
  key insights, 36
  measuring service risk, 26
  risk tolerance of services, 28-33
rollback procedures, 153
rollouts, 277, 379, 480
root cause
  analysis of, 105, 169
     (see also postmortems)
  defined, 56
Round Robin policy, 241
round-trip-time (RTT), 300
rows, 14
rule evaluation, in monitoring systems, 114-118
S
Safari® Books Online, xxi
sanity testing, 186
saturation, 60
scale
  defined, 341
  issues in, 347
security
  in release engineering, 89
  new approach to, 106
self-service model, 88
separation of responsibilities, 163
servers
```

```text
  vs. clients, 19
  defined, 13
  overload scenario, 260
  preventing overload, 265-276
service availability
  availability table, 477
  cost factors, 30, 32
  defined, 38
  target for consumer services, 29
  target for infrastructure service, 31
  time-based equation, 26
  types of consumer service failures, 29
  types of infrastructure services failures, 32
service health checks, 281
service latency
  looser approach to, 31
  monitoring for, 60
service level agreements (SLAs), 39
service level indicators (SLIs)
  aggregating raw measurements, 41
  collecting indicators, 41
  defined, 38
  standardizing indicators, 43
service level objectives (SLOs)
  agreements in practice, 47
  best practices for, 480
  choosing, 37-39
  control measures, 46
  defined, 38
  defining objectives, 43
  selecting relevant indicators, 40
  statistical fallacies and, 43
  target selection, 45
  user expectations and, 39, 46
service management
  comprehensive approach to, xvi
  Google’s approach to, 5-7
  sysadmin approach to, 3, 67
service reliability hierarchy
  additional resources, 106
  capacity planning, 105
  development, 106
  diagram of, 103
  incident response, 104
  monitoring, 104
  product launch, 106
  root cause analysis, 105
  testing, 105
service unavailability, 264
Service-Oriented Architecture (SOA), 81
Shakespeare search service, example
  alert, 137
  applying SRE to, 20-22
  cascading failure example, 259-283
  debugging, 139
  engagement, 283, 445
  incident management, 485
  postmortem, 487-490
  production meeting, 497-499
sharded deployments, 307
SHEDDABLE_PLUS criticality value, 251
simplicity, 97-101
Sisyphus automation framework, 93
Site Reliability Engineering (SRE)
  activities included in, 103
  approach to learning, xix
  basic components of, xv
  benefits of, 6
  challenges of, 6
  defined, xiii-xiv, 5
  early engineers, xvii
  Google’s approach to management, 5-7, 425
  growth of at Google, 473, 474
  hiring, 5, 391
  origins of, xvi
  sysadmin approach to management, 3, 67
  team composition and skills, 5, 126, 473
  tenets of, 7-12
  typical activities of, 52
  widespread applications of, xvi
slow startup, 274
smoke tests, 186
SNMP (Simple Networking Monitoring Proto‐
  col), 111
soft deletion, 350
software bloat, 99
software engineering in SRE
  activities included in, 106
  Auxon case study, 207-209
  benefits of, 222
  encouraging, 215
  fostering, 218
  Google's focus on, 205
  importance of, 205
  intent-based capacity planning, 209-218
  staffing and development time, 219
  team dynamics, 218
software fault tolerance, 34
```

```text
software simplicity
  avoiding bloat, 99
  modularity, 100
  predictability and, 98
  release simplicity, 100
  reliability and, 101
  source code purges, 98
  system stability versus agility, 97
  writing minimal APIs, 99
Spanner, 17, 32, 337
SRE engagement model
  aspects addressed by, 443
  Early Engagement Model, 448-451
  frameworks and platforms in, 451-455
  importance of, 441
  Production Readiness Review, 442, 444-448
SRE tools
  automation tools, 194
  barrier tools, 193, 195
  disaster recovery tools, 195
  testing, 193
  writing, 202
SRE Way, 12
stability vs. agility, 97
  (see also software simplicity)
stable leaders, 302
statistical tests, 196
storage stack, 16
stress tests, 188
strong leader process, 297
Stubby, 19
subsetting
  defined, 235
  deterministic, 238
  process of, 235
  random, 237
  selecting subsets, 236
synchronous consensus, 289
sysadmins (systems administrators), 3, 67
system software
  managing failures with, 15
  managing machines, 15
  storage, 16
system tests, 186
system throughput, 38
systems administrators (sysadmins), 3, 67
systems engineering, 390
T
tagging, 180
“task overloaded” errors, 253
tasks
  backend, 231
  client, 231
  defined, 16
TCP/IP communication protocol, 300
team building
  benefits of Google's approach to, 6, 473
  best practices for, 483
  development focus, 6
  dynamics of SRE software engineering, 218
  eliminating complexity, 98
  engineering focus, 6, 7, 52, 126-127, 474
  multi-site teams, 127
  self-sufficiency, 88
  skills needed, 5
  staffing and development time, 219
  team composition, 5
terminology (Google-specific)
  campuses, 14
  clients, 19
  clusters, 14
  datacenters, 14
  frontend/backend, 19
  jobs, 16
  machines, 13
  protocol buffers (protobufs), 19
  racks, 14
  rows, 14
  servers, 13, 19
  tasks, 16
test environments, 190
  (see also reliability testing)
test-induced emergencies, 152
testing (see reliability testing)
text logs, 138
thread starvation, 263
throttling
  adaptive, 250
  client-side, 249
“thundering herd” problems, 331, 383
time-based availability equation, 26, 477
Time-Series Database (TSDB), 112
time-series monitoring
  alerting, 118
  black-box monitoring, 120
  Borgmon monitoring system, 108
```

```text
  collection of exported data, 110
  instrumentation of applications, 109
  maintaining Borgmon configuration, 121
  monitoring topology, 119
  practical approach to, 108
  rule evaluation, 114-118
  scaling, 122
  time-series data storage, 111-113
  tools for, 108
time-to-live (TTL), 225
timestamps, 292
toil
  benefits of limiting, 24, 51
  calculating, 51
  characteristics of, 49
  cross-industry lessons, 467
  defined, 49
  drawbacks of, 52
  vs. engineering work, 52
traffic analysis, 21-22, 60
training practices, 392, 395-397
triage process, 137
Trivial File Transfer Protocol (TFTP), 157
troubleshooting
  App Engine case study, 146-149
  approaches to, 133
  common pitfalls, 135
  curing issues, 145
  diagnosing issues, 139-142
  examining system components, 138
  logging, 138
  model of, 134
  pitfalls, 135-136
  problem reports, 136
  process diagram, 135
  simplifying, 150
  systematic approach to, 150
  testing and treating issues, 142-145
  triage, 137
turndown automation, 157, 277
typographical conventions, xix
U
unit tests, 185
UNIX pipe, 327
unplanned downtime, 26
uptime, 341
user requests
  criticality values assigned to, 251
  job and data organization, 22
  monitoring failures, 60
  request latency, 38
  request latency monitoring, 60
  retrying, 254
  servicing of, 21
  success rate metrics, 27
  traffic analysis, 22, 60
utilization signals, 253
V
variable expressions, 113
vectors, 112
velocity, 341
Viceroy project, 432-437
virtual IP addresses (VIPs), 227
W
“War Rooms”, 164
Weighted Round Robin policy, 245
Wheel of Misfortune exercise, 173
white-box monitoring, 55, 59, 108
workloads, 296
Y
yield, 38
YouTube, 29
Z
Zab consensus, 302
Zookeeper, 291
```

#### About the Authors

**Betsy Beyer** is a Technical Writer for Google in New York City specializing in Site Reliability Engineering. She has previously written documentation for Google’s Data Center and Hardware Operations Teams in Mountain View and across its globally distributed datacenters. Before moving to New York, Betsy was a lecturer on technical writing at Stanford University. En route to her current career, Betsy studied International Relations and English Literature, and holds degrees from Stanford and Tulane.

**Chris Jones** is a Site Reliability Engineer for Google App Engine, a cloud platform-as-a-service product serving over 28 billion requests per day. Based in San Francisco, he has previously been responsible for the care and feeding of Google’s advertising statistics, data warehousing, and customer support systems. In other lives, Chris has worked in academic IT, analyzed data for political campaigns, and engaged in some light BSD kernel hacking, picking up degrees in Computer Engineering, Economics, and Technology Policy along the way. He’s also a licensed professional engineer.

**Jennifer Petoff** is a Program Manager for Google’s Site Reliability Engineering team and based in Dublin, Ireland. She has managed large global projects across wide-ranging domains including scientific research, engineering, human resources, and advertising operations. Jennifer joined Google after spending eight years in the chemical industry. She holds a PhD in Chemistry from Stanford University and a BS in Chemistry and a BA in Psychology from the University of Rochester.

**Niall Murphy** leads the Ads Site Reliability Engineering team at Google Ireland. He has been involved in the Internet industry for about 20 years, and is currently chairperson of INEX, Ireland’s peering hub. He is the author or coauthor of a number of technical papers and/or books, including *IPv6 Network Administration* for O’Reilly, and a number of RFCs. He is currently cowriting a history of the Internet in Ireland, and is the holder of degrees in Computer Science, Mathematics, and Poetry Studies, which is surely some kind of mistake. He lives in Dublin with his wife and two sons.

#### Colophon

The animal on the cover of *Site Reliability Engineering* is the ornate monitor lizard, a reptile native to West and Middle Africa. Until 1997, it was considered a subspecies of the Nile monitor lizard (*Varanus niloticus*), but is now classified as a polymorph of both *Varanus stellatus* and *Varanus niloticus* due to its different skin patterns. It also has a smaller range than the Nile monitor, preferring a habitat of lowland rainforest.

Ornate monitors are large lizards, able to grow up to 6–7 feet long. They are more brightly colored than Nile monitors, with darker olive skin and fewer bands of bright yellow spots running from the shoulder to the tail. Like all monitor lizards, this animal has a muscular stout body, sharp claws, and an elongated head. Their nostrils are placed high on their snout, permitting them to spend time in the water. They are excellent swimmers and climbers, which allows them to sustain a diet of fish, frogs, eggs, insects, and small mammals.

Monitor lizards are often kept as pets, though they require a lot of care and are not suitable for beginners. They can be dangerous when they feel threatened (lashing their powerful tails, scratching, or biting), but it is possible to tame them somewhat with regular handling and teaching them to associate their keeper’s presence with the delivery of food.

Many of the animals on O’Reilly covers are endangered; all of them are important to the world. To learn more about how you can help, go to *animals.oreilly.com*.

The cover image is from *Brockhaus Lexicon*. The cover fonts are URW Typewriter and Guardian Sans. The text font is Adobe Minion Pro; the heading font is Adobe Myriad Condensed; and the code font is Dalton Maag’s Ubuntu Mono.

### Notes

[^p47-1]: Impact is the effect on users, revenue, etc.

[^p47-2]: An explanation of the circumstances in which this incident happened. It’s often helpful to use a technique such as the 5 Whys [Ohn88] to understand the contributing factors.

[^p48-3]: “Knee-jerk” AIs often turn out to be too extreme or costly to implement, and judgment may be needed to re-scope them in a larger context. There’s a risk of over-optimizing for a particular issue, adding specific monitoring/alerting when reliable mechanisms like unit tests can catch problems much earlier in the development process.

[^p49-4]: This section is really for near misses, e.g., “The goat teleporter was available for emergency use with other animals despite lack of certification.”

[^p49-5]: A “screenplay” of the incident; use the incident timeline from the Incident Management document to start filling in the postmortem’s timeline, then supplement with other relevant entries.

[^p51-6]: Useful information, links, logs, screenshots, graphs, IRC logs, IM logs, etc.
