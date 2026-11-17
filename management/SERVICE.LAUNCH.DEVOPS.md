# Chapter 20

## Service Launch: DevOps

Source: [The.Practice.of.System.and.Network.Administration-Vol.I-2017-20.pdf](./The.Practice.of.System.and.Network.Administration-Vol.I-2017-20.pdf), PDF pages 1–19.

Now that we understand the fundamentals of launching a new service, how can we optimize the process?

The DevOps movement has a number of principles that we can adopt to improve any operational process. DevOps is a way to improve IT that involves developers and operations working in an integrated fashion. Yet, DevOps practices apply to any operational situation. You don’t need developers to benefit from the DevOps principles!

This chapter focuses on two ways of replacing a risky big launch with many smaller launches. By doing so, we enable the benefits described in Chapter 2, “The Small Batches Principle.” The first way is to use continuous integration and deployment techniques, which create the ability to rapidly and confidently launch new releases into production. The second way is to first develop a minimum viable product (MVP) and then enhance it with many small improvements over time.

##### DevOps or Rapid Release

The IT department of one Fortune 100 company started a project to adopt DevOps practices but found huge resistance at every level. People had many preconceived notions of what DevOps was. The myths and misconceptions held by employees confused the issue and derailed the entire project.

The team took a step back to analyze their situation. They realized what was important to them wasn’t DevOps per se, but rather that the company was being killed by the fact that it was so damn difficult to deploy new software releases. If software was never improved, the company couldn’t improve. It was being beaten by its competitors, which were able to rapidly improve everything from their web site’s shopping cart, to how price tags were printed, to how products were shipping to their stores, to making sure their snack shops didn’t run out of hotdogs. All of these things were driven by software.

Members of the IT department went back and rewrote all their presentations. They removed the phrase “DevOps” and instead used the phrase “rapid release.” They extolled the virtues of being able to rapidly and confidently release new software upgrades. They spotlighted one project that had successfully adopted the new testing and deployment processes that meant a new feature could go from concept to the field in a week instead of a year.

Other teams accepted the concept now that the focus was on a benefit they could appreciate. Soon this Rapid Release Initiative was transforming the company.

### 20.1 Continuous Integration and Deployment

The six-phase launch process discussed in Chapter 19, “Service Launch: Fundamentals,” included various staging areas that a software release flows through: development, testing, beta, and production. Each stage reduces the risk of surprises and failures at the production launch. We can think of those stages as practice for the actual launch. If practice makes perfect, shouldn’t we practice a lot?

While the staccato movement from one staging area to another is commonly used throughout the industry, it is non-optimal and possibly toxic. It is very labor intensive. Each stage has a team of people associated with it who must be available or the process halts. It keeps each team in a silo, optimizing for their own needs instead of the greater good. It encourages people to game the system, because the focus becomes getting work past the gate rather than creating good output. It also encourages large batches. If QA receives a new release every quarter, the number of changes since the last release will be complex enough that this team must test everything from scratch. As an optimization it is tempting to send a new release to QA less frequently, but this simply creates a bigger testing burden. If a bug is found, the developers need to isolate it to one of the changes, which is now an even larger list.

A more modern technique is called continuous integration and continuous delivery (CI/CD). This practice automates software building, packaging, and testing so that it can be done frequently. In fact, the cycle is repeated for each change to the source code. Thus, instead of disconnected tests done periodically, it is a continuous process. Each release is automatically installed in a stage and put through a testing process. If all the tests pass, the release is installed in the beta staging area, where more tests run automatically. In some environments these tests gate automatic deployment to the production environment.

#### 20.1.1 Test Ordering

The order in which tests run is important. The most expensive tests should be placed at the end of the testing pipeline. A less expensive test failing early prevents us from having wasted resources on a more expensive test. For example, if there are manual tests, do them at the end. These tests need to be done only if all the automated, and therefore less expensive, tests are successful. Imagine spending a morning running manual tests and then running the automated tests, only to find one failed. You just wasted your morning.

If there are manual tests, you may choose to run them only occasionally. In fact, some sites run the automated tests in a loop, always pounding on the newest release. The manual tests, if there are any, are run only on release candidates.

Sites with fully automated testing can achieve truly continuous integration and delivery. That is, each change triggers a release that is built and tested. This way, if there is a failure, team members know exactly which change introduced the problem—the most recent change! Batching up tests in a daily, weekly, or monthly release means a failure could be present in any of a large batch of changes and is more time-consuming to debug.

#### 20.1.2 Launch Categorizations

We classify the way organizations perform launches into four general categories: chaotic, periodic, continuous, and push-on-green.

- Chaotic: Releases are brought into testing at random times. The building, deployment, and testing of a release are not automated and may be ad hoc. Therefore testing may be done differently depending on who does it.
- Periodic: Releases are done on a schedule such as daily, weekly, or monthly. Usually the entire build and deployment processes are fully automated. Testing is generally automated or mostly automated with a few exceptions that are done manually if all automated tests succeed.
- Continuous: Each change to the source material triggers an automatic creation of the next release, build deployment, and testing. Because these steps happen so frequently, they must be automated. The actual release to the production stage requires human approval.
- Push-on-green: Here the decision to launch to production is automated. In other words, the push to production is approved when the status of all tests is green (success). This is extremely rare and requires extraordinary measures.

The latter categories permit faster iterations and achieve better confidence through more complete automated testing. They benefit from the small batches principle. Features get to production sooner, and the improved confidence they instill makes it easier for the organization to experiment and try new things. The ability to do rapid releases is a major goal of the DevOps philosophy. Volume 2 of this book series has more examples of this kind of methodology.

It is difficult for a chaotic project to jump all the way to push-on-green. Instead, one generally seeks to improve a project one level at a time. In fact, a reasonable goal is for all projects to aspire to periodic or continuous launches. The extra investment required to get to push-on-green should be reserved for only the most important projects. Making all projects push-on-green is a waste of resources; your time is better spent elsewhere. Push-on-green requires an extremely high level of confidence in your automated testing procedure that can be achieved only by involving everyone from product management to developers to operations.

The best way to achieve a specific level for all projects is to create a development platform that makes it easy. If projects start out using the platform, they skip the lower levels. Make right easy. If the lazy way to do something guides people to the best methodology, it becomes the default.

Etsy’s development platform starts new projects at or near push-on-green. Every individual change triggers the full testing and deployment suite. As a result, Etsy launched 6,000 times in 2015. Such a large number of launches demonstrates impressively high confidence in the company’s testing and deployment automation. This confidence enables team members to move faster and be bolder about launching new features.

Klein, Betser & Monroe (2014b) provide a good overview of the rigorous methodology required to make push-on-green possible at Google. The book Continuous Delivery: Reliable Software Releases Through Build, Test, and Deployment Automation by Humble & Farley (2010) is a deep dive into the topic.

Later in this chapter we’ll discuss how these techniques can be applied to third-party software, which is commonly used in most enterprise environments.

##### Eliminating the Approval Committees at Google

In some organizations, there is an approval committee that signs off on a release before it is launched to production. The committee has access to the beta or UAT environment and blesses the release.

Such committees are an impediment to rapid iterations. It is difficult to iterate quickly if the approval committee meets monthly. Ideally, such committees should be eliminated.

Daniel V. Klein gave a talk at LISA 2014 describing how Google is able to launch when the testing dashboard shows all green (all tests passing) rather than having a human approval committee (Klein, Betser & Monroe 2014a).

It is not simply a matter of automating the checks done by the approval committee. Klein recommends a higher level of quality and testing at every step of the process, starting with the methods and behaviors of the developers, all the way through to how the releases are pushed into production.

### 20.2 Minimum Viable Product

The six-step launch process described in the previous chapter is very common— but also extremely risky. It puts all our eggs in one basket. If we aren’t ready by the launch date, we are doomed.

A better method is to first launch early with the absolute minimum number of features, then add features through a series of subsequent launches. As the number of features grows, we can decide if it is time to expose the service to real users. If the deadline arrives, we can launch with what we have, even if the lower-priority features have not yet been added.

Let’s return to our example of launching an automated OS installation system for our Windows workstations. We want the system to install the OS, join the domain, configure settings, install MS Office, remove certain applications, and even do some fancy network configuration such as adjusting NIC priorities if a machine has multiple NICs. Suppose we have two months to create this system.

One way to organize this project is to add all those features to the ready list and then start working on the items. Sadly, by the time the deadline arrives, we aren’t quite done. Worst of all, we’re 80 percent done on all items, which means we’re zero percent able to launch.

A better way to organize our work is to first build a beta system with the absolute minimum features to be a viable OS installation system. This is the minimum viable product (MVP). We launch the MVP into the beta staging area. Then we add new features one at a time. Whenever we complete a feature, we launch the new release to the beta area. When the deadline arrives, we launch with the features that are working in beta.

Because we will be launching to beta so many times, it is important that the end-to-end system from building the pieces from the source files, to testing, to deployment is fully automated. Otherwise, the labor involved will be a burden and will steal time away from feature development.

For example, suppose our MVP is the ability to PXE boot the machine and install the default Microsoft-supplied version of Windows. It won’t be customized in any way. It won’t be attached to our domain. Nevertheless, the part of the process that we have implemented will be fully automated, as will a minimum set of tests. For example, to test the end-to-end process, we’ll use a tool that can act as a remote keyboard. It will start the PXE installation process, test that a user can log in and out, and so on. There may be some manual steps because of the realities of OS installation, but our goal is to eliminate human intervention as much as possible.

Lastly, the process by which the beta test configuration files are promoted from the beta area to the production area is automated, too.

Now we have our minimum viable product. If we had to, we could launch it to production, though it would be fairly anemic. At least it is better than walking around the office with the installation DVD.

Now we add more features. For example, we modify it to automatically join the ActiveDirectory domain. The tests are updated to verify this feature works. When this step is complete, we use the script to promote this version from the beta to the production environment. We repeat this for each feature.

When the deadline arrives, we launch the most recent working version in beta to the production environment. We know it works because we’ve been testing it all along.

While we borrow some techniques from the six-step launch process, the MVP approach gives us a different paradigm for launching services:

- Features drive the launch. We launch when the features are ready, not when a deadline on a calendar says we must launch. In fact, we can launch early if we choose. If we carefully choose the order in which we add features, we could launch earlier for certain segments of our target population. Perhaps we might focus on desktop features first and launch for laptops later.
- We are always ready to launch. Instead of fearing that we won’t be done on time, we are always ready to launch. What gets launched may not have every feature, but a system like this is never actually done. There is always another feature that could be added, configured, or improved. If the project is an ongoing concern with no hard definition of “done,” why use a paradigm that assumes there is a hard-and-fast endpoint?
- Launching is low risk. Because we’ve launched to beta so many times, we get very good at it. The process is automated. We’ve seen plenty of edge cases and added code to detect them, work around errors, and so on. The process gets smoother the more we use it. There is no big, scary “launch day.”
- The difference between beta and production stays small. Since we are adding one or two features at a time, the delta between the beta and production versions stays small. When we do launch, we understand which new feature is being launched. This is less risky, easier to comprehend, and easier to explain than mega-launches with hundreds or thousands of new features being introduced at once. We can concisely communicate what is in each release, which makes our customers better informed and better able to understand our work.

##### Case Study: MVP Applied to Writing Books

This launch methodology works for many kinds of projects, not just IT services. In fact, it is how we developed the third edition of this book. We determined a minimum viable product and worked toward that goal using a ready list. We also had a prioritized list of chapters we wanted to add or rewrite. Once the minimum viable product was complete, we were always in a state where we could hand the manuscript to the publisher for editing, production, and printing.

We wouldn’t want to ship the book in that initial state, as it was largely unchanged from the second edition. However, we could if we were called to do so. From then on we worked on the new features we wanted to see in the new edition. The ones we completed by our publisher’s deadline got included. The ones we didn’t finish . . . well, you’ll never know if there were any, will you?

### 20.3 Rapid Release with Packaged Software

This section deals with applying the rapid release methods with externally produced software—in other words, doing DevOps with shrink-wrapped software.

Our rapid-release examples so far have been with home-grown software or systems that require frequent code updates from within our own organization. If a third-party vendor produces one release per year, building all the rapid release automation seems like a lot of work for very little benefit. However, if the confidence gained from high-quality testing and deployment methods is good for some sources of software, it can benefit all software.

Also note that the industry trend is toward more frequent releases. Microsoft Office used to be updated every three years, with occasional bug fixes in between. Now it is frequently updated, including with security patches that are released on the first Tuesday of each month. Competition with web-based applications is driving packaged software to participate in this trend.

For all these reasons the rapid release techniques are becoming more relevant.

#### 20.3.1 Testing Before Deployment

If we treat packaged software as we did in the rapid release method, each release would be put through a battery of tests before it is deployed.

Testing requires building an environment that replicates the existing system, then performing the upgrade, then conducting various tests. We may tear down and rebuild the environment to perform tests that simulate the variations found in the field.

This is all very much the same as the testing process for home-grown software. The difference is that often organizations do not perform such rigorous testing, or if they do it is not automated. People tend to make rationalizations: We don’t have a lab environment to conduct the tests. Building a lab environment would be too expensive. We don’t have the labor resources to build and rebuild the environment, or to conduct the tests. We deploy to live users and have an excellent backout plan if something goes wrong.

If we do have a testing process, it may be so labor intensive that we might skip occasional releases to reduce our labor cost. While this decision saves labor, it makes the upgrade more risky, as the jump from version n to n + 2 may not be as well tested by the provider. Many organizations do no testing at all and simply assume that the vendor’s testing is sufficient. This is not only risky but also misses the fact that the vendor’s environment cannot possibly include the infrastructure we have built around the vendor’s product.

These excuses and rationalizations are widespread in our industry. Some are justified by the fact that a small shop may not have the budget to do such testing, or that the risk is small enough that a backout plan is sufficient, or that the return on investment (ROI) for automating an infrequent task makes it difficult to justify. But suppose that the company then grows and grows and grows, and we have developed bad habits. At what point in our growth should we change our ways?

As a professional system administrator, it is your responsibility to use resources wisely. On the one hand, it would be irresponsible to set up a fully automated test infrastructure for a inconsequential service that is updated rarely. On the other hand, it would be equally irresponsible to not set up a repeatable testing structure, automated or manual, for the company’s most critical system. As professionals, we must hone our communication skills to make the case for such efforts in terms of risk and reward rather than pure cost.

A written checklist of tests performed manually is better than no tests. A few scripts that make it easier or less error-prone to execute some of those tests is even better. Full automation on the most basic tests, leaving a few manual tests at the end, is an improvement. It is natural for a testing regimen to start out ad hoc, solidify as a checklist, and evolve to be increasingly automated over time. We create an MVP of testing and grow it over time.

##### Case Study: Evolving Email Server Tests

In one case, the system being developed was an email server. The tests that we ran included sending email to test complex email routing rules that were set up. Email was sent inside-to-inside, inside-to-outside, outside-to-inside, to an internal user with an automatic vacation message reply, to mailing lists, and so on.

Originally these test messages were sent manually, followed by a mad dash to log into the various destinations to verify that the messages landed as expected.

Sending the messages was easy to automate. Testing that they reached their destinations was much more difficult to automate. Initially we simply made the manual task easier by using subject lines that made manual verification easier. Each test run was given a number, and one simply had to verify that this test number was seen (manually) in all the desired inboxes. This meant that once the inboxes were open, each in a separate window, the manual verification was easier.

Eventually the delivery verification was also automated.

#### 20.3.2 Time to Deployment Metrics

A key metric to track is the amount of time between when an update is available and when it is in use in production. The longer this time span, the more we are vulnerable to security holes, the longer our customers are exposed to bugs, and the longer the delay for the productivity enhancements that new features bring. If we installed every update immediately, however, we would be the first to discover problems.

We can seek a happy medium between those two extremes. For example, riskaverse organizations might delay the deployment of Microsoft Windows “patch Tuesday” updates by about a week on the assumption that bad patches will be recalled during that time as early adopters report bugs. The more organizations that do this, the fewer early adopters that will exist. An alternative method is to have a testing framework that lets us gain confidence in the updates by testing them ourselves.

Whether your goal is to have new updates deployed as soon as possible, or as soon as possible after a waiting period, recording a history of this gap between availability and use in production is useful so that we know if we are doing better over time.

##### Better Software Upgrades

Upgrading software in large complex systems can be difficult. The book Ship It! A Practical Guide to Successful Software Projects by Richardson, J. R. & Gwaltney, Jr., W. A. (2005) includes many upgrade techniques and software design patterns that can make future upgrades easier.

### 20.4 Cloning the Production Environment

One technique for upgrading a service is to take an ordinary upgrade process and engineer a way to do it frequently.

As will be discussed in Chapter 33, “Server Upgrades,” a common upgrade technique is to clone the hard drive of a system and upgrade the copy. This permits us to revert to the original system if the upgrade process is a failure.

Initially we do this process manually. First the machine is cloned. The clone may require some reconfiguration to be able to run alongside the production environment. We run the tests on the original clone to verify it works. Next we run the upgrade procedure. The tests are now repeated to verify the results are the same.

We’ve been in situations where the service was running on a virtual machine and cloning it was simple. It did, however, require downtime. Luckily the service could be taken down at specific times of the day. This limited our ability to perform the full test to once a day—at least until we realized we could clone the clone. Now we could run the entire process repeatedly from the master clone. The master clone was updated periodically.

In another situation the service was on a physical machine. This made cloning more difficult. However, the tests could be run in a virtual environment. We used a physical-to-virtual (P2V) utility to clone the physical machine.

In both of these situations, the process was very manual but evolved to be more automated over time. The breakthrough was always the point when the clone– modify–startup process was automated and ran in a loop. This automated process left out installing the upgrade, running the tests, and verifying the test results. However, because we had the process running in a loop without manual intervention, we could add these other elements one by one and enhance the system over time. Getting the minimal loop running was the hard part. After that, a wider group of team members could get involved by adding tests and other improvements.

##### Legacy Systems

A legacy system is any system that cannot be upgraded, fixed, or improved because the only way to test such changes is to try them in production. In other words, it is any system with no continuous test and deployment infrastructure. The oldest mainframe application is not a legacy system if we can test changes to it and confidently launch new releases without fear. The hottest new technology is a legacy system if we are stuck with the existing version, unable to confidently upgrade it.

Organizations should seek to eliminate all legacy systems either by literal replacement or by retrofitting them with rapid release techniques.

### 20.5 Example: DNS/DHCP Infrastructure Software

In this example we examine a complex system that already has a long manual testing regimen and evolve it to be automated to gain confidence and agility.

The product is DNS/DHCP appliances with a central management station deployed in a large enterprise that is very nervous about any infrastructure changes. The service is global, with changes and upgrades being applied at the management station and automatically propagating to all the appliances around the world. It is complex, as it interacts with many different systems: appliance to appliance, appliance to ActiveDirectory (many versions), appliance to network infrastructure, appliance to CMDB, CMDB to appliance, and automation to appliance, plus a wide variety of client operating systems that use DHCP and perform dynamic DNS updates.

The vendor produces new software releases frequently, but the organization deploys only one release per year.

#### 20.5.1 The Problem

The policy of upgrading only once per year came about for many reasons:

- Customers are very cautious, perhaps paranoid, about network changes. DNS/DHCP is a fundamental part of the infrastructure. Nearly every service and function relies on DNS/DHCP. Any misstep affects many.
- The IT team has been burned by past upgrade problems in a variety of other areas that turned into highly visible outages. They really want to avoid repeating such problems.
- Testing is expensive. An isolated test lab is used to run a new release through a series of tests before a release can be deployed in the field. The tests are

performed manually. If any problems are found, the vendor produces a patch and the entire testing process starts again. Thus, there is a disincentive to find a bug.
- The organization maintains a tightly controlled network. Each change goes through a Change Review Board (CRB) that has a complex and intimidating approval process, and that requires changes to critical infrastructure to be accepted by leaders across the entire company.
- The company is so large that scheduling a change to significant infrastructure requires booking a slot in the change calendar months in advance.

Sadly, the yearly update cycle creates many problems:

- Security problems are fixed with patches/hotfixes rather than software upgrades, resulting in the site running a version of code that is run at few, if any, other sites. The fear of patches is less than the fear of upgrades.
- Yearly tests are too infrequent to be automated. There is low ROI for automating something done so infrequently, and by the time the next deployment rolls around enough has changed that the automation is obsolete. Any additional tests add to the labor burden of implementing a new release. This creates a disincentive to do more tests in an environment—the opposite of what we want.
- The vendor publishes new releases every few months but most go uninstalled. This means each actual upgrade that the site undertakes includes a large batch of changes. Big batches of changes are more difficult to test, and more risky to deploy.
- Yearly deployments mean the engineers doing the upgrade are less experienced. They haven’t done the process for a year; newer members are usually doing the process for their first time.
- The Change Review Board has a certain amount of turnover each year. Yearly upgrades mean that each change requires re-educating the CRB about the upgrade process and risks from scratch. People are more nervous about a change the first time they see it. Each year half the committee is seeing changes for the first time. Being new, they try to review the entire proposal, which is a lot of work and difficult to do well. Meanwhile, the few who remember the process from the previous year could do a better job of focusing on what’s new.

#### 20.5.2 Desired End-State

The organization would be in a better place if it could benefit from new releases sooner. Incentives should encourage more testing and more frequent upgrades.

The skills of both the CRB members and the engineers should stay fresh through practice.

The ultimate goal is to have high confidence in a release. The company should be able to confidently say, “Yes, this release is ready to be deployed and the deployment will go smoothly.” All employees should know this because of the thorough testing process they’re using.

#### 20.5.3 First Milestone

While this organization would benefit from a fully automated release process, we begin with a few baby steps.

First we get our own house in order. The goal here is to create a test process that proves out a release and deploys it into a beta environment. This process stops one step short of deploying into production. Success in the beta environment will reduce the fears of the CRB and other employees. This will pave the way to getting more frequent upgrades approved.

A minimal test loop is created. This is a slow evolution. A script is written that builds one particular test configuration. Another script runs a set of tests. Eventually a continuous integration tool is used to automate these tests, which can now be run in a loop.

Once this loop is running, new scenarios and tests are added. A library of cloned hosts is developed for use in test cases.

Some tests are still done manually, but this improves over time. What we learn from automating the easy cases prepares us for the more difficult ones. Some tests must be done using physical hardware and require a manual step to kick them off. This improves over time: First we find we can leave the machines in a configured state and require manual intervention only to wipe and reload the machine, which doesn’t need to be done every test cycle.

The test automation tests not only features but also the upgrade process itself. The tests are run on a fresh installation as well as an older installation that has been upgraded to the most recent release. This is especially important if past outages were caused by bad interactions of old and new releases.

All of this automation is hard work and takes many months. However, soon a new release can be put through a battery of tests and deployed into the beta area, with the exception of a few manual steps. Even with those manual steps, we are in a better situation than before.

When customers report bugs, each bug is not only fixed, but the customer is also shown that the automated test framework now tests that situation; thus, if the bug reappears, it won’t make it into production. These are called regression tests. Also, if we openly document our regression tests, it builds confidence with our customers, who can see that their needs are being addressed because those needs are codified in a way that will keep the bug fixed. It creates positive pressure to automate other tests: Management will be inclined to expect that all testing be automated.

Soon all testing and deployment processes are automated. The beta environment is upgraded with every new release. The engineers dream of a day when customers will participate in this process with enthusiasm by not simply submitting a bug report, but also offering a VM snapshot to be part of the regression test.

#### 20.5.4 Second Milestone

The next milestone is to convince the CRB and other stakeholders to permit more frequent updates outside of the beta area. A number of strategies are employed.

First the team enlists allies. They partner with the security group, whose members appreciate the value of security patches being deployed swiftly. The team makes a good impression by demoing the new highly automated testing system, showing the higher degree of confidence that it brings. Then key members of the CRB are recruited one by one to get them on board. They are impressed by the idea that more frequent deployments will reduce the risk associated with the change from high to medium.

Another way to develop confidence in the new methods is to use them in lowrisk situations or places where the burdensome approval process is not required. The company is growing and new networks are added all the time. New networks, before live users are on them, do not require the same CRB approval process. Problems will not be visible to live users.

Confidence in the deployment process improves as it is exercised many times. Soon it is the allies who are requesting the process change. The team leverages these allies and gets permission to begin more frequent deployments. At first this is a pilot project, but soon it is approved and becomes the standard operating procedure.

In the end new releases are reaching users faster and with better results than before.

### 20.6 Launch with Data Migration

Often a launch involves migrating data from the old system to the new system. This may be a traditional SQL database, data files, or application-specific data. If you are launching a new email service, for example, there may be millions of email messages to migrate.

Usually we can’t migrate data while the system is running live. The database has to be frozen, converted, and then used. If you convert the database on Monday and do the upgrade on Wednesday, you will miss any data entered on Tuesday. Such migrations are full of surprises. Here are some anonymized but real examples:

- A company was migrating to new, faster hardware. After waiting 3 months to find a time that everyone could agree to do the migration, the conversion began and users were locked out of the system. Fifteen minutes later, the team discovered they didn’t have a required cable to connect the new storage system and wouldn’t be able to purchase one in the 12-hour maintenance window. It took another 3 months to schedule more downtime. The company lost customers as a result of the delay.
- When a company migrated a database, it broke because the new system rejected records of any users with a space in their first name. Manually massaging these records was taking hours and was error-prone. The conversion was rescheduled.
- A Girl Scouts council’s database was offline for two weeks due to a migration. Some pieces were offline even longer. No one could register for events, leaders couldn’t get rosters, and the entire region’s holiday season was a disaster.
- A university planned a weekend-long outage of its email system to migrate it to a different product. It had been estimated that each user would be migrated in minutes, but once the migration began each user was taking nearly an hour. The incorrect time estimate was based on a simple file copy benchmark. The actual migration used the IMAP protocol to read and write one message at a time, which was significantly slower. The upgrade plan was cancelled. The damage to the IT department’s reputation made it nearly impossible for them to get large projects approved for years.

In summary, most of these problems were related to insufficient resources, processes taking longer than expected, bugs, and general unpreparedness. For any system large enough to be useful, there will always be surprises with data migration. Therefore it is irrational to make plans that assume there won’t be trouble.

One way to fix such problems is to migrate individual users one at a time. However, this requires that the old and new systems can run in parallel. This approach is recommended for the email migration example but won’t work for the other cases described previously. That said, it does have the advantage that it permits a slow migration. Beta users can test the system first, and the most risk-averse users can be migrated last.

##### Email Migration Hacks

Another way to speed up an email migration is to not migrate the messages. Here are some alternatives:

- Permit users to log into the old system if they need to access their old messages. Keep the old system running in a read-only state for a few months.
- For exceptional users who do require migration, charge them a small fee to do the migration. Only a few will ask.
- Create a control panel that lets users activate the migration process themselves.
- Create a control panel that allows users to select whether new messages will go to the old or new system (but not both). At a certain cut-off date, point everyone to the new service. This also gives you a way to monitor adoption rates.

Do practice runs of the conversion ahead of time. Set up a new system and practice the migration there. Delete the result and repeat the process until it can be done without fail. Now you can do the actual launch by freezing the existing system, doing the conversion, and then giving people access to the new system. You’ll not only be confident that the process works, but also have an accurate time estimate for how long it will take to run.

An optimization is to give people access to the practice system for the purpose of testing. Of course, they have to understand that any changes they make will be lost, but it gives them the ability to try their processes and make sure everything works. This is called user acceptance testing (UAT).

Since you are going to be doing the conversion many times, you’ll want to automate it. As in the second example, there may be database records that need to be fixed. Fixing them manually the first time is reasonable, as you are debugging the process. However, such manipulations should be automated by the next practice run. Done right, soon you will be able to do the entire copy–convert–launch by typing one command.

If the process is automated, you can run it a lot. You can, for example, run it daily, fixing problems each day as they arise. People can be given access to the practice system and report bugs. Eventually things will reach a steady state. The next day’s conversion can be the official one. You’ve reduced the risk of failure, as the risk that a new surprise will develop in the last 24 hours is low.

We teach our children that practice makes perfect. Basically that’s what we’re doing here. We practice every day, learning as we go, until we are ready for showtime.

### 20.7 Controlling Self-Updating Software

Many software packages automatically update themselves. Examples include web browsers such as Mozilla’s Firefox and Google Chrome, applications such as Google Earth, most smartphone apps, and many others.

Self-updating software is a blessing, as the upgrades happen automatically without requiring any IT effort. However, it can also be a curse if an upgrade breaks. Suddenly employees can’t use the home-grown corporate payroll application, for example, because the broken HTML it generates doesn’t display on the newest Chrome release.

Because of this, IT departments may choose to tightly control when applications are automatically updated. The ability to control updates is usually considered an enterprise feature. It enables companies to test new releases before they are distributed to the entire company.

At one site, the OS software could be reconfigured to retrieve updates from a custom URL instead of the default URL that pointed to the vendor’s HTTPS server. The vendor provided instructions for mirroring the software repository locally.

The company maintained three mirrors of the repository: test, early, and standard. Once a week the vendor’s repository was mirrored to the test repository. A lab of test machines were configured to point to this repository. The test machines ran a series of automated tests. If the tests succeeded, the test repository was copied to the early repository. This early repository was used by a random 1 percent of all machines in the company, plus by any user who requested it. Users could opt out of being selected for the early group.

If no bugs were reported by the early users, the early repository was copied to the standard repository. Within a few hours the entire company had the latest release. Actually, the way this was implemented was that the copy was automatically done a week later. If a bug was reported, someone would manually halt the copying process. The general case was automated, and the exception required manual intervention.

Over time, the system became more fully automated. As confidence in the system grew, the lag time was reduced.

##### An Honest Job Advertisement

Imagine if job advertisements were completely honest. Most companies advertising for IT workers would state that the job is mostly great except for twice a year when “Hell Month” arrives and everyone scrambles to deploy the new release of some major software system. This month is so full of stress, fear, and blame that it makes you hate your employer, your job, and your life. Oh, and by the way, the software releases are often late, so you can’t predict which month will be Hell Month. As a result, you can’t schedule any kind of vacation. Without time off to relax, stress builds and makes your life even worse.

Sadly, at many companies Hell Month is every month. A company that adopts the DevOps principles is different. A rapid release environment deploys upgrades to production weekly, daily, or more often. It is not a stressful event—it is just another day. There is no fear of an upcoming Hell Month.

Imagine if an auto manufacturer’s employees spent most of their time assembling cars, but when a car actually left the factory it was a fearful, stressful month of hell. It would be unacceptable to run a car company that way. It should be unacceptable to manage technology that way, too.

Adopting DevOps techniques is not just better for the company, it is better for you. Over time more and more companies will adopt these techniques not just because they are better for the bottom line, but because they will find it impossible to recruit and hire technical talent if they do not embrace this approach.

Who would want to work anywhere else?

### 20.8 Summary

In this chapter we discussed many ways to turn a big, risky launch process into many small iterations. With small iterations we benefit from the small batches principle introduced in Chapter 2, “The Small Batches Principle.”

Launching a new service is both risky and important. A service that is created but unlaunched isn’t very useful. A failed or mismanaged launch is an embarrassment that harms a team’s reputation, which robs it of momentum.

Launching home-grown software can be improved by automating the software delivery pipeline: the process of building, testing, and deploying software. Once it is automated, we can repeat the process at will.

How an organization launches software can be thought of as falling into one of four levels: chaotic (ad hoc), periodic (daily, weekly, monthly), continuous (after each change a release is deployed in a test environment), or push-on-green (continuous, to the production environment).

Another small batch technique is to launch a minimum viable product (MVP), followed by many launches, each with new features. This has the benefit of giving operations personnel time to develop their runbook and operational experience.

These techniques work with third-party software as well. A test environment is created to simulate the production environment. Tests are automated and run continuously. There is more confidence in the deployment to the live environment, so we can launch more frequently.

Similar techniques work when converting users to a new service. Rather than converting all users at once, we can convert a certain number of people per day, increasing our velocity as we develop confidence with the process.

Launching in small iterations makes the process more likely to succeed, less risky for the business, and less stressful for the people involved. Who would want to work in an environment that works any other way?

### Exercises

1. Describe the rapid release strategy for launching a new service. 2. What is continuous integration and deployment? 3. Why is it important to perform the most expensive tests last? 4. What is the MVP methodology, and what are its benefits? 5. Pick a project in your environment and determine which of the four launch categorizations it falls into. Which steps would raise it to the next level? 6. Why is it unreasonable to try to achieve push-on-green for all projects? 7. Which would benefit a company most: taking a chaotic launch process and making it periodic, or taking a continuous launch process and making it pushon-green? 8. Pick a project you are involved with and describe how it could be organized using the rapid release/DevOps methodology. 9. Pick a project you are involved with and describe how it could be organized using the MVP methodology. 10. This chapter suggested permitting people to have access to an old email server to access their old messages. If you plan to keep the system running for a month, how long should you announce it will be available?
