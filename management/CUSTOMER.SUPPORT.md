# Chapter 27

## Customer Support

Source: [The.Practice.of.System.and.Network.Administration-Vol.I-2017-27.pdf](./The.Practice.of.System.and.Network.Administration-Vol.I-2017-27.pdf), PDF pages 1–19.

The case studies and first-person accounts below reflect the source authors’ experience, as published in 2017.

This document is a macro view of customer support and helpdesks: what they are, how to organize them, how to manage them, and so on. [Handling an Incident Report](HANDLING.INCIDENT.REPORT.md) covers individual helpdesk calls.

A helpdesk is a place, real or virtual, where people can get answers to their computing questions, report problems, and request new services. It may be a physical desk that people walk to, or it may be a virtual helpdesk that people access electronically. The helpdesk is the primary mechanism for providing customer support.

Nothing is more important than an IT organization’s helpdesk. It is the face of your organization. The helpdesk staff make the first impression on your customers and maintain your relationship, good or bad, with them. The helpdesk staff fix the daily problems that are part of living with modern computers and are the heroes who are called when customers have an emergency. A good helpdesk reflects well on your organization. The typical customer sees only the customer support portion of your organization and often assumes that this is your entire organization. Customers have no idea which back-office operations and infrastructure duties are also performed. In short, a helpdesk is for helping the customers. Don’t forget the *help* in helpdesk.

The basics of running a helpdesk are first to simply have one and to ensure that it has a friendly face. The customer support center should have enough staff to support the traffic, a defined scope of coverage, processes for staff, an escalation process for when things go badly, and call-tracking software.

### 27.1 Having a Helpdesk

Every organization has a helpdesk. It may be physical, such as a walk-up counter, or virtual, such as by phone or email. Sometimes, the helpdesk function is unofficial, being the portion of each day spent directly helping customers.

Small SA teams, with just one or two people, frequently have no official helpdesk, but that situation isn’t sustainable. As the organization grows, small SA teams become big SA teams, and big SA teams become enterprise organizations. Organizations sometimes don’t realize that they need to institute a formal helpdesk until it is too late.

Earlier is better when it comes to setting up a formal helpdesk. The best time to do this is nine months before you realize that you should have done this six months ago. Organizations without access to time-travel devices need other techniques. Organizations grow through planning, and developing a formal helpdesk should be part of that planning. If growth is slow, you can simply look for warning signs. One warning sign is when SAs start to notice that their group has grown to the point that communication problems are occurring. Alternatively, SAs might notice that they aren’t able to get project work done because they are continually being interrupted by customer requests. Typically, the SAs might decide that it would be better if, for example, one SA could be interrupted in the morning to handle customer issues and focus on project work in the afternoon, and the other SA could do the opposite. If you are considering such a structure, you are in the formative stage of adopting a formal helpdesk.

The transition from ad hoc to formal helpdesk can be uncomfortable to customers. SAs should expect this push-back and do their best to ease the transition. Communicating the new helpdesk procedures clearly is important.

##### Tell People About Changes

When an organization is establishing a formal helpdesk, whether physical or virtual, people must be told that this is being done. When Lumeta had fewer than ten people, most of them did their own computer support, and Tom intervened for more difficult problems. Eventually, the company grew and had three SAs, including one who was dedicated to PC-related problems and other employees. For all intents and purposes, this person was the helpdesk.

The customers didn’t understand that the various SAs had specializations. This lack of understanding frustrated both the SAs, who felt pestered with inappropriate questions, and the customers, who were confused because every SA wasn’t able to help in every situation. The problem was fixed when an email went out explaining which SAs to contact for which kinds of problems; the message was repeated at weekly staff meetings for two meetings in a row. You can prevent this kind of confusion by making such announcements as the change happens.

Helpdesks do not need to be purely physical locations but instead can be virtual. Problems can be reported and replies returned via email. Telephone, text-based, and audio chat systems can also be used.

Self-help systems are also popular but should not be considered a replacement for systems that involve human interaction. With the pervasiveness of the web, there is no excuse not to have at least a simple repository of documentation for customers on such topics as how to get help or request service activation and solutions to common problems. A simple single-page web site with links to important documents, a wiki, or even a searchable blog (one post per FAQ) can be used. Web-based systems let customers help themselves. These systems can reduce the workload of helpdesk attendants but cannot provide interactive debugging or resolve workflow issues that require real-time interaction. There should be a phone number to call to report that the self-help system is down.

##### Ease Transitions

A 75-person department had a network decoupled from the centralized, corporate IT department. People who were more knowledgeable about the systems did more of the SA duties, and others did less. One semi-technical clerk on staff, named Karen (not her real name), took care of backups and was trained to do most installations and other semi-automated tasks. Nearly every bit of user documentation included the instruction, “Email Karen to get started.” As a result of business changes, the department was eventually required to use the centralized support that other departments in that building used. Customers were frustrated that instead of sending email to Karen, they had to send email to “help.” The personal touch had been lost. Rather than deal with the emotional issues head-on, management simply kept pushing people to use the new process.

Karen had a lot of political clout in the department because everyone knew her, and she could spread pessimism effectively if she chose to do so. Because she was not made part of the process but was shoe-horned into it, she felt that she was being pushed out. Karen eventually quit, and it took a couple of years for the new helpdesk system to be fully accepted by the customers.

The problems could have been prevented if the transition had been handled better, first by understanding what the customers were used to and then by integrating that culture into the new process.

### 27.2 Offering a Friendly Face

A helpdesk should have a friendly face. For a physical helpdesk, the interior design should be pleasant and welcoming. A web-based virtual helpdesk should be equally welcoming, which often means a design that uses soothing colors and readable fonts with the most commonly selected items at the top left of the first page.

The faces of the staff members also should be welcoming and friendly, as should their personalities. Some people have personalities that are suited for customer service; others don’t. That factor should be a consideration when hiring people for your staff. The tone set by the staff will reflect that set by the supervisor. A supervisor who yells at the staff will find staff yelling at customers. A good-natured supervisor who can laugh and is always friendly will attract similar staff, who will reflect such an attitude with customers. It is easier to build a reputation for being friendly initially than to restore a bad reputation. In short, if you are the supervisor, be the friendly person you want your staff to be. Be a role model.

### 27.3 Reflecting Corporate Culture

The look and feel of your helpdesk should reflect corporate culture. Often a helpdesk doesn’t garner respect in a company when people working at the helpdesk buck the corporate culture. For example, a company that is very strict and formal may reflect this with strict dress codes and ways of conducting business, but the people at the helpdesk wear logo T-shirts and jeans, and a visitor hears the sound of a video game being played in the background. A little asking around will find that the helpdesk has a reputation of being a bunch of slackers, no matter how hard they work or how high the quality of the service they provide.

The opposite happens, too. When Tom was at Bell Labs, he worked in an area with very creative, free-thinking people. The dress code was simply “You must be dressed,” and protocol was very relaxed. Tom’s SA team was tasked with being a buffer between these researchers and corporate IT. Anytime these two groups of people interacted directly, it was like a bad sitcom episode.

Take the time to consider the culture and “look” of your helpdesk as compared to that of the customers they serve. Try to evolve to a culture that suits the customers served.

### 27.4 Having Enough Staff

A helpdesk can be helpful only if it has enough people to serve customers in a timely manner. Otherwise, people will look elsewhere for their support.

Sizing a helpdesk staff is very difficult because it changes from situation to situation. Universities often have thousands of students per helpdesk attendant. Corporate helpdesks sometimes have a higher ratio or sometimes a lower ratio. In a commercial computer science research environment, the ratio is often 40:1, and the first-tier SAs have a similar skill level as second-tier SAs at other helpdesks, to meet the more highly technical level of questions. E-commerce sites usually have a separate helpdesk for internal questions and a “customer-facing” helpdesk to help resolve issues reported by paying customers. Depending on the services being offered, the ratio can be 10,000:1 or 1,000,000:1.

Ratios are a no-win situation. Management will always push to have a higher ratio; customers will always demand a lower ratio. You can always increase the ratio by providing less service to the customers, which usually costs the organization more because the customers spend time doing their own SA work inefficiently.

Rather than focus on customer-to-attendant ratios, it is better to focus on callvolume ratios and time-to-call completion. For example, you can monitor the rate at which customers receive busy signals or how long they wait to receive a response to their email or the number of minutes issues take to be resolved—minus, of course, time spent in “customer wait,” as described in [Perils of Skipping a Step](HANDLING.INCIDENT.REPORT.md#286-perils-of-skipping-a-step).

These metrics focus on issues that are more important to the customer. Customer-to-attendant ratios are an indirect measurement of benefit to the customers. In metric-based management, direct metrics are better.

Managing resources based on call volume also presents a more diverse set of potential solutions. Instead of one solution—headcount management—companies can invest in processes that let customers help themselves without the need for human intervention. For example, new automation can be created that empowers customers to do tasks that previously required privileged access, online documentation can be provided, new services can be provisioned automatically via web interfaces, and so on.

You need to have appropriate metrics to make decisions about improving processes. Metrics can reveal good candidates for new automation, documentation, or training for both SAs and customers. Metrics can reveal which processes are more effective, which are used heavily, or which are not used at all.

##### Case Study: Human Web Browsers

Making the customers more self-sufficient can backfire if not done correctly. One company established a web site to give easy access to all the documentation and FAQs that previously had been the domain of the helpdesk staff. Upon examining the web site access logs, the management discovered an unexpected trend. At first, the web site was wildly successful. The hits to the site were coming from the customers. The volume of phone calls was reduced, and everything was happening as planned. However, by the third month, the logs indicated a new trend: The web site was seeing an increasing number of hits from the helpdesk itself.

Investigation showed that people had returned to calling the helpdesk and that the helpdesk attendants were reading answers off the web site. The helpdesk was acting as a human web browser! The situation was rectified when the helpdesk attendants were instructed to try to refer people to the appropriate web page rather than give answers directly. Customers were reminded to visit the web site before calling the helpdesk.

### 27.5 Defining Scope of Support

A helpdesk should have a policy defining the scope of support. This document explains what an SA group is and isn’t responsible for. The components of scope are *what*, *who*, *where*, *when*, and *how long*:

- **What is being supported?** Only the PCs or the LAN itself? Are all PCs supported, no matter which OS is being used, or only certain OSs and certain revisions? Which applications are being supported? How are unsupported platforms handled?
- **Who will be supported?** A particular department, building, division, enterprise, university? What if a person has offices in multiple buildings, each with its own helpdesk? Are only people who pay supported? Are only people of a certain management level and higher (or lower) supported?
- **Where are the customers?** This question is similar to *who* if one is supporting, for example, everyone in a particular building or location. However, *where* also includes support of traveling customers, customers visiting third-party sites, customers performing demos at trade shows, and people working from home.
- **When is support provided?** Are the hours of operation 8 AM to 6 PM, Monday through Friday? How are things handled outside of these hours? Do people have to wait until the helpdesk reopens, or is there a mechanism to reach SAs at home? If there is no support in the off-hours, what should facilities management do if environmental alarms sound or if a fire occurs?
- **How long should the average request take to complete?** Certain categories of requests should be instant; others will take longer. Establishing these goals sets expectations for the staff and customers. Customers expect everything to be immediate if they aren’t told that certain tasks should be expected to

take longer. A tiered structure might list certain things that are to be fast (5 minutes), slow (1 hour), and multiple days (requests for new service creation).

Having a written scope-of-support policy is one of the best gifts management can give to an SA group. Without it, the group either will become overworked from trying to do everything for customers, or will infuriate customers by not having time to help. If you are a manager, it is your responsibility to clearly communicate when it is okay to decline to help and when it isn’t. This should be done in writing, with this policy then being communicated to all people who work for you. The same policy should also be publicly accessible on the internal IT web site to set expectations with customers.

Overworked SAs often do not have a written scope-of-support policy. SAs should create a written log of what they worked on for a week and write down all their open tasks. Showing this list to the SAs’ managers often makes them realize that their people are spread too thin or are working on tasks that are not department priorities. Often, managers are surprised to find that their SAs are, in essence, doing their customers’ jobs for them. Writing a policy defining the scope of support empowers the manager to clearly communicate priorities to the group, and the staff to communicate those same priorities to the customers.

When we investigate SA teams that have gained reputations for being unhelpful curmudgeons, we often discover that the root problem is a lack of written scope policy. Here, having a written policy sets a higher expectation for what the team’s responsibilities are.

Without a written policy, the SAs are simply basing their responses on the accumulation of verbal edicts, and newer SAs simply follow the folklore they hear from their team members. New SAs, trying to be helpful, can undo a precedent without realizing it when they instead go against the ad hoc policy. Even worse, they may break something that shouldn’t have been touched or step on a different department’s toes.

##### Case Study: Wide Scope, Narrow Responsibility

Scope of support also means scope of responsibility. The New Jersey engineering division of a computer-aided design company was entirely in one building. The helpdesk’s policy was that no question was inappropriate, but the SAs had a sharply defined scope of responsibility. This worked because they understood where their responsibility ended. They had complete responsibility for certain issues: If someone reported a workstation problem, the SAs would fix it. For other issues, they would advocate on behalf of the customer:

If the problem was with a WAN link that they didn’t control, they would take responsibility for contacting the corporate network operations center and seeing that it got fixed. With other issues, they acted as a referral service: Someone reporting a burned-out light in the office was referred to facilities management, and the SAs would not take responsibility for seeing the issue to completion. They became a clearinghouse for requests and information.

To save money, the regional sales office was in the same building. The helpdesk was funded by the engineering division, not the sales office, and therefore the scope of responsibility for the sales staff was different. The helpdesk could refer the sales engineers to the proper documentation or suggest where they could get more training, but they could not be involved in helping to set up machines for demos or presentations. The sales staff used the central email server that was run by the engineering group, so email support was complete only if the salesperson was using the email client supported by the engineering group. However, because the support of the sales staff was free, there usually was a limit to how far the helpdesk staff would go to support the sales staff’s requests.

Having a clearly defined scope of responsibility prevented the helpdesk from taking on more work than it could handle, yet let it provide an extremely friendly referral service. It also prevented the sales group from abusing its service by giving the helpdesk the ability to say “no” in a way that had management support.

A helpdesk must have a good process for dealing with requests about technologies that are out of scope. Of course, the helpdesk could simply state that the request is out of scope and refuse to help, but that is an unfriendly response. It is much better to clearly state the scope of what the helpdesk can and can’t do for the person and then offer a little help but give a time limit before that assistance begins. For example, you might say, “We don’t support systems with that video card, but I’ll try my best for 30 minutes. If I can’t fix it, you are on your own.” You might spend 45 minutes on the problem and then politely tell the customer that you’ve reached your limit. The customer will appreciate your effort.

##### Setting Expectations When Working Outside of Job Scope

One of Jay Stiles’s favorite stories takes place before DHCP made network configuration automatic. At one point, Jay worked in a place that had one set of technicians who installed network jacks in offices and a different set of technicians who configured the PC to connect it to the network. The customers often asked the first group of technicians to configure their PCs. The technicians weren’t trained on how to do this but sometimes felt pressured and tried. Rarely were they successful, and more often than not, they corrupted some other configuration while trying to configure the PC. When they made this kind of mistake, their boss would be called and put in a difficult position: How do you answer a complaint about an employee who wasn’t supposed to do a particular task and didn’t do it correctly?

Later, the technicians learned that if they were asked to configure a PC, they should stop what they were doing, back away from the machine, and explain, “Well, that really isn’t my job, but I happen to know a little about these PC computers, and I can give it a try. However, if I can’t figure it out, I’m going to ask you to wait for the people who are supposed to do that kind of thing. Okay?” If they said those magic words, the result was very different. If they were successful, the customer was very happy, knowing that the technician had gone above the call of duty. If they weren’t successful, the customer appreciated that the technician had tried.

The boss started receiving calls that were compliments: “The technician tried to configure my PC and couldn’t, but I want to thank him for trying so hard!”

It’s all in how you sell it.

### 27.6 Specifying How to Get Help

The companion to the scope-of-support document is a document that specifies how to get help: by phone, email, a ticket system, and so on. Certain types of requests may be directed to certain departments, or a unified helpdesk might be the single point of contact that forwards requests as appropriate to individual departments.

Such a document should be a few hundred words at most. Ideally, a shorter version that can fit on a sticker should be put on every new PC deployed. The same image can appear on default Windows background wallpaper images: “CompanyName IT helpdesk: [phone number] [email address] [web site].”

This is one of the most important things IT management can do to help the staff save time. If customers have not been given clear directions on the proper way to get help, they will contact SAs directly, interrupting them at inappropriate times, and making it impossible to get larger projects done. Even worse, SAs may be contacted at home!

### 27.7 Defining Processes for Staff

Helpdesk staff should have well-defined processes to follow. In a smaller environment, this is not as important, because the processes are more ad hoc or are undocumented because they are being used by the people who built them. However, for a large organization, the processes must be well documented.

Very large helpdesks use *scripts* as part of their training. Every service supported has an associated flow of dialogue to follow to support that service. For example, the script for someone calling to request remote access service captures the appropriate information and tells the operator what to do, be it enable remote access directly or forward the request to the appropriate service organization. The script for a request to reset a password would, for security reasons, require callers to prove who they are, possibly by knowing a unique piece of personal information, before a new password would be set.

[Handling an Incident Report](HANDLING.INCIDENT.REPORT.md), discusses a formal process that helpdesk attendants can use to process individual trouble reports.

### 27.8 Establishing an Escalation Process

Escalation is a process by which an issue is moved from the current staff person to someone with more expertise. The first line of operators should be able to handle 80 percent to 90 percent of all calls and escalate the remaining calls to a second tier of support. The people at this second tier may have more experience, more training, and, possibly, other responsibilities. Larger organizations may have four or more tiers; the higher tiers may include the people who built or currently maintain the service in question.

It is common to have a policy that the first tier of support should escalate all calls that get to the 15-minute mark. This has a carryover effect in that the secondtier people, who may be responsible for project-oriented work, now have less time for projects. This situation can be alleviated by designating one second-tier person to sit with the first-tier people each week—that is, to make the helpdesk his or her project for the week. Although upper-tier staff people usually dislike this policy, a sizable organization will assign people to this duty only once every 6 weeks or so. A side benefit of this strategy is that the first-tier staff will learn from the secondtier person. The second-tier person will also get a better understanding of the kinds of issues coming in to the helpdesk, which will help determine which new projects will be the most help to the first tier and the customers.

The escalation process is also what customers use when they are dissatisfied with the support they are receiving. One hopes that this happens as infrequently as possible, but inevitably someone will want to talk to a manager. The helpdesk should be prepared for this kind of request. Large numbers of calls being escalated to the second tier is a warning sign of a larger, systemic problem. Usually, it indicates that the first-tier staff people need more training or do not have the tools to do their job properly. If large numbers of calls are escalated to management, there may be systemic problems with the support the helpdesk is providing.

##### Escalating for Results

Escalation should not exist simply to pacify angry customers. One small ISP’s helpdesk often receives calls from angry individuals who demand to speak to a manager. In that case, the person hands the phone to the person on his or her left, who then claims to be the manager. Although this practice works in the short term or when business growth is exploding, it is not a sustainable way of maintaining a helpdesk.

### 27.9 Defining “Emergency” in Writing

Often, SAs are overloaded because every customer claims to have an emergency that requires immediate attention. SAs may feel that customers are using this claim to boss them around, which decreases morale and increases stress levels.

Having a written policy empowers SAs to know when to push back and gives them a document to point to when they need it. If the customer still disagrees with this assessment, the SA can pass the issue up to someone in management, who can make the decision. This lets the SA focus on technical duties and lets management focus on setting priorities and providing resources.

Every company should be able to define what constitutes an emergency. At a factory, an emergency is anything that stops the assembly line. At a web-based service or ISP, an emergency might be anything that will prevent the service from meeting an SLA. A sales organization might define an emergency as anything that will prevent a demo from happening, end-of-quarter revenues from being booked, or commissions from being processed. At a teaching institution, which has tightly scheduled lecture times that cannot be simply moved or delayed owing to a system outage, an emergency might be anything that would disrupt scheduled technology-dependent lectures, as well as other matters related to the seasons of the school year: new-student arrival, exam deadlines, grade publication, graduation, new-student recruitment deadlines, and so on.

It may sound simple, but writing a definition of what constitutes an emergency can become a political battle. It may be part of a larger SLA document, or a stand-alone policy written to help helpdesk personnel make the right decision. This definition is often included in an escalation policy.

##### Planning Well

When Tom was in college, the business office put up a poster that said, “Bad planning on your part does not constitute an emergency.” The students were offended, but the sign stayed. The truth is that this statement could be one of the most important lessons the university could teach its students before they hit the real world.

### 27.10 Supplying Request-Tracking Software

Every helpdesk needs some kind of software to help it manage requests. The alternative is a collection of notes written on scraps of paper. Although it is simple in the beginning and sufficient for environments with one or two SAs, a system based on notes on paper doesn’t scale. Requests get lost, and management has no ability to oversee the process to better allocate resources. Those are the first qualities that you need in helpdesk software. As a helpdesk grows, software can help in other areas. The scripts mentioned in [Defining Processes for Staff](CUSTOMER.SUPPORT.md#277-defining-processes-for-staff) can be displayed automatically and can be made “smart” by being part of the information-gathering process rather than simply a static screen.

Helpdesk software should permit some kind of priority to be assigned to tickets. This not only helps meet customer expectations but also helps SAs manage their time. An SA should be able to easily list the top-priority issues that have been assigned to him or her.

Another important aspect of helpdesk software is that it collects logs about which kinds of requests are made and by whom. Statistical analysis of such logs can be useful in managing the helpdesk. However, if the software doesn’t capture that information, one can’t gain the benefits of such statistics. This often happens when there is a lot of walk-up and phone traffic. In such cases, it can be useful for the software to permit logging common questions or issues with one click. Caller ID can be used to populate fields with the caller’s information.

Helpdesk software can also automate the collection of data on customer satisfaction. Every day, the software can select a random sample of the previous day’s customers and survey them about the service they received.

##### Case Study: From Good to Bad

A software company with approximately 1,500 employees was using an enhanced version of a freely available call-tracking system. It was very simple to use, with a few different interfaces, the most popular of which was the email interface. The system tracked the customer, department, category, status, who the call was assigned to, how much time had been spent on it and by whom, priority, due date, and so on. Custom scripts produced metrics that management could use to track how things were going. Customers could use a web interface to see the history of their calls and all the other associated fields. Customers could also look at the call queue for the person to whom the call was assigned and see where it was in the person’s priority list. Although the system wasn’t glitzy, everyone was comfortable with it and could get the needed information out of it.

The management information systems (MIS) group, which provided support for databases and the applications that ran on top of them, and which was not a part of the SA group, was commissioned to build a new call-tracking system for the customer-support center. The management chain of that group expanded the scope of the project to make this into one unified call-tracking system that would also be used by the operations group, MIS, and the SA group. Neither the operations group nor MIS had a call-tracking system, so they were designing a system without knowing what makes a good system. No one in the SA group was told of the project, so its needs and those of its customers were not taken into consideration in the design.

The graphical user interface (GUI) system that resulted had no email interface and no command-line interface. Creating a new call involved bringing up ten different windows, each a pop-up that was slow to appear and required mouse movements to chase to the right place on the screen. Updating a call required five or six different pop-ups. It was impossibly slow for many SAs who dialed in over modems to use the system from home. The system didn’t work for anyone with a Mac or Unix system since the client was made only for Microsoft Windows. It frequently took longer to open a trouble ticket than it took to solve the problem, so the numerous small calls were no longer tracked. What was once a quick email process had become a ten-minute endeavor. Several SAs went back to tracking projects on pieces of paper or in their heads.

Customers complained because they could no longer see the status of their calls or where those calls were in the priority queues. They also complained because they couldn’t open a ticket via email any more and because the new system sent them far too much email whenever a small field in the call was changed. All of these complaints were predicted by the SA group when they were suddenly presented with the new system that they would have to start using, but it was too late to change anything.

The system was supposed to provide better tools for producing metrics. However, because a lot of the data was no longer entered into the system, it clearly didn’t, even though the tools it provided for metrics may have been better.

It is critical that helpdesk software match the workflow of the people who use it. If one ticket is opened per week, it is reasonable for the creation of a ticket to take a long time. However, if you expect hundreds of tickets per day, initiating the new ticket should be almost instantaneous, such as sending email. Do not use helpdesk software to introduce radical new workflow concepts. However, if some requests require approvals from the line manager or someone responsible for a particular application, the helpdesk software should be able to get those approvals before passing the ticket to the SA team.

Choosing helpdesk software is not an easy process. Most software will need a lot of customizing for your environment. When you decide to invest in helpdesk software, you need to be prepared to invest in the customizations as well, so that the SAs can use it effectively. If it is a burden to use, they will not use it or will use it only for large projects.

### 27.11 Statistical Improvements

Many sophisticated statistics can be gathered about a helpdesk. For example, you can monitor the rate of escalations to determine where more training is needed. However, when dealing with upper management for budgeting and planning purposes, historical statistics become much more valuable. You can make a better case for your budget if you can show multiyear trends of customer growth, call volume, types of calls, technologies used, services provided, and customer satisfaction. When you are asked to support a new technology or service, you can use past data to predict what the support costs may be.

The value of statistics increases as the organization grows, because the management becomes less directly involved in the work being done. It is often difficult to collect statistics in small organizations, because practices are often less automated and can’t be instrumented to collect data. As an organization grows, statistics are easier to collect, and it becomes more important that they be collected.

##### Identifying the Top Requesters

SAs are usually detail-oriented people. Statistics gloss over the details to find general trends. As a consequence, SAs are often not the best people to generate statistics about helpdesk requests.

When asked to generate statistics from a helpdesk request system, Tom was stymied. How could useful statistics be generated if the organization didn’t first change the software to ask SAs how many minutes of work a ticket had required, provide a classification of the type of work, and tell which department the work was for? Then, after a year of collecting this data, he could produce excellent charts and graphs for a complete analysis.

The problem was that his boss wanted results in 24 hours. His boss suggested a very different tack: The entire process could be simplified if one assumed that all requests took the same amount of time. Tom was horrified but was eventually convinced that, on average, all their requests took an average amount of time.

His boss then suggested a database query that would determine who generated the most request tickets. It turned out that three customers had created 10 percent of all tickets in the past year. Rather than meticulously classifying all tickets, Tom and his boss looked at a few from each of the three top customers and spoke with the SAs who had helped them the most. They learned that one person was continually requesting help with a product that was out of scope; the SAs were nicely helping the person anyway. The manager put his foot down and told the SAs that the product was intentionally not supported because it was too difficult to do so. Future requests for help were to be denied. The manager visited the customer to explain that he had to either become self-sufficient or permit the helpdesk to help him convert to the corporate standard product. The helpdesk didn’t usually provide this kind of conversion service but would be willing to do so in this case.

The second customer was asking a lot of basic questions. Tom’s manager spoke to the customer’s manager about getting the person more training. The person’s manager was aghast at the level of help this person had needed, and took care of the issue.

The third customer was, to put it politely, getting an SA to do the person’s job. This issue was raised to the person’s manager, who took care of it.

In the absence of any statistics, some basic rough estimates were still extremely useful. This one technique was able to eliminate approximately 10 percent of all the tickets entered into the system. That’s a a big improvement!

### 27.12 After-Hours and 24/7 Coverage

As computers become critical to an ever-expanding list of business processes, customers are asking for 24/7 coverage more often. Although a full three-shift staff may be required in some organizations, some very simple ways to provide 24/7 coverage are not as expensive.

For example, you can set up a voicemail box that alerts a pager when new messages arrive. The pager can be rotated among various staff members. The responsibility of the staff person may not be to fix the problem but simply to alert the appropriate person or keep calling various people until someone is found. This requires all staff to have a list of everyone’s home phone number.

A variation on this technique is to have all managers of the customer groups know the home phone number of the helpdesk’s supervisor, who then takes responsibility for calling SAs in turn until one is found. This has the benefit of limiting personal information to fewer people but can wear down a helpdesk supervisor and doesn’t take into account the supervisor’s vacations. However, local solutions can be found, such as rotating this duty among a couple of supervisors.

##### After-Hours Coverage at T. J. Watson

The modern equivalent of a researcher’s laboratory catching fire at night is a major file server being down. In turn, you can treat this issue the same way other industries treat fire alarms. Security personnel have a call list in case of alarms, fires, and so on. You can send alerts to the security personnel, and give them a call list for IT problems. They can start at the top of the list and keep calling numbers until they find someone. Depending on the issue, that person may tell the security guard whom to call. Of course, this method means that the person whom is at the top of the list may never get a good night’s sleep!

In the late 1990s, IBM’s T. J. Watson facility extended the process for handling fire and other alarms to the major computer systems. If a major computer was down, customers could call the security desk and report the issue. The security guards had a separate list of people to call for a computer-related problem.

No matter how SAs are contacted after hours, the person must be compensated. Some organizations have a salary incentive for oncall time, equivalent to a fraction of the employee’s salary and time and a half if the person is called. Other organizations issue compensation time either officially or unofficially. Comp time allows the employee to take off that much time—or 1.5 times that amount, in some cases—without claiming it against vacation time.

### 27.13 Better Advertising for the Helpdesk

Defining your policies and providing announcements online is nice, but rarely will anyone seek them out. In this section, we talk about getting your policies and announcements “out there” and understood.

With the Internet, it is easy to make all policies accessible to all customers. There is no excuse for not providing this material online. However, you must get customers to that web site. Some SA organizations choose to have a portal web site that is the gateway to all their services, policies, and documentation. Because customers will already be using the site to receive information that is important to them, they also will know to go there for information that is relevant to the helpdesk.

Pick the right message. Talk with customers to find out what is important to them. It’s difficult to get people to read something that isn’t important to them. They may not care that server3 will be down during the weekend, but knowing that the database stored on server3 won’t be accessible all weekend will draw their attention.

New policies can be emailed to customers or sent via paper memo if they are particularly critical. Portals can highlight a “policy of the month.” If the message will benefit from repetition, put posters in appropriate places. A physical helpdesk should have its hours posted at all entrances. People spend a lot of time staring at the wall while waiting for their turn; fill those blank walls with the message you want them to remember. Posters that say “Change your password every 30 days!” or “Server3 is being decommissioned on May 1” give good advice and warn of upcoming changes.

Messages are most effective when received at the right time. If server3 is being decommissioned in a month, inform people of that fact every time they use server3.

### 27.14 Different Helpdesks for Different Needs

When an organization grows, it may make sense to have two separate helpdesks: one for requesting new services and another for reporting problems that arise after the service has been successfully enabled. Often, a third group deals with installing the new service, especially if it requires physical work. This third group may be an internal helpdesk that installers all over the organization can call to escalate installation problems. It is not uncommon, though, for this third group to be the second tier of one of the other helpdesks.

The benefit of dividing the helpdesk this way is that the two or three groups can be placed under different supervisors. A supervisor can effectively manage only a certain number of people. This division of labor makes it clear where to place various supervisors. They should all report to the same manager to make sure that communication happens and finger-pointing doesn’t.

Another benefit is that the different groups can be separately trained for the different skills required for their task. This tends to be less expensive than hiring people who are experienced enough to be able to do all the tasks.

Providing a provisioning service is a process that should be the same for all customers. The initial collection of data can be done by someone trained to ask the right questions. This person may have more sales experience than the other staff. Solving installation problems is a highly technical issue but has a narrow focus, and training can be customized to those issues. The separate helpdesk for reporting problems requires people with wider technical experience and background. This division of labor is critical to scaling the organization to very large sizes.

A provisioning helpdesk should have a web-based request system. This can save a lot of data entry and prevent mistakes. Having the customer enter the data reduces the number of typos that can occur when a third party enters data. The system can check for common errors and reject inconsistent or conflicting requests. Phone requests can be replaced by phone assistance for people who need help filling out the web form.

You do not need to create an entirely new software system to support this kind of functionality. The form data can simply be submitted into the regular requesttracking system, which can then handle the workflow.

### 27.15 Summary

For many customers, the helpdesk is the only way they interact with the IT organization. There may be dozens of subteams behind it, but the helpdesk is the only part that people see. Often they assume it is the entire organization.

The helpdesk is the “face” of the IT organization, so make it a happy, friendly one, no matter whether it is physical or virtual. Properly sizing the helpdesk is important and affects not only your budget but also customer satisfaction. In planning your helpdesk, you must define what is supported, who is supported, where the helpdesk is located, when you provide support, and how long customers should expect an average call to last. Constructing accurate budget and staffing plans is made easier by collecting the statistics mentioned in [Supplying Request-Tracking Software](CUSTOMER.SUPPORT.md#2710-supplying-request-tracking-software).

Processes should be defined for staff to follow regarding how they provide support for various services and how issues are escalated. Software must be used to collect statistics on all calls and to track issues that last longer than a single call.

Once those functions are established, a helpdesk can grow in other areas. After-hours coverage can be instituted; policies can be better advertised; and, with high growth, the helpdesk can be split into separate groups for new-service provisioning and trouble reporting.

We discussed a lot of topics in this document, and every issue in some way touched on communication. The helpdesk is how customers communicate with your organization, yet it is often the role of the helpdesk to communicate to customers how, when, and why things are done. Such communication can determine whether IT personnel are perceived as friendly or unfriendly. The statistics that are collected help communicate to management the needs of the organization during planning cycles. Escalation procedures keep the communication flowing when things stall. Having a written after-hours support policy sets expectations with customers and prevents the frustration of unexpected calls late at night.

### Exercises

1. Describe your helpdesk staff structure. 2. How many attendants does your helpdesk have at any given moment? How is that number selected? 3. Which helpdesk attendant in your organization is perceived as the least friendly by the customers? How do you know? How would you help that person improve? 4. How hands-on are you with your helpdesk? Which statistics do you use to manage it, and which statistics are given to your management? If you were one step less hands-on, which new statistics would you need then? 5. Figure out which customers generate the top 10 percent—or 1 percent for larger sites—of all requests. Which trends do you see in those requests, and how can this information be used to improve your helpdesk? 6. Report a problem tonight at 10 PM. Describe how well it went.
