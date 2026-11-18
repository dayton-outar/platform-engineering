# Chapter 7

Source: [Designing.Secure.Software-2021-7.pdf](Designing.Secure.Software-2021-7.pdf)

## Security Design Reviews

A *security design review (SDR)* examines whether a proposed architecture addresses its important threats. It helps find missing protections before they become expensive implementation changes. It also records the security properties on which later implementation and testing depend. A review cannot prove that code faithfully implements a design; combine it with [Security Testing](SECURITY.TESTING.md).

![Figure 7-1: Chapter-opening artwork](/.attachments/7-1-Designing.Secure.Software-2021-7.png)

*Figure 7-1: Chapter-opening artwork*
<!-- Source: Designing.Secure.Software-2021-7.pdf, PDF page 1; generated number for uncaptioned artwork. -->

### Understanding the Review

#### When to Conduct an SDR

Review a reasonably stable design before it is finalized. Complex or high-risk work also benefits from an early architectural discussion, while major alternatives are still affordable. Keep explicit security review time separate from functional review so that feature discussions do not consume it.

The designer owns the design; the reviewer contributes analysis and an independent perspective. Agree on review timing and release expectations early. A surprise review immediately before launch creates pressure to treat security as an obstacle rather than an engineering responsibility.

#### Documentation Is Essential

Collect the design revision, scope, requirements, assumptions, dependencies, data flows, and previous assessments. Missing documentation is a finding to clarify, not evidence that the missing component is secure. Even a small system benefits from a diagram and a written decision record. See [Secure Design](SECURE.DESIGN.md) for what to include.

### The SDR Process

#### 1. Study

Read the design first to understand its purpose, then again with threats in mind. Note valuable assets, exposed interfaces, and unfamiliar dependencies. Gather enough domain context to avoid proposing a protection that defeats the feature's purpose. Save questions and potential issues without treating every suspicion as a confirmed vulnerability.

#### 2. Inquire

Clarify assumptions and omissions with the people who know the design. Ask who owns storage, backup, key rotation, and external integrations. Ask what the team worries about most and why. Correct shared documentation when an answer affects future implementers; keep narrow reviewer questions in working notes where appropriate.

For example, “documents are private” needs clarification: private to one user, a tenant, support staff, or everyone possessing a link? Different answers imply different authorization models.

#### 3. Identify

Focus analysis where exposed inputs can reach important assets. Interfaces, storage, communications, trust boundaries, and privileged operations deserve particular attention. Retain a whole-system view so that an apparently minor administrative route does not escape scrutiny.

Trace at least one representative flow end to end. For a file download, follow the authenticated identity, object identifier, tenant check, storage access, response, and audit event. Then ask whether alternate routes can skip a check.

#### 4. Collaborate

Discuss concrete threat scenarios and alternative mitigations with the designers. Explain consequences rather than simply declaring the design insecure. Consider cost, usability, and operations as well as protection. Record agreed changes and unresolved choices.

Design ownership and approval authority depend on the organization. Identify who may accept residual risk and who decides release-blocking disagreements. A reviewer's recommendation is useful only if the responsible people understand the risk and the proposed response.

#### 5. Write

Produce an *assessment report* identifying the reviewed revision, scope, overall assessment, significant findings, recommendations, and unresolved issues. Explain what is secure as well as what needs work. A bare “looks good” loses the assumptions that made the assessment valid.

The source suggests *Must*, *Ought*, and *Should* to distinguish mandatory, strongly recommended, and optional changes. Define their meaning for the project and include consequence and likelihood reasoning. Do not confuse a priority label with a universal severity score.

#### 6. Follow Up

Verify that agreed design changes actually appear in the updated design. Reassess affected flows when a fix changes boundaries or responsibilities. Link implementation and test evidence where available; design acceptance and implementation verification are separate milestones. Close the review only after required changes are verified or explicitly accepted by the appropriate authority.

Track the review and its findings in the project's normal work system. “A fix is planned” and “a fix has been verified” must remain distinct states.

### Assessing Design Security

#### Use the Four Questions

1. What are we working on? Establish purpose, scope, assets, and legitimate behavior.
2. What can go wrong? Identify abuse, failures, and assumptions that could become false.
3. What are we going to do about it? Match threats to actual enforcement points and mitigations.
4. Did we do a good job? Evaluate coverage, residual risk, and evidence.

These questions connect design review with threat modeling. The [OWASP Threat Modeling Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Threat_Modeling_Cheat_Sheet.html) describes this iterative approach.

For example, an unnecessary health field in an employee system creates exposure without supporting its purpose. Removing the field may be better than adding protections around it. When the field is necessary, identify authorized users, retention, and safeguards instead.

#### Where to Dig: Practical Checklist

This repository checklist supports analysis; completing it does not prove that all threats have been found.

- Scope: Are owners, assumptions, dependencies, and excluded responsibilities explicit?
- Assets: Which data or operations have the largest consequences if disclosed, changed, or unavailable?
- Identity: How are users, services, administrators, and tenants distinguished?
- Authorization: Is access checked for each object and operation, including alternate routes and jobs?
- Boundaries: Which inputs cross a trust boundary, and where are validation and limits enforced?
- Interpreters: Can data become SQL, shell commands, executable templates, or runtime objects?
- Data lifecycle: Are collection, sharing, logs, backups, retention, and deletion covered?
- Failure: Do timeouts, unavailable policy services, and recovery paths preserve security requirements?
- Operations: Who rotates credentials, patches dependencies, monitors abuse, and restores data?
- Evidence: Does each important mitigation have a proposed check and a responsible owner?

Use [Injection Attacks](INJECTION.ATTACKS.md), [Information Leaks](INFORMATION.LEAKS.md), and [CSRF](CROSS.SITE.REQUEST.FORGERY.ATTACKS.md) to make web-specific checklist questions concrete.

#### Privacy Reviews

Map data collection, use, storage, and sharing to the applicable policy requirements. Involve the responsible privacy specialists when interpretation is outside the reviewer's expertise. Record approvals and constraints rather than silently assuming that access permission permits any use of the data.

#### Reviewing Updates

Review the design delta against the last assessed revision. Changes to interfaces, privileges, data sensitivity, or deployment assumptions can invalidate earlier conclusions. Update the assessment's version and findings even if the outcome is that no new mitigation is required. A small code change can have a large security effect.

### Managing Disagreement

#### Communicate Tactfully

Describe the risky behavior and its consequences without attributing fault. Offer alternatives and useful documentation improvements. Ask how extensive feedback should be delivered, and prioritize material issues. Poor or incomplete documentation may reflect delivery pressure; seek the missing information while making the shared record better.

#### A Difficult Review: Practical Lessons

The source's case study shows how a late review, outdated documents, and a stressed team can create conflict. Its useful lessons are to understand the team's constraints, find informed participants, distinguish advance questions from meeting topics, and confirm the resulting changes. These are lessons from the source author's experience, not claims about this repository's maintainers.

#### Escalate Material Disagreements

For a declined recommendation, document the proposed change, rationale, consequence, and decision owner. Escalate unresolved major risks through the agreed authority. Capture both positions and shared assumptions, plus the final decision and conditions for revisiting it. Do not silently mark a disputed finding resolved.

### Tracking Findings: Worked Example

The following is a repository example, not a source table.

*Table 7-1: Example design-review finding record*

| Field | Example |
| --- | --- |
| ID and design revision | SDR-007; document service revision 3 |
| Observation | The download handler checks login but trusts the supplied document ID without a tenant ownership check. |
| Threat and impact | A tenant can retrieve another tenant's confidential document if its identifier becomes known. |
| Priority | Must: cross-tenant disclosure violates a stated requirement. |
| Recommended outcome | Verify tenant authorization before storage access on every download route. |
| Owner and target | API owner; before release of the download feature. |
| Status | Open; proposed change recorded, verification pending. |
| Acceptance evidence | Updated flow and policy; a test denying tenant A access to tenant B's document, alongside a successful authorized download. |
| Residual risk and decision | Identifier secrecy is not an accepted substitute for authorization; no exception approved. |

Preserve the original finding when recording its resolution. Add the verified revision, evidence, reviewer, and date. Accepted risks need an accountable decision, rationale, and review trigger; they should not disappear from the report.

### Practice

Choose a modest design or a bounded component. Produce a diagram, apply the four questions, write an assessment, and follow one finding through resolution. A self-review is useful when another reviewer is unavailable, although another perspective can reveal assumptions the designer misses.

Use [Secure Design](SECURE.DESIGN.md) for preparation and [OWASP Threat Modeling](https://cheatsheetseries.owasp.org/cheatsheets/Threat_Modeling_Cheat_Sheet.html) for further guidance. Repeated reviews build familiarity with both failure patterns and effective design alternatives.
