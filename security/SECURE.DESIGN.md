# Chapter 6

Source: [Designing.Secure.Software-2021-6.pdf](Designing.Secure.Software-2021-6.pdf)

## Secure Design

Secure design incorporates threats and mitigations while architecture is still inexpensive to change. A later review provides another perspective, but designers remain responsible for making security part of the design. Use [Threats](THREATS.md), [Mitigation](MITIGATION.md), and [Patterns](PATTERNS.md) as foundations, then use [Security Design Reviews](SECURITY.DESIGN.REVIEWS.md) to challenge the result.

![Figure 6-1: Chapter-opening artwork](/.attachments/6-1-Designing.Secure.Software-2021-6.png)

*Figure 6-1: Chapter-opening artwork*
<!-- Source: Designing.Secure.Software-2021-6.pdf, PDF page 1; generated number for uncaptioned artwork. -->

The guidance below follows the source's section order, with a practical document-service example added for this repository.

### Integrating Security in Design

Write *design documents* that explain both the *functional description*—what users and other systems observe—and the *technical specification*—how components implement that behavior. Include security decisions in those descriptions. A statement such as “the service stores files securely” is too vague to guide implementation or review.

For example, state which identity may upload a document, where bytes are stored, how download authorization works, and what happens when a dependency fails. Early documentation exposes architectural choices that would be expensive to reverse after implementation.

#### Make Design Assumptions Explicit

Record data sensitivity, expected attackers, performance goals, compatibility constraints, staffing and budget limits, and expected future uses. Shared organizational assumptions can live in a common document, but deviations must be explicit.

A prototype using synthetic records has different confidentiality needs from a production service handling customer documents. Record that distinction so the prototype is not repurposed under an unstated assumption. Treat “only trusted staff can reach this endpoint” as a condition to verify, not an intrinsic property of the code.

#### Define the Scope

Identify the components, interfaces, deployments, and operations covered by the design. List dependencies outside that scope with their owners and required guarantees. Excluding database administration from a feature design does not eliminate responsibility for backups: assign that responsibility explicitly.

For incremental work, document the changed components and the assumptions at their boundaries. Expand the scope when changes alter those assumptions—for example, when a previously internal export becomes available to customers.

#### Set Security Requirements

Describe outcomes before prescribing mechanisms. Confidentiality, integrity, and availability help identify what must be protected. Requirements should be achievable and testable, with unusually strong or weak requirements explained.

Repository example requirements for a document service:

- A user can retrieve only documents belonging to an authorized tenant.
- An upload cannot cause server-side code execution.
- Deleted documents become inaccessible immediately and expire from retained copies according to a documented schedule.
- A failed authorization dependency cannot grant access.
- Recovery objectives specify acceptable data loss and restoration time; backup restoration is exercised.

The example values and retention periods must be chosen for the actual service. A technology name alone is not a security requirement.

#### Threat Modeling and Trust Boundaries

A trust boundary marks a change in authority or trust assumptions. It can exist between browser and API, between tenants in one database, or between a public service and an internal worker. “Internal” does not imply that every caller may perform every operation.

Map assets, actors, entry points, data flows, and boundaries. For each crossing, ask who authenticates the caller, which authorization rule applies, which inputs remain untrusted, and what failure means. Use the four questions—scope, threats, responses, and evaluation—to keep the analysis focused. [OWASP Threat Modeling](https://cheatsheetseries.owasp.org/cheatsheets/Threat_Modeling_Cheat_Sheet.html) provides a complementary workflow.

The source's *essential threat model* distinguishes unavoidable risk from risk introduced by a design choice. A service that must process private data has inherent exposure; an unnecessary cache creates another copy and another access path. Consider eliminating a data flow before adding controls around it.

For example, the source contrasts copying an entire customer directory to sales devices with giving each salesperson only a regional subset. The smaller dataset reduces the consequence of a lost device. Similar reasoning favors giving a document-processing worker access to one assigned object rather than the entire storage account.

### Building in Mitigations

#### Design Interfaces

Specify each interface's input types, authentication, authorization, validation, limits, and error behavior. Define which side enforces each obligation. If a dependency's guarantees are unknown, document the uncertainty and treat its outputs defensively.

Apply least privilege to component identities and deny access when required checks cannot complete. For a document service, the download handler should derive tenant identity from the authenticated session and verify access to the selected object. A client-supplied tenant field is data, not proof of entitlement.

A wrapper can compensate for some legacy weaknesses, such as encrypting data before passing it to storage. Describe what the wrapper protects and what it cannot protect; key custody, metadata exposure, and integrity still need consideration. Prefer established security components over custom mechanisms.

#### Design Data Handling

Document the lifecycle of sensitive data: collection, validation, processing, storage, logging, sharing, backup, and deletion. Minimize the number of copies and parties with access. An opaque identifier in an operational log can permit investigation without copying customer details into every log sink.

Separate public data from confidential data, but consider combinations: a public address plus a person's current location can reveal private activity. Avoid retaining personal data without a purpose. Include caches, replicas, and backups when describing deletion, and ensure a restore cannot silently resurrect records that should remain deleted.

### Integrating Privacy into Design

Translate privacy commitments into technical constraints on collection, purpose, access, retention, and sharing. Identify who can clarify ambiguous policy requirements. When a feature introduces new data use or sharing, obtain the appropriate policy decision and record it in the design.

For example, if support staff need to inspect a document only during a customer-authorized investigation, build time-limited access and an audit trail into that workflow. Do not give every support account permanent access to all documents. Record which identity accessed which object and why, without copying its sensitive contents into the event.

A retention promise should have an enforceable deletion process with observable failures. A promise against sharing should be reflected in storage policies and approval paths. Privacy is part of the behavior the system must implement, not only text displayed to users.

### Planning for the Full Software Life Cycle

Plan for deployment, maintenance, credential rotation, dependency upgrades, migration, incident response, and retirement. Record who owns these tasks. A service may stop accepting new uploads while continuing to support exports or required retention; design those states explicitly.

Retirement includes removing endpoints, revoking identities and keys, expiring stored copies, and preserving only the records that still have an approved purpose. A design that assumes the service will run forever usually leaves these responsibilities unclear.

### Making Trade-offs

Compare threat consequences with implementation cost, usability, performance, and maintenance complexity. Ask how much data can be affected and how long service can be unavailable. State residual risk and the assumptions that make accepting it reasonable.

For the document service, short-lived download links reduce the duration of unintended access, but anyone holding a valid bearer link may be able to use it until it expires. If immediate revocation is required, that architecture may need an authorization check on each download. Record the trade-off rather than describing both designs as equally secure.

Use monitoring to observe important abuse paths, and identify triggers for revisiting decisions: a new tenant model, more sensitive data, a public integration, or a changed deployment environment.

### Design Simplicity

Prefer a small number of clearly enforced policies to scattered exceptions. A shared authorization component reduces inconsistent checks only if every access path actually uses it. Background jobs, administrative tools, direct storage access, and alternate protocols can bypass an otherwise sound boundary.

Simplicity must make security easier to reason about. Adding overlapping controls without clear responsibilities can create more failure modes. Review the complete path from an untrusted entry point to a valuable asset.

### Practical Design Record and Review Handoff

Capture the following before requesting review:

1. Purpose, scope, assumptions, dependencies, and owners.
2. Assets and measurable confidentiality, integrity, and availability requirements.
3. Component and data-flow diagrams with trust boundaries.
4. Threat scenarios, mitigations, and their enforcement points.
5. Data lifecycle, privacy constraints, and operational responsibilities.
6. Alternatives considered, accepted risks, and conditions requiring reassessment.
7. Verification plans, including denial paths and recovery exercises.

This is a repository handoff checklist, not a table extracted from the PDF. Continue with [Security Design Reviews](SECURITY.DESIGN.REVIEWS.md) for assessment and finding resolution, [Untrusted Input](UNTRUSTED.INPUT.md) for boundary validation, and [OWASP Threat Modeling](https://cheatsheetseries.owasp.org/cheatsheets/Threat_Modeling_Cheat_Sheet.html) for further analysis guidance.
