# Chapter 22

## Disaster Recovery and Data Integrity

Source: [The.Practice.of.System.and.Network.Administration-Vol.I-2017-22.pdf](./The.Practice.of.System.and.Network.Administration-Vol.I-2017-22.pdf), PDF pages 1–9.

A disaster is a catastrophic event that causes a massive outage affecting an entire building or site. A disaster can be anything from a natural disaster, such as an earthquake, to the more common problem of stray backhoes cutting your cables by accident. A disaster is anything that has a significant impact on your company’s ability to do business.

A disaster-recovery (DR) plan lists which disasters could hit the company and includes a plan for responding to those disasters. Disaster-recovery planning includes implementing ways to mitigate potential disasters and making preparations to enable quick restoration of key services. The plan identifies what those key services are and specifies how quickly they need to be restored.

All sites need to do some level of DR planning. DR planners must consider what happens if something catastrophic occurs at any one of their organization’s sites and how they can recover from it. This chapter concentrates on the electronic data aspects of DR. However, this part of the plan should be built as part of a larger program to meet the company’s legal and financial obligations. A number of books are dedicated to disaster-recovery planning, including these resources:

- The Business Continuity Management Desk Reference: Guide to Business Continuity Planning, Crisis Management & IT Disaster Recovery by Watters (2010)
- Disaster Recovery Planning: Getting to Business-Savvy Business Continuity, 4th ed., by Toigo (2003)
- A Practical Guide to Risk Management by Coleman (2011)

Building a disaster-recovery plan involves understanding both the risks that your site faces and your company’s legal and fiduciary responsibilities. From this basis, you can begin your preparations. This chapter describes what is involved in building a plan for your site.

As with any project, building a disaster-recovery plan starts with understanding the requirements: determining which disasters could afflict your site, the likelihood that those disasters will strike, the cost to your company if they do strike, and how quickly the various parts of your business need to be revived. Once you and your management understand what can happen, you can get a budget allocated for the project and start looking at how to meet and, preferably, beat those requirements.

### 22.1 Risk Analysis

The first step in building a disaster-recovery plan is to perform a risk analysis. A risk analysis involves determining which disasters the company is at risk of experiencing and what the chances are of those disasters occurring. The analyst also determines the likely cost to the company for each disaster should it occur. The company then uses this information to decide approximately how much money it is reasonable to spend on trying to mitigate the effects of each type of disaster.

Since risk management involves specialized skills that most employees don’t use daily, it can be wise to hire external consultants to perform the risk analysis. A large company may hire specialists to produce a risk report, while having an in-house person be responsible for risk management.

##### Lack of Planning Can Cause Risk Taking

A computer equipment manufacturer had a fire in its facility in the west of Ireland. Knowing that the building’s fire protection system was inadequate, several staff members went to the datacenter and started throwing equipment out the window because the equipment had a better chance of surviving the fall than the fire. Other staff members then carried the equipment up the hill to their neighboring building. The staff members in the burning building left when they judged that the fire hazard was too great.

All the equipment that they had thrown out the window actually survived, and the facility was operational again in record time. However, the lack of a disaster-recovery plan and adequate protection systems resulted in staff members’ risking their lives. Fortunately, no one was badly injured in this incident. Nevertheless, their actions were in breach of fire safety codes and extremely risky because no one there was qualified to judge when the fire had become too hazardous.

The approximate budget for risk mitigation is determined by the formula (D −M) × R, where D is the probable cost of disaster, M is the probable cost after mitigation, and R is the risk of disaster.

For example, if a company’s premises has 1 chance in 1 million of being affected by flooding, and if a flood would cost the company $10 million, the budget for mitigating the effects of the flood would be in the range of $10. In other words, it’s not even worth stocking up on sandbags in preparation for a flood. In contrast, if a company has 1 chance in 3,000 of being within 10 miles of the epicenter of an earthquake measuring 5.0 on the Richter scale, which would cause a loss of $60 million, the budget for reducing or preventing that damage would be in the $20,000 range.

A simpler, smaller-scale example is a large site that has a single point of failure, in that all LANs are tied together by one large router. If it died, it would take one day to repair, and there is a 70 percent chance that failure will occur once every 24 months. The outage would cause 1,000 people to be unable to work for a day. The company estimates the loss of productivity to be $68,000. The SAs are given a budget of $23,800 to build router redundancy. The SAs also need to investigate the cost of reducing the outage time to 4 hours—for example, by expanding contracts with vendor support. If that costs a reasonable amount, it further reduces the amount the company would lose and, therefore, the amount it should spend on full redundancy.

This example is somewhat simplified. Each disaster can occur to different degrees with different likelihoods and have a wide range of cost implications. Damage prevention for one level of a particular disaster will probably have mitigating effects on the amount of damage sustained at a higher level of the same disaster. All this complexity is taken into account by a professional risk analyst when he or she recommends a budget for the various types of disaster preparedness.

### 22.2 Legal Obligations

Beyond the basic cost to the company, additional considerations need to be taken into account as part of the DR planning process. Commercial companies have legal obligations to their vendors, customers, and shareholders in terms of meeting contract demands. Public companies have to abide by the laws of the stock markets on which they are traded. Some industries, such as banking, are governed by local laws relating to business continuity. Universities have contractual obligations to their students. Building codes and work-safety regulations also must be followed.

The legal department should be able to elaborate on these obligations. Typically, they are of the form “The company must be able to resume shipping product within one week” or “The company can delay reporting quarterly results by at most three days under these circumstances.” Those obligations translate into requirements for the DR plan. They define how quickly various pieces of the physical and electronic infrastructure must be restored to working order. Restoring individual parts of the company to working order before the entire infrastructure is operational requires an in-depth understanding of which pieces of infrastructure those parts rely on and a detailed plan of how to get them working. Meeting the time commitments also requires an understanding of how long restoring those components will take. We look at that issue further later in this chapter.

### 22.3 Damage Limitation

Damage limitation is about reducing the cost of the disasters. Most damagelimitation efforts involve additional cost to the company and are subject to the cost/benefit analysis that is performed by the risk analysts. Some damage limitation can come at little or no cost to the company through advance planning and good processes.

For example, in an area prone to minor flooding, placing critical services above ground level may not significantly increase construction and move-in costs, but might prevent flood disasters in the future. Choosing rack-mountable equipment and reasonably sturdy racks to bolt it into, rather than putting equipment on shelves, can significantly reduce the impact of a minor earthquake for little or no extra cost. Putting lightning rods on a building in an area that is prone to lightning storms is also a cheap way of limiting damage. These solutions are particularly economical because they fix the problem once, rather than requiring a recurring cost.

Limiting the damage caused by a major disaster is more costly and always should be subject to a cost/benefit analysis. For example, a datacenter could be built in an underground military-style bunker to protect against tornados and bombs. In an earthquake zone, expensive mechanisms allow racks to move independently in a constrained manner to reduce the risk of computer backplanes’ shearing—the major issue with rigidly fixed racks during a strong earthquake. These mechanisms for limiting damage can be so costly that only the largest companies are likely to be able to justify implementing them.

Most damage-limitation mechanisms fall somewhere between almost free and outlandishly expensive. Fire-prevention systems typically fall into the latter category. It is wise to consider implementing a fire-protection system that is designed to limit damage to equipment in the datacenter when activated. Local laws and human safety concerns limit what is possible in this area, but popular systems currently include gas-based systems that remove heat or oxygen and water-based systems with early-warning mechanisms that permit an operator to detect and resolve a problem before there is a fire, such as a disk or power supply overheating. Systems for detecting moisture under raised datacenter floors or in rarely visited UPS or generator rooms are also moderately priced damage-limitation mechanisms.

Another area that often merits attention is loss of power. Short power outages, spikes, or brownouts can be handled by a UPS; longer interruptions will require a generator. The more equipment on protected power and the longer you need to be able to run that equipment, the more it will cost. Typically the most essential equipment should be protected against power loss—for example, freezers at a biotech company and call centers at a customer service company.

### 22.4 Preparation

Even with a reasonable amount of damage control in place, your organization may still experience a disaster. Part of your disaster planning must include preparation for this eventuality. Being prepared for a disaster means being able to restore the essential systems to working order in a timely manner, as defined by your legal, ethical, and fiduciary obligations.

Restoring services after a disaster can require rebuilding the necessary data and services on new equipment if the old equipment is not operational. Thus, you need to arrange a source of replacement hardware in advance from companies that provide this service. You also need to have another site to which this equipment can be sent if the primary site cannot be used because of safety reasons, lack of power, or lack of connectivity. Make sure that the company providing the standby equipment knows where to send it in an emergency. Make sure that you get turnaround time commitments from the provider and that you know which hardware the company will be able to provide on short notice. Don’t forget to take the turnaround time on this equipment into account when calculating how long the entire process will take. If a disaster is large enough to require the company’s services, chances are that company will have other customers that also are affected. Find out how the company plans to handle the situation in which both you and your neighbor have the right to the only 50 KW generator the vendor has in your metro area.

Once you have the machines, you need to recreate your system. Typically, you first rebuild the system and then restore the data. This requires that you have backups of the data stored off-site—usually at a commercial storage-and-retrieval service. You also need to be able to easily identify which tapes are required for restoring the essential services. This basic part of the preparation plan relies on infrastructure that your site should have already put in place. An ongoing part of disaster preparation involves retrieving tapes from the off-site storage company on a regular basis to see how long it takes. This time is subtracted from the total amount of time available to completely restore the relevant systems to working order. If it takes too long to get the tapes, it may be impossible to complete the rebuild on time. For more on these issues, see Chapter 44, “Backup and Restore,” particularly Section 44.7.2.

A site usually will need to have important documents archived at a document repository for safekeeping. Such repositories specialize in DR scenarios. If your company uses a repository, you may want to consider using it to house the data tapes as well.

Remember that you may need power, telephone, and network connectivity as part of restoring services. Work with the facilities group on these aspects of DR planning. It may be advisable to arrange for an emergency office location to house critical functions as part of the disaster plan.

##### Good Preparation for an Emergency Facility

A company in California provided call center services for its clients, which were predominantly large financial institutions. The company had a wellrehearsed procedure to execute in case of a disaster that affected the call center building. The company had external outlets for providing power and call center phone services, spare cables and equipment were maintained in reserve, and items such as tents, and folding tables and chairs, were kept in case they were needed.

When a strong earthquake struck in 1991, the call center was rapidly relocated outside and became operational again within minutes. Not long after it was set up, it received lots of calls from the company’s customers in New York, which wanted to make sure that services were still available if they required them. The call center staff calmly reassured customers that all services were operating normally. Customers had no idea that they were talking to someone who was sitting on a chair in the grass outside the building. The call center had to remain outside for several days until the building was certified safe—but it remained operational the entire time.

The plan to relocate the call center outside in the case of emergency worked well because in California the most likely emergency was an earthquake and the weather was likely to be dry, at least long enough for the tents to be put up. The company had prepared well for its most likely disaster scenario.

### 22.5 Data Integrity

Data integrity means ensuring that data is not altered by external sources. Data can be corrupted maliciously by viruses or individuals. It can also be corrupted inadvertently by individuals, bugs in programs, and undetected hardware malfunctions. For important data, consider ways to ensure integrity as part of day-today operations or the backup or archival process. For example, data that should not change can be checked against a read-only checksum of the data. Databases that should experience small changes or have only data added should be checked for unexpectedly large changes or deletions. Examples include source code control systems and databases of gene sequences. Exploit your knowledge of the data on your systems to automate integrity checking.

Disaster planning also involves ensuring that a complete and correct copy of the corporate data can be produced and restored to the systems. For disasterrecovery purposes, a company will need a recent, coherent copy of the data with all databases in sync. Data integrity meshes well with disaster recovery.

Industrial espionage and theft of intellectual property are not uncommon, and a company may find itself needing to fight for its intellectual property rights in a court of law. The ability to accurately restore data as it existed on a certain date can also be used to prove ownership of intellectual property. To be used as evidence, the date of the information retrieved must be accurately known, and the data must be in a consistent state. For both disaster-recovery purposes and use of the data as evidence in a court, the SAs need to ensure that the data has not been tampered with.

It is important to make sure that the implementers put in place the dataintegrity mechanisms that the system designers recommend. It is inadvisable to wait for corruption to occur before recognizing the value of these systems.

### 22.6 Redundant Sites

The ultimate preparation for a disaster is to have fully redundant versions of everything that can take over when the primary version fails. In other words, an organization may have a redundant site with redundant systems at a location that will not be affected by the same disaster.

Although this sounds expensive, in large companies, especially banks, having a redundant site is a minimum standard. In fact, large companies have stopped using the term disaster recovery and instead use the term contingency planning or business continuity planning.

There are a few different design patterns for this sort of redundancy. Moreover, different services may follow different design patterns, as described in Chapter 18, “Service Resiliency and Performance Patterns.”

To reduce expense, some companies may decide to use a second site as an alternative location for rebuilding services, rather than maintaining live redundant equipment at site. If the company has a contract for an emergency supply of equipment, that equipment could be sent to the alternative datacenter site. If the site that was affected by the disaster is badly damaged, this may be the fastest way to have the services up and running. Another option is to designate some services as less critical and to use the equipment from those services to rebuild the critical services. Sometimes, you are lucky enough to have a design that compartmentalizes various pieces, making it easy to design a redundant site.

### 22.7 Security Disasters

A growing concern is security disasters. Someone breaks into the corporate web site and changes the logo to something obscene. Someone steals the database of credit card numbers from an e-commerce site. A virus deletes all the files it can access. Unlike with natural disasters, no physical harm occurs with a security disaster, and the attack may not be from a physically local phenomenon.

A similar risk analysis can be performed to determine the kinds of measures required to protect data. Architecture decisions, for example, always have a risk component. One can manage this risk in many ways—for example, by building barriers around the system or by monitoring the system so that it can be shut down quickly in the event of an attack.

Sites often purchase large, canned systems without asking for an explanation of the security risks of such systems. Although no system is perfectly secure, a vendor should be able to explain the product’s security structure, the risk factors, and how recovery would occur in the event of data loss.

### 22.8 Media Relations

When a disaster occurs, the media will probably want to know what happened, what effect it is having on the company, and when services will be restored. Sadly, the answer to all three questions is usually “We aren’t sure.” This can be the worst answer you can give a reporter. Handling the media badly during a disaster can cause bigger problems than the original disaster.

We have two simple recommendations on this topic. First, have a public relations (PR) firm on retainer so that you aren’t trying to hire one as the disaster is unfolding. Some PR firms specialize in disaster management, and some are proficient at handling security-related disasters. Second, plan ahead of time how you will deal with the media. This plan should include who is allowed to talk to the media, which kinds of things will and will not be said, and what the chain of command is if the designated decision makers aren’t available. Anyone who talks to the media should receive training from the PR firm.

Note that these recommendations all have one thing in common: They require planning ahead of time. Never be in a disaster without a media plan. Don’t try to write one during a disaster.

### 22.9 Summary

The most important aspect of disaster planning is understanding which services are the most critical to the business and what the time constraints are for restoring those services. You must know which disasters are likely to happen and how costly they would be before you can complete a risk analysis and determine the company’s budget for limiting the damage.

A disaster plan should be built with consideration of those criteria. It should account for the time to get new equipment, retrieve the off-site backups, and rebuild the critical systems from scratch. These steps require advance planning for getting the correct equipment and being able to quickly determine which backup tapes are needed for rebuilding the critical systems.

Look for simple ways to limit damage, as well as more complex and expensive ways. Preparations that are automatic and become part of the infrastructure are most effective. Fire containment, water detection, earthquake bracing, and proper rack-mount equipment fall into this category. You also must prepare a plan for a team of people to execute in case of emergency. Simple plans are often the most effective. The team members must be familiar with their individual roles and should practice disaster recovery a few times a year.

Full redundancy, including a redundant site, is beyond the budget of most companies. If a company has a second datacenter site, however, there are ways to incorporate it into the disaster plan at reasonable expense.

### Exercises

1. What is disaster-recovery planning? 2. What are the key elements of a disaster-recovery plan? 3. How much should be spent to mitigate a potential disaster? 4. Which business units in your company would need to be up and running first after a disaster, and how quickly would they need to be operational? 5. Which commitments does your company have to its customers, and how do those commitments influence your disaster planning? 6. Which disasters are most likely to hit each of your sites? How big an area might each disaster affect, and how many of your company’s buildings could be affected? 7. What would the cost be to your company if a moderate disaster hit one of its locations? 8. Which forms of disaster limitation do you have in place now? 9. Which forms of disaster limitation would you like to implement? How much would each of them cost?
