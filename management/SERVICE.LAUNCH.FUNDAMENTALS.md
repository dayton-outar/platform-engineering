# Service Launch: Fundamentals

The case studies and first-person accounts below reflect the source authors’ experience, as published in 2017.

This document is about launching a new service such as an email system, a VPN server, an OS installer, or a web-based application. The way that a new service is launched is every bit as important as the way that it is designed. The customers’ first experiences with the service will color the way that they view the service in the future. First impressions last, especially bad ones.

Launching a new service can be the most exciting part of a job. It is the culmination of intense work by many people. Once launched, the service is something you point at and are proud of. While running the service will fill more calendar months, the launch itself is more intense and requires sustained focus.

This document focuses on the initial launch of a service, using our recommended six-step process. [Service Launch: DevOps](SERVICE.LAUNCH.DEVOPS.md), also deals with launches but covers more advanced techniques. [Service Conversions](SERVICE.CONVERSIONS.md), covers the special case where a launch requires users to be migrated from an old service to the new service. The companion volume, *The Practice of Cloud System Administration*, covers related topics such as techniques for upgrading live services.

## Planning for Problems

While a service launch is exciting and fulfilling, the problem with launching a new service is that something always goes wrong. The road to launching a service is full of tricks and traps that catch us in the most non-obvious ways: Users with a particular web browser can’t access it, the service doesn’t work for remote users, the production environment doesn’t have enough space, the documentation is confusing, and so on.

We should be optimistic that a newly launched service will be a success. However, it is irrational to expect a flawless launch. That said, we tend to be surprised when there are problems, and we don’t plan countermeasures for common problems. This document is about the work (and it *is* work) we must do to make the reality match our optimism.

Luckily, the more we launch services, the more skilled we become. We accumulate knowledge that helps us anticipate and overcome obstacles. We look for potential problems when the system is being designed rather than after it is built. We improve our ability to test edge cases. We find better ways to communicate, delegate, and collaborate. Every problem has a solution. The more we plan, the better the outcome. The more we launch, the easier it gets.

The popular perception of the NASA Apollo program is that only the Apollo 13 mission had problems. People typically think the other missions were trouble-free. In fact, this is not true. Every Apollo mission was fraught with problems. Apollo 7 experienced an assortment of minor hardware problems. During Apollo 8, parts of the computer’s memory were erased, causing it to forget its position. During Apollo 12, the instrumentation panel lost power 36 seconds after launch, and had to be manually switched to backup power.

The possibility of each of those problems had been foreseen and NASA had worked out step-by-step plans for what to do. Astronauts had practiced those plans. What made Apollo 13’s problem special was that it was unforeseen. There was no playbook that the astronauts and NASA engineers could turn to. It was an unplanned problem and there wasn’t much time to invent a solution. The difference between a problem and a crisis is preparation.

As we gain experience we become better at foreseeing potential problems. We keep a checklist of potential issues to plan for. A typical system administrator accumulates this checklist in his or her head and uses past experience to be better prepared in the future. A good system administrator keeps the checklist written somewhere for reference. A great system administrator maintains this list in a way it can be used and updated by everyone in the organization. Each new launch problem results in updates to the list. As a result, with each new experience the entire organization gets smarter.

## The Six-Step Launch Process

The general launch process relies on two major concepts: the **ready list** and iterations of launches and learning.

The ready list defines what it means to be ready to launch. We work on these items either directly or via delegation. The list is dynamic, with new items being added as bugs or regressions are found. Once all the items on the ready list are satisfied, we can attempt to launch the service.

The launch itself is a loop whereby we do a small launch, learn from it, then repeat. For example, the initial launch may be a beta release rather than an actual launch that is visible to customers. Each iteration may result in adding more items to the ready list based on what we learned.

After the system is launched and fully in production, there is more learning to do. We pause to perform a **retrospective** where we look back and reflect on the entire process to see what we learned and how we can improve future launches.

The entire process can be summarized as follows:

- **Step 1: Define the ready list.** Define a list of tasks that must be complete before the launch may proceed.
- **Step 2: Work the list.** Schedule, work on, and mark as complete each item in the list.
- **Step 3: Launch the beta service.** Launch to one of many test areas where the assembled pieces are verified.
- **Step 4: Launch the production service.** Launch the service into production.
- **Step 5: Capture the lessons learned.** Capture the lessons learned for posterity and to educate the organization.
- **Step 6: Repeat.** Begin the process again.

### Step 1: Define the Ready List

The ready list is a list of tasks that must be completed or assertions that must be satisfied to indicate that we are ready to launch. In other words, this is the list that tells us when we are done.

Maintaining this list helps team members keep focus and avoid work that is unrelated to the shortest path to launch. It communicates your progress to management, other teams, and customers.

### What to List Items on the ready list generally fall into four categories:

- **“Must have” feature set:** There may be other features requested, but the launch cannot happen without these features. This part of the list serves as a contract between the customer and the provider. Setting expectations in this way prevents the problem where, after launch, stakeholders are surprised that certain features are missing.
- **“Would be nice” feature set:** All features that are not must-haves fit in this category.
- **Bugs to be fixed:** Testing will reveal bugs that must be tracked. If not tracked, they will not be fixed, or there will be a duplication of effort as multiple people try to fix the same issues.

- **Assertions and approvals:** This is the list of things that must be true before launch. For example, the legal department has approved the trademark, security audits are complete, the CEO and head of product management have signed off, and so on. This checklist contains technical items such as verifying disk capacity or confirming that a particular ISP connection is up.

### List Attributes At a minimum each item in the list should have a brief title, a priority, the name of who is responsible for completing the task, a link to the history of the task, and a status.

The priorities should be standardized among all projects to avoid confusion, to make it easier for people to move from project to project, and to prevent employees from spending time reinventing the wheel with each project. For simplicity’s sake, we focus on three priorities: must have, low priority, and not required.

If there has been a decision to not include a feature, it may be less confusing to list it as “not required” than to delete it from the list. If it is deleted, people will be unaware of the decision to not include the item in the launch.

If your organization has a standardized priority scheme for bugs, adopt it for use on the ready list to avoid confusion. For example, at Google there is a carefully defined set of bug priorities from P0 to P6. Of those, the definitions of P1, P2, and P5 map well to the must have, low priority, and not required priorities. Therefore we would adopt those priorities even though they are not contiguous numbers.

### Items Should Be Specific and Unambiguous

The description of an item should be measurable. “Make things better” is ambiguous. “Improve sort performance by 20 percent” and “System enables RAID controller” are better. One way to make sure goals are well written is to follow George T. Doran’s advice in his 1981 article, “There’s a S.M.A.R.T. Way to Write Management’s Goals and Objectives.” One is in a better position when goals are specific, measurable, achievable, relevant, and time-bound:

- **Specific:** The goal addresses a specific area for improvement.
- **Measurable:** The goal’s success (or progress) can be quantified.
- **Achievable:** It can be realistically achieved given available resources.
- **Relevant:** It is worthwhile, and in line with broader goals.
- **Time-bound:** The goal has a deadline.

Doran’s article discourages us from trying to achieve all five qualities for every item. For example, listing a deadline is overkill for simple bug reports.

### Storing the Ready List The list should be viewable to all stakeholders—both people on the team and external people. This transparency enables everyone to understand what is going on outside their silo. It also helps management see progress and spot bottlenecks. Updates to the list should be made by those doing the work rather than requiring all updates go through a central coordinator. This improves agility.

Following are the most common places to store and maintain the list. Pick whatever fits your team and corporate culture the best.

- **Bug or ticket system with tags:** One method is to assign a bug ID or ticket to each item in the list and label the items in a way that one can easily query for all the items. For example, some systems permit tagging. One assigns a tag to all items in the ready list. For example, if the project is called “email2017,” each item is tagged as tag:email2017launch01. Publish a launch status page URL that queries for items with that tag, sorted first by priority and then by status.
- **Bug or ticket system with watchlists:** Some issue tracking systems permit one to create a “watchlist” or “hotlist” of items, which provides similar functionality to tagging. Issues are added to the watchlist. People who are subscribed to that watchlist can treat the list as a unit, with the ability to sort and filter the items within it.
- **Kanban board:** Kanban boards can be either index cards on a wall or electronic equivalents such as LeanKit or Trello. Cards are placed in columns: a “backlog” column for issues that aren’t being worked on yet, an “active” column for issues that are in progress, and a “completed” column for items that are finished. The history of the item is stored on each card, and may include a URL to a bug or ticket if needed.
- **Online or shared spreadsheet:** One can list each item in a row of a spreadsheet, using columns for title, priority, and so on. Web-based spreadsheets can be viewed by anyone at any time, and with the right permission system can be updated by anyone on the team. For simple items, you can keep the history in the spreadsheet; for more complex items, you can list a URL of the related bug or ticket.

- **Offline status spreadsheet coordinator:** With this option, a designated coordinator maintains a spreadsheet listing the items and their statuses. All updates go through this person, and he or she emails the updated spreadsheet to all team members periodically. For example, if you have completed an item, you would email the good news to the coordinator, who then updates the spreadsheet. Once a week the coordinator emails the latest spreadsheet file as an attachment to everyone involved. We recommend against using this technique, for several reasons. People will always be working from outdated information. Having a third party update the spreadsheet can be a source of errors. The coordinator becomes an information bottleneck. Transparency is reduced. This method may have made sense before the web, but now sharing information is easy and convenient. We list this technique here only so you can recognize it when you see it.

The benefit of tagging or using a watchlist is that it keeps both the items and their history in one place. The Kanban system is growing in popularity as new online tools are making it easier to use. It provides transparency and a more modern way to set the cadence of the process.

Consider using systems that give a visual sense of accomplishment. Completed items should stay on the list but be marked as complete by changing their color or font, or by moving them to a list of completed items. If the system you use makes any completed items disappear, you are literally hiding your accomplishments.

Making accomplishments visible motivates the team. The team will see the completed portion growing over time, which gives them a sense of accomplishment. It is emotionally draining to have your accomplishments hidden, and to not see the progress you and the team are making.

A visual indication of progress also helps communicate with management. You want to accurately communicate progress to management, especially executive management. If they peek at the list weekly, they should see more and more items being marked complete. If what they see is a list of what’s left to do, the subliminal message is that you haven’t even started. Upper-level managers have too much on their plate to remember what the list looked like last week. Expecting someone to remember that last week there were 20 items and this week there are only 15 is expecting too much.

### Step 2: Work the List

Now we work through the items in the list. Some we do ourselves, others we delegate.

If you have never managed a project before, it may surprise you how much work is required to keep people accountable. If they do not feel accountable for the work they agree to do, they will not do that work. This is not because they are evil or lazy, it is simply that they are generally overloaded. Urgent items get attention and others slip. If we overdo it, however, we are perceived as nagging or micromanaging. The ready list helps keep people accountable and on schedule without requiring you to nag or micromanage.

First, make sure that everyone knows what is expected of them and their deadlines. Don’t assume they “just know.” You can do this by making sure the ready list includes the specific deliverable rather than being vague. For example, “Installer works on Dell 740DX hardware” is better than “Improve installer reliability.”

People should be aware of a task’s deadline. Therefore deadlines should be agreed to by all and recorded in the ready list so there is no ambiguity.

People are more likely to stick to deadlines when they have control over them. For this reason, it’s best not to set a deadline, but rather ask the person taking the task to suggest a deadline. Nine times out of ten, that person will suggest a deadline that is the same or sooner than what you expected. You can accept this suggestion and the person will feel good because he or she was in control. You will have to negotiate an earlier date only if the individual suggests something that would be incompatible with the overall schedule. Even then, because the date was agreed to by negotiation, the person will feel some sense of control.

Second, keep people on track. Now that people know what they must do and the deadline, it becomes your job to make sure they are held accountable.

Some teams use weekly status meetings to monitor progress. The purpose of the meeting is to verify that deadlines are being met and to troubleshoot any roadblocks that might prevent deadlines from being met.

Some teams start each day with a very fast status meeting called a “stand-up.” This technique is borrowed from Agile methodology. People stand in a circle and take turns announcing three things: what they completed yesterday, what they plan on doing today, and which roadblocks they are facing. To keep the meetings brief, there is no discussion, just the individual announcements. Because people are standing, there is a physical reason to keep the meeting short. Assisting co-workers with their roadblocks is done only after the meeting ends. As the meeting breaks up, people socialize a bit, as people often do. However, now it is productive socialization, because people initiate ad hoc discussions about the various roadblocks raised. It is more efficient to have two people discuss a roadblock together than to have the entire team pause and wait for the length of the discussion during the meeting.

### Single-Person Teams Need a Ready List, Too

You might not think you need to maintain a ready list if you are a solo SA or the only person on the project. The opposite is true. Keeping other people apprised of your status can be even more important in this situation. It keeps the work you do visible to management, and keeps customers aware of the status in a way that prevents them from bothering you to ask, thereby distracting you from your work.

### Step 3: Launch the Beta Service

Before launching to real customers, the service should be launched to an isolated environment for testing. The isolated environment is called a **staging area**. The bugs and issues found there should be added to the ready list.

The concept of staging areas usually applies to software releases, but you can actually apply it to any service launch. You may be introducing a new OS release to your automated OS installation system; a beta test would involve testing it with friendly users. You may have a process for onboarding customers that is being reengineered; a beta test would involve walking through the new process with a person who pretends to be a newly hired employee.

There are many different types of staging areas. Each has a different name and a very specific purpose.

- **Dev:** The Dev environment is the Wild West of staging areas. It is where developers can deploy to at any time for whatever testing purposes they have. There are no gates.
- **Quality Assurance:** The QA area receives a release candidate from the developers. The QA process tests the release and either accepts or rejects it, and provides a list of bugs found. New releases enter this stage based on QA process schedules.
- **User Acceptance Testing:** The UAT area is a sandbox for external customers to test a release. They bring real-world data to the system and verify their procedures work, or learn how to adapt them to the new release.
- **Beta or Early-Access:** The Beta area is where approved customers have early access to a release so that they may provide feedback or report bugs.
- **Production:** This is where live users use the service.

Each stage has entry and exit criteria. The entry criteria determine when the testing starts. For example, the QA stage receives a release candidate from development, whose intention is to deliver a software package that the developers believe is ready for use. The exit criteria restrict or **gate** whether the release passes or fails. For example, the gate to successfully leave QA may be that all automated and manual tests pass. The gate to successfully leave UAT may be zero P0 bugs and no more than three P1 bugs. The larger the project, the more important it is that these exit and entry criteria be explicit and written.

### Step 4: Launch the Production Service

Finally we are ready to launch. The must-haves are done. As many of the nice-tohaves as possible are complete, too.

Launching is both a technical task and a communication task. The technical task involves pushing code to the production environment and any other step required to expose the new release to users.

The launch may require some kind of final approval. This often takes the form of executive approval, a product manager who signs off, or a senior team member’s nod of agreement. Later we’ll discuss replacing such mechanisms with comprehensive automated testing.

Communication is equally important. We need to communicate that the launch is happening, what is new since the last release, and most importantly whether any items are incomplete (these need to be clearly enumerated).

You should never launch to all users at once. That is too risky. Launch to a few users and expand. Go beyond a simple beta user process. Release to waves of users, such as different departments or teams, starting with the most risk tolerant and ending with the most risk averse. Gradual rollouts are also discussed in [Service Conversions](SERVICE.CONVERSIONS.md#216-gradual-roll-outs).

### Step 5: Capture the Lessons Learned

Congratulations on the launch! The next step is to learn from this experience. A retrospective is a live meeting where the launch process is reviewed. It is called a retrospective because it involves looking back at what happened. We determine what went well, what didn’t go well, and what needs to be improved. The purposes of the retrospective are fourfold:

- To get agreement about needed improvements from the project members.
- To educate the entire team about issues visible to only a few.
- To communicate problems to management, particularly ones large enough that their resolution will require additional resources.
- To spread what you learned to the entire organization so all can benefit.

It is best to do a retrospective in a live meeting with all the stakeholders and team members in one room, or in a video conference. Use this meeting to build a document collaboratively. This document should then be disseminated to all involved, and placed on a wiki so it can be reviewed. A retrospective document should have sections including an executive summary, a timeline, successes and failures, and short- and long-term changes that should be made.

- **Executive Summary:** What happened, when, and briefly what went well and what should be improved in the future. This section should be written in nontechnical, business language that any executive could understand.
- **Timeline:** A high-level summary of when the project started, when it ended, and major milestones in between.
- **Successes:** What went well. Celebrate what you did right.
- **Failures:** What didn’t go well. Be honest about areas of improvement. Do not phrase them in terms of blaming individuals, but describe the facts of what happened and how it affected the project.
- **Short-Term Changes:** What should be done differently; the low-hanging fruit that can be achieved simply.
- **Long-Term Changes:** What should be done differently; changes that are large enough that making the change may require the creation of a project, assigned resources, budget approval, and so on.

Once a retrospective document is complete, each of the short-term and long-term changes should be entered into the bug tracking system so they can be triaged along with other kinds of requests. This is how we make sure that operational concerns are not forgotten.

Creating a retrospective document and publishing it for the entire company to see is actually quite a radical move. Traditionally businesses announce their accomplishments and hide or deemphasize their problems. Individuals tend to hide their mistakes, struggle through their problems silently, and confide in only a small group of people about their shortcomings. Here we not only discuss them but also celebrate them by documenting them for everyone to see.

We do this because it is the best way to learn. If we keep our problems to ourselves, we restrict ourselves to only the solutions that we think of. We rob ourselves of learning from other people’s suggestions and experiences. If a team doesn’t share its problems and learning with other teams, we miss out on an opportunity to educate the entire organization.

Most companies have many IT teams. Imagine a company with ten IT teams. If they do not share their issues and learn from one another, one could imagine ten launches in a row where each team has the same problem. External customers do not realize there are many different teams. They simply see a big company that can’t seem to get anything right since it keeps repeating its mistakes.

In a traditional model, our problems define our weaknesses. A better model is one where problems and solutions are discussed openly. Problems are a means by which we learn something valuable. Each problem is an opportunity to share this new wisdom as widely as possible. Instead of defining our weaknesses, the discovery and communication of a problem makes the entire organization smarter.

### Step 6: Repeat

There’s no such thing as a final launch. There will be new releases with new features, bug fixes, and so on. Each requires another launch.

Each launch should have its own ready list and use a similar process, though future launches tend to be smaller and do not require as much bureaucracy.

As discussed in [Software Upgrades](SERVICE.REQUIREMENTS.md#1664-software-upgrades), software requires upgrades. Even if you do not add features or find bugs to be fixed, the underlying libraries and frameworks will need to be upgraded as security holes are discovered in them. They start out fresh but they go bad. Joshua Corman (2015) explained it this way: “Software ages like milk.”

## Launch Readiness Review

A launch readiness review (LRR) is a process created to assure successful launches by comparing the state of the service against launch readiness criteria (LRC).

### Google’s Production Readiness Reviews

The term LRR comes from Google SRE culture, which is described in the book *Site Reliability Engineering: How Google Runs Production Systems* (Beyer, Jones, Petoff & Murphy 2016).

### Launch Readiness Criteria

The LRC include basic tasks such as verifying that stakeholders have approved the launch, that there is a disaster recovery plan in place, that data backups are working and have been tested, and that various security audits have been completed. The LRC also include items that come from the lessons learned during past launches.

A major search engine once launched a new analytics tool. It was unexpectedly popular on its first day and attracted ten times as many users as had been planned for. It quickly became overloaded. There was no plan in place for how to handle this situation, so the service was embarrassingly slow as the operations team scrambled to make adjustments and scale the system. The launch became a big black-eye for the company.

The organization already had launch criteria that product management must produce usage estimates and load testing must be performed to verify that the system could handle such a load. However, as a result of the unexpected popularity of this one service, the organization added a new criterion: The team must have a plan for what to do in the unlikely event that the service turns out to be ten times more popular than expected. Whether this means that the service will be disconnected (and who can approve this decision), or that the service will be restricted to a small subset of users, these decisions must be made ahead of time so that there is prior agreement about what to do, and operations can test these plans ahead of time.

Note that the launch criteria list is not the same as the launch procedure. The latter may itself be a checklist of technical steps required to actually launch, such as DNS updates, firewall updates, load-balancer changes, and so on.

When all the criteria are met, we are ready to launch. Management should be strict in applying the criteria. If exceptions are permitted, the business risk should be quantified so that managers understand the implications of what they are agreeing to.

### Sample Launch Criteria

Here are some sample items that might appear on launch review criteria lists:

- **Service is monitored.** Service is monitored both for uptime and performance. Internal systems are measured for resource utilization, access counts, and error counts.
- **Monitoring alerts are configured.** Alerts trigger for SLA violations, and for situations that would lead to SLA violations.
- **Backups and restores are tested and working.** Data storage has a backup mechanism, and restores have been tested.
- **Standard authentication, authorization, and access control have been tested and are working.** The service uses the company’s standard AAA service, and access has been successfully tested.
- **SLA is defined.** The service SLA is defined in writing.
- **Service request model is in place.** Customers will be able to submit service requests using the standard tools, and these requests will be properly handled.
- **User documentation is complete.** The documentation required by users is complete and verified.
- **Operational documentation is complete.** The documentation required by operations to run the service is complete, and there exists a written procedure for what to do for each exceptional situation (alerts).
- **User training is complete.** Users have received training. If training cannot begin until the system is launched, how will people be trained before their need for the service is critical?

- **Load testing is complete.** The system has been load-tested using simulations of the expected and optimistic traffic levels.
- **Ten-times response plan is developed and tested.** If on the first day the service receives ten times as many users as expected, which steps will be taken? This plan must be written and tested.

### Organizational Learning

Sometimes problems identified during a retrospective become candidates to add to the launch readiness checklist. The LRC is how we capture learning in an organizationally beneficial way.

Without an LRC process each team will probably make similar mistakes with its launches. They’re all in the same environment, with the same trips and traps, and all probably equally naive when doing their first launch; or perhaps their last launch is such a distant memory that this launch may as well be the team’s first launch.

LRC are a way to improve the organization’s collective knowledge. They communicate learning from one group to another so that people do not repeat one another’s mistakes.

### LRC Maintenance

As the LRC list grows, it can become a bureaucratic burden. We can prevent that problem by being cautious about adding new items, and by removing obsolete items.

Each item on the list incurs a cost to the company. Whether the cost is just answering a question on the LRC form or the actual mitigation, it is work that someone must do for each launch.

As the list gets longer, fear of the list grows. If that happens, the LRC become an excuse not to launch, and the company becomes calcified, unable to adapt and change and meet new business needs. Alternatively, people may find ways to work around the LRC, which means unofficial launches occur that don’t benefit from the learning of others. This change will not happen overnight. It is a long, slow process. One day we wake up and find that launches are going wrong because the LRCs are no longer being used.

The list should grow slowly through experience. Growing the list in this way keeps the criteria based in reality, and prevents well-intentioned individuals from adding hypothetical situations that are unlikely to happen. It avoids the temptation to add every seemingly good idea to the list.

Items should be updated and rewritten as needed. Obsolete items should be removed. This is difficult because there is personal risk in shrinking the list but no liability in adding to it. People fear that they will be personally blamed if they suggest removing an item that later enables a problem or outage. The bigger risk is that the list will become so big and scary that launches become less frequent. The benefits of the small batches principle will disappear: Each launch will include a larger batch of changes, people will get out of practice in doing launches, and so on. This will discourage launches, which further delays or blocks new features. The result is that the company becomes immobilized. Therefore it is everyone’s duty, and management’s responsibility, to assure discipline in list maintenance.

The best way to remove something from the list is to make it obsolete because it has become automatic. For example, suppose one LRC item is to explain how the service survives single-host failures. Each launch designs and implements a failover mechanism that then is documented, tested, reviewed, and evaluated as part of the LRC. If the company creates a centralized load balancing service that can be used on a subscription basis (“load balancer as a service”), the labor involved in satisfying this LRC item becomes significantly easier. The criteria can be satisfied by simply showing that the system uses the platform’s default load balancer.

## Launch Calendar

Large organizations that engage in many simultaneous launches require some kind of coordination of these activities. Usually this takes the form of a launch calendar. The launch calendar may be a simple chronological list of future launches on a wiki or something more complex, such as a groupware calendar created via Microsoft Exchange or Google Calendar.

As organizations grow larger, it is more likely that some people will be out of the loop, which can lead to errors. The calendar creates awareness of future launches. In a small organization it is easy for everyone to know about future launches since they are probably involved. Having a shared launch calendar prevents conflicts. It also makes it easier to schedule isolated launches. Some launches are better done in isolation for technical reasons, or simply to avoid confusion.

The calendar also helps teams to plan around one another’s launches. For example, the launch of something externally visible may require marketing announcements and other work that needs to be done ahead of time. A calendar enables this activity to be planned and coordinated.

Some items on the calendar will be listed on their final planned date; other launch dates are approximate or aspirational. Items in the far future are more likely to be estimates, while items happening soon should be more accurate. It is useful to indicate items whose dates are still changing with a tag, icon, or different color.

Each team should be held responsible for keeping its entries on the launch calendar up-to-date.

## Common Launch Problems

This section describes the most common launch problems. An observant reader may notice that most of these problems can be avoided by using the DevOps methodology described earlier. DevOps culture also encourages the early involvement of operations, which would prevent the other issues.

### Processes Fail in Production

Deployment processes that worked in other stages may fail in production. Often the deployment process is different in each of the dev, test, beta, and production environments. This often happens when a different team is responsible for each stage, and they don’t communicate or share code. The members of each team develop their own installation process. Often the production environment is completely different, and the production team finds itself developing a procedure from scratch, without the benefit of experience from deploying to the other stages. Or, perhaps the beta and production environments are similar enough that one process should work for both. However, the production environment has changed in the many months since the last time code was pushed to it and the process breaks. Either way, the lesson here is that the best launch experience comes from having a unified deployment mechanism across all stages. It should be developed as part of the code, not as something left to do the day the service is pushed to production. One of the goals of containers such as Docker is to provide a single delivery format that works in all stages.

### Unexpected Access Methods

The way users access the service is different in all stages prior to production. Many service launches fail because tests were performed using one access method but customers actually access it another way. In one case, an SSL VPN was going to be how 80 percent of the users would access the service but the developers and testers used the service only without the VPN. In another case, the system was tested only by local users with fast networks; no one considered that actual users would be accessing the system over high-latency links, which would cause certain protocols to time out. The lesson here is to make sure the testing is as similar to the access method that real users will use.

### Production Resources Unavailable

Assumptions are typically made about resources available in production. Suppose shortly before launch the developers announce that the new service will require a large amount of disk space, CPU, or bandwidth resources. Developers simply assumed that these resources would be available without making their requirements known ahead of time. The real-world logistics of installing additional servers or acquiring more bandwidth are complex and often take weeks to months of pre-work. Developers are often shocked to learn that there isn’t a petabyte of spare disk space they can use, or that adding ISP capacity can easily take 12 or more weeks. The lesson here is to involve operations with resource and capacity planning early in development, not at the end.

### New Technology Failures

The first time a new technology is used, the launch takes longer. The first time you use a new technology, it will take considerably longer to get it working in production. This is true for a new language, framework, database, or other service. For example, if your organization is a C# shop, the first time you try to use NodeJS in production you will discover dozens of unexpected issues, each requiring a week of experimentation, design, and testing. If you have great operational expertise in one brand of database, and a developer decides that some new data should be in a different database system, the developer may be surprised to discover that adding this database requires developing new expertise and procedures. Sadly, the true believers in the new technology can’t possibly imagine that any of these tasks will take any time because the new technology solves all problems (or so they think). As a result, the use of these new technologies may not even be communicated until the last minute and the launch will be delayed. The lesson here is that new technologies require weeks and possibly months of operational work before they can be used in production. This factor should be weighed as part of the decision to adopt the new technology. Descriptions of such failures have been documented in articles such as “Why MongoDB Never Worked Out at Etsy” (McKinley 2012) and “Providence: Failure Is Always an Option” (Punyon 2015).

### Lack of User Training

No training for users leads to disaster. For most new services, users require training or at least simple screenshots that walk them through the steps to access the system. Many times the excuse to delay training is the following Catch-22: Training requires that the system be launched, but the launch can’t be done until the training is complete. The organization, therefore, decides to launch without training and ends up in a sticky situation where people can’t go back to the old system (it has been disabled) and don’t know how to use the new system. One solution is to build a staging area used exclusively for training purposes. This provides a platform for training without disrupting the legacy production service. The lesson here is that the training process must be planned for just like any other feature.

### No Backups

Suppose that shortly before launch, operations points out that the new system has no disaster recovery plan. The system is constructed in a way that makes doing backups and restores very difficult. More importantly, it can take weeks, perhaps months, to test the restore process until it is sufficiently reliable. The lesson here is that disaster recovery features must be built into a service and can not be designed as an afterthought.

## Summary

Launching a service requires a lot of coordination. We must be prepared for unexpected surprises, especially the first time we use a new technology or are in a new environment.

There are six steps to launching a new service: We define the end goal in terms of a ready list, or tasks that must be completed. We work this list. We launch into a beta test environment, and then we launch into production. We capture lessons learned, and we repeat the process.

The ready list should be stored where everyone has access to it to improve transparency and communication. Consider using a bug or request-tracking system, a Kanban board, or an online shared spreadsheet. Such coordination is important even for a single-person team.

As we launch more services, we start to accumulate best practices and guidelines that will help future launches be successful. We can share these lessons learned by assembling them into a launch readiness checklist used for all launches.

Some common launch problems to be aware of include differences between test environments and production environments, operational difficulties the first time a new technology is used in production, and lack of resource and capacity planning.

The more launches you are involved in, the better you will get at launching new services.