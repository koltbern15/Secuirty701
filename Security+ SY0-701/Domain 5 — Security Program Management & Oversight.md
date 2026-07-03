> **Domain 5 — Security Program Management & Oversight**
> **Exam weight:** 20% (~18 questions on a 90-question form) — the second-heaviest domain, behind [[Domain 4 — Security Operations]] (28%).
> **What it covers:** The business and governance layer of security — guidelines/policies/standards/procedures, the full risk-management lifecycle (identification, assessment, quantitative/qualitative analysis, the risk register, risk strategies, BIA), third-party/vendor risk and agreement types, compliance and privacy obligations, audits and penetration testing, and the security-awareness program.
> **Strategic note:** Governance, risk, third-party, compliance, audits, awareness. Memorize the risk formulas (**SLE = AV × EF**, **ALE = SLE × ARO**) and the BIA metrics (**RTO, RPO, MTTR, MTBF**) cold — they are guaranteed points and the only place this exam asks you to do arithmetic. As a banker living FFIEC exams, third-party risk, and recertification campaigns, much of this domain is your day job dressed in CompTIA vocabulary — the trap is answering from how *your* shop does it rather than how the exam defines it.

## Objective map

- **5.1** Security governance — guidelines; policies; standards; procedures; external considerations; monitoring & revision; governance structures; roles & responsibilities.
- **5.2** Risk management process — identification; assessment types; qualitative vs. quantitative analysis (SLE/ALE/ARO/EF); risk register (KRIs, owners, thresholds); tolerance vs. appetite; strategies (transfer/accept/avoid/mitigate); reporting; business impact analysis (RTO/RPO/MTTR/MTBF).
- **5.3** Third-party risk — vendor assessment; vendor selection; agreement types (SLA/MOA/MOU/MSA/SOW/NDA/BPA); vendor monitoring; questionnaires; rules of engagement.
- **5.4** Compliance — reporting; consequences of non-compliance; monitoring; privacy (data subject, controller vs. processor, retention, right to be forgotten).
- **5.5** Audits & assessments — attestation; internal; external; penetration testing (types, environment knowledge, reconnaissance).
- **5.6** Security awareness — phishing; anomalous behavior; user guidance & training; reporting & monitoring; development; execution.

---

## 5.1 Security governance

**Governance** is the system of direction and oversight that decides *how* security gets done in an organization — who sets the rules, what those rules are, and how they're enforced and updated. SY0-701 organizes governance into a four-level document hierarchy that descends from broad intent to concrete steps. Know the order and the bindingness of each level, because CompTIA tests "which document is this?" relentlessly.

### The document hierarchy

| Level | What it is | Binding? | Test it by |
|---|---|---|---|
| **Policy** | High-level statement of management's intent and direction. Sets the *what* and *why*. | Mandatory | "Management says we *will* protect X." |
| **Standard** | Specific, mandatory requirements that make a policy enforceable — exact configurations, versions, lengths. | Mandatory | "Passwords *must* be 14 characters." |
| **Procedure** | Step-by-step instructions to accomplish a task consistently. The *how*. | Mandatory | A numbered how-to. |
| **Guideline** | Recommended, non-binding best practice. Advice, not a mandate. | Optional | "We *recommend* / *should* consider." |

🎯 **Exam trap** — **Standard vs. guideline.** A standard is *mandatory and specific* ("must use AES-256"); a guideline is *optional and advisory* ("should consider encrypting sensitive files"). The keyword **"recommended" / "should consider"** signals guideline; **"required" / "must"** signals standard or policy. This is the most-tested governance distinction.

🎯 **Exam trap** — **Policy vs. procedure.** Policy = the high-level *what/why* set by management. Procedure = the granular *how*. If the document describes intent → policy; if it's numbered steps → procedure.

### Policies

A **policy** declares management's position. SY0-701 names a specific set you should recognize on sight.

| Policy | Governs |
|---|---|
| **Acceptable Use Policy (AUP)** | What users may and may not do with org systems, data, and network. The document new hires sign. |
| **Information security policy** | The overarching statement of how the organization protects confidentiality, integrity, and availability. |
| **Business continuity (BCP)** | How the business keeps *critical operations* running during/after a disruption. Broad, business-wide. |
| **Disaster recovery (DRP)** | How *IT systems and data* are restored after a disaster. The technical subset of continuity. |
| **Incident response (IRP)** | How the org detects, responds to, and recovers from security incidents (ties to [[Domain 4 — Security Operations]]). |
| **Software development lifecycle (SDLC)** | How software is securely designed, built, tested, and maintained across its lifecycle. |
| **Change management** | How changes to systems are requested, approved, tested, and deployed (the discipline detailed in [[Domain 1 — General Security Concepts]] 1.3). |

🎯 **Exam trap** — **BCP vs. DRP.** Business continuity is the *whole business staying operational* (people, processes, alternate sites, communications). Disaster recovery is the *IT-restoration slice* (systems, data, backups). If the scenario is broad/business-wide → BCP; if it's "restore the servers and data" → DRP. DR is a component *of* BC.

### Standards

A **standard** turns a policy into enforceable specifics.

| Standard | Specifies |
|---|---|
| **Password** | Length, complexity, rotation, history, lockout — the exact rules backing the auth policy. |
| **Access control** | The authorization model and rules (least privilege, RBAC assignments, review cadence). |
| **Physical security** | Required physical controls — badge access, camera coverage, vestibules at sensitive areas. |
| **Encryption** | Approved algorithms and key lengths (e.g., AES-256 at rest, TLS 1.2+ in transit) and where each applies. |

### Procedures

A **procedure** is the repeatable how-to that staff follow.

| Procedure | Covers |
|---|---|
| **Change management** | The operational steps to execute an approved change safely. |
| **Onboarding** | Provisioning a new hire — accounts, access, equipment, training, agreements signed. |
| **Offboarding** | Deprovisioning a departing worker — disabling accounts, recovering assets, revoking access *promptly*. |
| **Playbooks** | Predefined response steps for a specific scenario (e.g., a phishing report, ransomware detection). Tie to SOAR in [[Domain 4 — Security Operations]]. |

🎯 **Exam trap** — **Offboarding is a security control, not just HR.** The exam's offboarding answer is usually about **promptly disabling access** to prevent a former employee retaining entry — that's the risk it's testing, not returning the laptop.

### External considerations

Governance doesn't happen in a vacuum; outside forces dictate requirements. SY0-701 lists drivers and geographic scopes.

| Consideration | Meaning / banking example |
|---|---|
| **Regulatory** | Government-mandated rules a regulator enforces (GLBA, FFIEC guidance, PCI is contractual not regulatory). |
| **Legal** | Statutes and case law creating liability and obligations (breach-notification laws, contracts). |
| **Industry** | Sector frameworks and requirements (PCI DSS for card data, HIPAA for health). |
| **Local / regional / national / global** | The *jurisdictional scope* — a state privacy law (regional), GLBA (national), GDPR (global reach for EU data subjects). |

⚡ **Keyword association** — "government mandate / regulator enforces" → regulatory. "statute / lawsuit / liability" → legal. "PCI / HIPAA / sector requirement" → industry. "EU data subjects / cross-border" → global.

### Monitoring and revision

Governance documents are living. **Monitoring** watches whether controls and policies are actually working and stay aligned with the threat and regulatory landscape; **revision** updates them on a schedule (and after incidents, audits, or major changes). A policy that's never reviewed becomes a finding.

⚡ **Keyword association** — "policies reviewed annually / updated after the incident" → monitoring and revision.

### Governance structures

*Who* holds the authority.

| Structure | What it is |
|---|---|
| **Boards** | The top governing body (e.g., board of directors) with ultimate accountability and oversight. |
| **Committees** | Specialized groups under the board (e.g., a security or audit committee) focused on a domain. |
| **Government entities** | External authorities — regulators, agencies — that impose and enforce requirements. |
| **Centralized** | One authority/team sets and enforces security org-wide. Consistent, easier to audit, can be a bottleneck. |
| **Decentralized** | Authority distributed to business units/regions. Flexible and responsive locally, harder to keep consistent. |

🎯 **Exam trap** — **Centralized vs. decentralized governance.** Centralized = uniformity and control (one team decides for everyone); decentralized = agility and local fit (each unit decides). If the scenario stresses *consistency/standardization* → centralized; if it stresses *flexibility/local autonomy* → decentralized.

### Roles & responsibilities

Data-handling roles are a guaranteed exam item; the controller-vs-processor split repeats in 5.4 privacy.

| Role | Responsibility |
|---|---|
| **Data owner** | Senior person *accountable* for a data set — classifies it and decides who may access it. Sets policy, doesn't do the hands-on work. |
| **Data controller** | The entity that *determines the purpose and means* of processing personal data (the "why and how"). Bears the legal obligation. |
| **Data processor** | A party that processes data *on behalf of the controller*, following its instructions (e.g., a SaaS vendor, a payroll provider). |
| **Data custodian / steward** | The *hands-on* role implementing and maintaining the controls the owner mandates — backups, access provisioning, security settings. (Steward leans toward data *quality/governance*; custodian toward technical handling — the exam often treats them together.) |

🎯 **Exam trap** — **Owner vs. custodian.** The owner is *accountable* and *decides* (classification, who gets access); the custodian *implements and maintains* (applies the permissions, runs the backups). "Decides classification" → owner. "Configures and maintains the controls" → custodian/steward.

🎯 **Exam trap** — **Controller vs. processor.** The controller *decides why and how* personal data is processed and owns the legal liability; the processor *acts on the controller's instructions*. Your bank is the **controller** of customer PII; the cloud vendor running your core is a **processor**. Don't flip these — it's a privacy-law staple (5.4).

🧠 **Mnemonic** — Governance levels top-down: **"People Should Pick Guidance"** → **P**olicy (intent), **S**tandard (mandatory specifics), **P**rocedure (steps), **G**uideline (optional advice). The first three are mandatory; the **G** at the end is the one optional level.

🔬 **Hands-on** — Your bank already runs this hierarchy: the information security policy is board-approved, the password *standard* is enforced as an Entra ID password protection / Conditional Access setting, the offboarding *procedure* is the disable-and-revoke runbook you execute, and your SOC's phishing *playbook* fires when a user reports a message. Map one real control in your environment to each of the four levels and the abstraction collapses into your day job.

---

## 5.2 Risk management process

Risk management is the lifecycle of finding, sizing, deciding on, and tracking risk. This sub-objective owns the only math on the exam — and it's free points if you memorize two formulas. **Risk** here is the combination of a threat exploiting a vulnerability and the resulting impact.

### Risk identification

The first step: discovering and cataloging risks — assets, the threats against them, the vulnerabilities present, and potential impacts. Outputs feed the assessment and the risk register. You can't manage what you haven't named.

### Risk assessment — cadence types

*How often* you assess.

| Type | When run | Use |
|---|---|---|
| **Ad hoc** | On demand, triggered by an event (new threat, incident, major change). | React to a specific trigger. |
| **One-time** | A single, bounded assessment for a specific purpose. | A new project, an acquisition. |
| **Recurring** | On a fixed schedule (quarterly, annually). | Routine compliance and posture checks. |
| **Continuous** | Always-on, real-time monitoring of risk. | Mature programs; dynamic environments. |

⚡ **Keyword association** — "triggered by an event / unplanned" → ad hoc. "scheduled / annual review" → recurring. "real-time / always on" → continuous.

### Risk analysis — qualitative vs. quantitative

| | **Qualitative** | **Quantitative** |
|---|---|---|
| **Basis** | Subjective ratings — High/Medium/Low, heat maps, expert judgment. | Objective numbers — dollars, percentages, probabilities. |
| **Output** | Relative priority/severity | Concrete financial figures (SLE, ALE) |
| **Speed/cost** | Fast, cheap, easy to communicate | Slow, data-hungry, harder |
| **When** | Hard-to-quantify or fast triage | Cost-justifying controls (ROI on a safeguard) |

🎯 **Exam trap** — **Qualitative = subjective labels (H/M/L); quantitative = hard numbers ($).** If the scenario uses High/Medium/Low or a color heat map → qualitative. If it computes dollar losses or probabilities → quantitative. The presence of a *dollar figure* almost always means quantitative.

### The quantitative formulas — memorize these

These are the guaranteed-points calculations. Define each term, then run the chain.

| Term | Means | Unit |
|---|---|---|
| **AV** (Asset Value) | What the asset is worth. | $ |
| **EF** (Exposure Factor) | Percentage of the asset's value lost in *one* incident. | % (0–1) |
| **SLE** (Single Loss Expectancy) | Expected dollar loss from *one* occurrence. | $ |
| **ARO** (Annualized Rate of Occurrence) | How many times per year the event is expected. | count/yr |
| **ALE** (Annualized Loss Expectancy) | Expected dollar loss *per year*. | $/yr |

**The two formulas:**

> **SLE = AV × EF**
> **ALE = SLE × ARO**  (equivalently, ALE = AV × EF × ARO)

**Worked example 1 — laptop theft.** A fleet laptop is worth **AV = $2,000**. A theft destroys 100% of that asset's value, so **EF = 1.0 (100%)**. You expect **ARO = 3** thefts per year.
- SLE = AV × EF = $2,000 × 1.0 = **$2,000**
- ALE = SLE × ARO = $2,000 × 3 = **$6,000/yr**
A control that prevents laptop theft is cost-justified only if it costs *less* than $6,000/yr to operate.

**Worked example 2 — partial loss.** A database server worth **AV = $100,000**. A ransomware hit corrupts 40% of its value, so **EF = 0.40**. You estimate **ARO = 0.5** (once every two years).
- SLE = $100,000 × 0.40 = **$40,000**
- ALE = $40,000 × 0.5 = **$20,000/yr**

**Cost-benefit:** the value of a safeguard = **ALE(before) − ALE(after) − annual cost of the control**. If that number is positive, the control pays for itself.

🎯 **Exam trap** — **SLE is per-incident; ALE is per-year.** The single most-missed step is forgetting to multiply by ARO. If the question asks "annual" or "per year," you *must* apply ARO. If it asks the loss from "a single event," stop at SLE.

🎯 **Exam trap** — **EF is a percentage, not a dollar amount.** A 50% exposure factor is 0.5. A common bait is handing you EF as a percent and AV as dollars and seeing if you multiply correctly. Also: **ARO can be a fraction** — "once every four years" = 0.25, not 4.

⚡ **Keyword association** — "loss from one event" → SLE. "loss per year / annual" → ALE. "how many times per year" → ARO. "percent of value lost per event" → EF.

**Probability, likelihood, impact:** *Probability* and *likelihood* express the chance an event occurs (likelihood often qualitative, probability often a number). *Impact* is the magnitude of harm if it does. Risk is the product of likelihood and impact — high-likelihood/high-impact risks rise to the top of the register.

### Risk register

The central document tracking each identified risk through its life. SY0-701 names three fields you must know.

| Field | What it captures |
|---|---|
| **Key Risk Indicator (KRI)** | A metric that signals rising risk *before* it materializes — an early-warning gauge (e.g., percentage of overdue patches, failed-login spikes). |
| **Risk owner** | The person *accountable* for managing a specific risk and its treatment. |
| **Risk threshold** | The level at which a risk becomes unacceptable and demands action. |

⚡ **Keyword association** — "early-warning metric / leading indicator" → KRI. "accountable for this specific risk" → risk owner. "trigger point for action / unacceptable level" → risk threshold.

### Risk tolerance vs. risk appetite

| Term | Definition |
|---|---|
| **Risk appetite** | The *amount and type* of risk an organization is *willing to pursue* to meet its objectives — a forward-looking, strategic stance set by leadership. |
| **Risk tolerance** | The *acceptable variation/deviation* around that appetite — how much wobble the org can absorb before it acts. |

**Risk appetite postures:**

| Posture | Stance |
|---|---|
| **Expansionary** | Willing to take *more* risk to pursue growth/opportunity. |
| **Conservative** | Risk-averse; prioritizes protection over opportunity (typical for a community bank). |
| **Neutral** | Balanced middle ground. |

🎯 **Exam trap** — **Appetite vs. tolerance.** Appetite = how much risk you *want* to take (strategic, set in advance). Tolerance = how much *deviation* from that you'll *accept* before reacting (the operational band). "Willing to pursue" → appetite. "Acceptable variance / threshold of deviation" → tolerance. A bank's appetite is conservative; its tolerance is the small band around that it won't exceed.

### Risk strategies

What you *do* about a risk once sized. Four core responses.

| Strategy | What you do | Example |
|---|---|---|
| **Mitigate** | Reduce likelihood or impact with controls. | Deploy MFA, patch, segment. The default. |
| **Transfer** | Shift the financial impact to a third party. | Buy cyber-insurance; outsource to a vendor contractually. |
| **Accept** | Acknowledge and live with the risk (cost of treatment > the risk). | Document and move on. |
| **Avoid** | Eliminate the risk by not doing the activity. | Discontinue the risky feature/service entirely. |

**Acceptance sub-types:** an **exemption** is a formally approved, often standing pass from a requirement; an **exception** is a documented, usually time-bound deviation for a specific case. Both are *accept* decisions that must be recorded and approved — undocumented acceptance is just negligence.

🎯 **Exam trap** — **Transfer vs. accept.** Transfer *moves* the impact (insurance, contractual shift) — you still have the risk but someone else pays. Accept *keeps* the risk and its cost in-house. "Buy insurance" → transfer (the classic). "Decide it's cheaper to live with it" → accept.

🎯 **Exam trap** — **Avoid vs. mitigate.** Avoid *eliminates the activity* so the risk can't occur; mitigate *keeps the activity* but reduces the risk. "Stop offering the service" → avoid. "Add controls to make it safer" → mitigate.

⚡ **Keyword association** — "insurance / outsource the liability" → transfer. "stop doing it entirely" → avoid. "add controls / reduce" → mitigate. "document and live with it" → accept.

📌 **Scenario pattern** — "The cost of remediating a low-impact legacy risk exceeds the potential loss, so leadership signs off on a documented exception." → **Risk acceptance** (via exception).

### Risk reporting

Communicating risk status to stakeholders and leadership — the register's contents, KRIs trending past thresholds, treatment progress. Drives informed decisions and demonstrates due care to regulators and the board.

### Business impact analysis (BIA)

A BIA identifies critical business functions and the impact of their disruption over time, producing the recovery metrics that drive continuity/DR planning. The four metrics are guaranteed points.

| Metric | Means | The question it answers |
|---|---|---|
| **RTO** (Recovery Time Objective) | Max tolerable *time to restore* a function after disruption. | "How fast must we be back up?" |
| **RPO** (Recovery Point Objective) | Max tolerable *data loss*, measured as time. | "How much data (in time) can we afford to lose?" |
| **MTTR** (Mean Time To Repair/Recover) | Average time to *fix* a failed component and restore service. | "How long does a repair take on average?" |
| **MTBF** (Mean Time Between Failures) | Average time *between* failures of a repairable system — a reliability measure. | "How reliable is it / how often does it fail?" |

🎯 **Exam trap** — **RTO vs. RPO.** RTO is about **time to recover** (downtime you can tolerate). RPO is about **data loss** (how far back your last good recovery point can be) — and RPO directly dictates **backup frequency**. If the answer hinges on *how often you back up* → RPO. If it hinges on *how long you can be down* → RTO. Picture a timeline: RPO is *before* the outage (last backup), RTO is *after* (restore deadline).

🎯 **Exam trap** — **MTTR vs. MTBF.** MTTR measures *repair* time (smaller is better). MTBF measures *time between failures* / reliability (larger is better). "How long to fix" → MTTR. "How often it fails / reliability" → MTBF. Don't confuse MTTR (Mean Time To Repair) with **MTTF** (Mean Time To Failure — for *non*-repairable items that fail once).

⚡ **Keyword association** — "how fast back online" → RTO. "how much data loss / backup interval" → RPO. "average repair time" → MTTR. "reliability / time between failures" → MTBF.

🧠 **Mnemonic** — BIA pair: **"RTO = Time Off; RPO = Points Of data."** RT**O** ties to **time** down; RP**O** ties to data **points** lost. And **"R**epair = **R** in MTT**R**, lower is better; **B**etween-failures = **B** in MT**B**F, higher is better."

🔬 **Hands-on** — Run a mini-BIA on your own M365 tenant: set an RPO by checking your Exchange/SharePoint backup or retention interval, set an RTO by timing how fast you could restore a deleted mailbox or site, and pull failure/uptime data for a home-lab server to estimate MTBF. Then build a one-row risk register in Excel for "ransomware on the file server" with AV/EF/ARO, compute SLE and ALE, and assign yourself as risk owner with a patch-coverage KRI. That single sheet exercises every 5.2 term at once.

---

## 5.3 Third-party risk

Vendors inflate your attack surface — their breach is your breach. This is bread-and-butter for a banker under FFIEC third-party guidance, and the exam tests the assessment tools and, heavily, the *agreement-type acronyms*.

### Vendor assessment

How you evaluate a vendor's security before and during the relationship.

| Method | What it is |
|---|---|
| **Penetration testing** | Authorized simulated attack against the vendor's systems to find exploitable weaknesses (see 5.5). |
| **Right-to-audit clause** | A contract term letting *you* audit the vendor's security/controls. Negotiate it *in advance* — you can't audit without it. |
| **Evidence of internal audits** | The vendor's own audit results, showing they self-check. |
| **Independent assessments** | Third-party evaluations (e.g., **SOC 2 Type II**, ISO 27001 cert) — objective, not self-reported. |
| **Supply chain analysis** | Examining the vendor's *own* vendors/dependencies — risk you inherit transitively (the SolarWinds problem). |

🎯 **Exam trap** — **Right-to-audit clause must be negotiated up front.** The exam's lesson is that without the clause in the contract, you have no legal standing to inspect the vendor. If a scenario laments "we can't verify the vendor's controls," the missing piece is the right-to-audit clause.

🎯 **Exam trap** — **Independent (third-party) assessment vs. evidence of internal audit.** Independent = an outside party attests (SOC 2) — more trustworthy. Internal audit evidence = the vendor grading itself — useful but self-interested. If the scenario wants *objective* assurance → independent third-party assessment.

### Vendor selection

| Concept | Meaning |
|---|---|
| **Due diligence** | The investigation you perform *before* engaging — financial health, security posture, reputation, references. "Look before you sign." |
| **Conflict of interest** | A relationship that could bias the selection (a vendor tied to a decision-maker). Must be disclosed and managed to keep selection objective. |

⚡ **Keyword association** — "investigate before signing / vet the vendor" → due diligence. "personal/financial tie influencing the choice" → conflict of interest.

### Agreement types — memorize the acronyms

This is the highest-yield acronym cluster in the domain. CompTIA loves "which agreement is this?"

| Acronym | Full name | What it does |
|---|---|---|
| **SLA** | Service Level Agreement | Defines the *measurable service levels* expected — uptime %, response times, penalties for missing them. The performance contract. |
| **MOU** | Memorandum of Understanding | A *non-binding* statement of intent — both parties agree to cooperate, but it's not a legal contract. |
| **MOA** | Memorandum of Agreement | More formal than an MOU; spells out *specific responsibilities* of each party. Closer to binding than an MOU. |
| **MSA** | Master Service Agreement | An *umbrella* contract setting overall terms; individual projects are added later via SOWs/work orders. Negotiate the big terms once. |
| **SOW / Work order** | Statement of Work | The *specific scope, deliverables, timeline, and cost* for a particular project — operates under an MSA. |
| **NDA** | Non-Disclosure Agreement | Legally binds parties to keep shared information *confidential*. |
| **BPA** | Business Partners Agreement | Governs a *partnership* — how two businesses share responsibilities, profits, liabilities, decision-making. |

🎯 **Exam trap** — **SLA vs. MOU.** SLA = *measurable, enforceable* service metrics (99.9% uptime). MOU = *non-binding* intent. If the scenario specifies uptime/response numbers with penalties → SLA. If it's "we agree to work together" with no teeth → MOU.

🎯 **Exam trap** — **MOU vs. MOA.** Both are memoranda; the MOU is the looser, non-binding *understanding*, the MOA is the more formal one defining *specific obligations/responsibilities*. "Intent to cooperate, not binding" → MOU. "Defined responsibilities, more formal" → MOA.

🎯 **Exam trap** — **MSA vs. SOW.** MSA is the *master umbrella* (general terms, signed once); SOW is the *per-project detail* (scope, deliverables, dates) under it. "Overall framework terms" → MSA. "This specific project's deliverables and timeline" → SOW.

⚡ **Keyword association** — "uptime / response time / penalties" → SLA. "non-binding intent" → MOU. "confidentiality / secret" → NDA. "umbrella / master terms" → MSA. "deliverables / project scope" → SOW. "partnership responsibilities" → BPA.

### Vendor monitoring, questionnaires, rules of engagement

| Item | Meaning |
|---|---|
| **Vendor monitoring** | Ongoing oversight *after* signing — tracking the vendor's security posture, SLA performance, and news of their breaches throughout the relationship. Not a one-time check. |
| **Questionnaires** | Standardized security questionnaires (e.g., SIG, CAIQ) the vendor completes so you can assess their controls at scale. |
| **Rules of engagement (RoE)** | The agreed boundaries for an assessment/pen test — scope, timing, allowed techniques, targets off-limits, contacts. Defines what testers may and may not do. |

🎯 **Exam trap** — **Rules of engagement vs. SOW.** RoE specifically governs *what an assessment/pen test may do* (scope, methods, timing, off-limits systems). SOW defines a project's deliverables broadly. If the scenario is about *the boundaries of a security test*, the answer is rules of engagement.

🔬 **Hands-on** — Pull a real vendor's **SOC 2 Type II** report (many publish summaries) and read the controls and exceptions — that's the "independent assessment" artifact your bank relies on instead of auditing every vendor directly. Then draft a one-page right-to-audit clause and a short security questionnaire; you've now produced two of the assessment instruments this objective names.

---

## 5.4 Compliance

Compliance is conforming to the laws, regulations, standards, and contracts that bind you — and *proving* it. For a regulated bank this is existential; the exam tests reporting, the consequences of failure, how compliance is monitored, and privacy mechanics.

### Compliance reporting

| Type | Audience |
|---|---|
| **Internal** | Reporting compliance status to leadership/board/audit committee — self-awareness and governance. |
| **External** | Reporting to regulators, auditors, partners, or the public as required by law/contract. |

### Consequences of non-compliance

The exam wants you to recognize these as the *cost* of failing compliance.

| Consequence | What it is |
|---|---|
| **Fines** | Monetary penalties imposed by regulators (e.g., GDPR fines up to 4% of global revenue). |
| **Sanctions** | Punitive restrictions or actions imposed by an authority. |
| **Reputational damage** | Loss of customer/market trust after a public failure — often the costliest long-term. |
| **Loss of license** | Revocation of the legal authority to operate (catastrophic for a bank). |
| **Contractual impacts** | Breaching compliance terms in a contract — penalties, termination, liability to partners. |

⚡ **Keyword association** — "monetary penalty from regulator" → fines. "can no longer legally operate" → loss of license. "lost customer trust / brand harm" → reputational damage.

### Compliance monitoring

How you keep yourself compliant and prove it.

| Mechanism | What it is |
|---|---|
| **Due diligence / due care** | *Due diligence* = the investigation/research to understand obligations and risks. *Due care* = actually *doing* the reasonable things to meet them. (Diligence = research; care = action.) |
| **Attestation & acknowledgement** | Formally *affirming* compliance — signing that you've read the policy / that controls are in place. Creates an accountability record. |
| **Internal / external** | Monitoring performed by your own team vs. by outside parties (auditors, regulators). |
| **Automation** | Using tools (GRC platforms, continuous-control monitoring) to track compliance at scale rather than manually. |

🎯 **Exam trap** — **Due diligence vs. due care.** Due diligence is *investigating/knowing* (the homework); due care is *acting* (doing the reasonable thing a prudent person would). "Researched the vendor's risk" → due diligence. "Implemented the controls a reasonable org would" → due care. Both are needed to show you weren't negligent.

### Privacy

| Term | Definition |
|---|---|
| **Data subject** | The individual whom the personal data is *about* (your customer, under GDPR-style law). |
| **Controller** | Determines the *purpose and means* of processing — bears legal responsibility (your bank for customer PII). |
| **Processor** | Processes data *on the controller's behalf* per instructions (a vendor/SaaS). |
| **Data ownership** | Who has rights and accountability over a data set — drives who may decide its use and protection. |
| **Data inventory & retention** | Knowing *what data you hold and where* (inventory) and *how long you keep it* before secure disposal (retention). Retention schedules are both a legal requirement and a risk-reduction measure (you can't lose what you've purged). |
| **Right to be forgotten** | A data subject's right to demand *erasure* of their personal data (GDPR "right to erasure"). |
| **Legal implications (local→global)** | Privacy obligations vary by jurisdiction; a global org may face GDPR (EU), state laws (regional), and national statutes simultaneously. |

🎯 **Exam trap** — **Controller vs. processor (privacy edition).** Same split as 5.1 roles: the controller *decides why/how* and holds liability; the processor *executes on instructions*. Under GDPR both have obligations, but the controller is primarily accountable. Don't assume the vendor (processor) shoulders the legal blame — the controller does.

🎯 **Exam trap** — **Right to be forgotten** is specifically *erasure on request*. Don't confuse it with data minimization (collecting less) or retention limits (auto-deleting on a schedule). The trigger word is the data subject *requesting deletion*.

⚡ **Keyword association** — "the person the data is about" → data subject. "decides purpose/means, holds liability" → controller. "processes on instructions" → processor. "request to erase my data" → right to be forgotten. "how long we keep it" → retention.

🔬 **Hands-on** — In M365 Purview, build a **retention policy** and a **DLP policy** for customer PII, and run a content search to satisfy a mock "right to be forgotten" deletion request. That maps the abstract privacy terms (retention, data inventory, erasure) onto buttons you can actually click in your tenant.

---

## 5.5 Audits & assessments

Audits and assessments *verify* that controls exist and work. Know the internal/external split and — heavily tested — the penetration-testing taxonomy.

### Attestation

A formal declaration, often by an independent party, that something is true — that controls were tested and operate as claimed (e.g., an auditor *attests* to your SOC 2 controls). It's the credibility-bearing sign-off on an assessment's results.

### Internal audits

| Type | What it is |
|---|---|
| **Compliance audit** | Internal check that you meet a specific regulation/standard. |
| **Audit committee** | A board-level group overseeing the audit function and internal controls independently of management. |
| **Self-assessment** | The team evaluating its *own* controls against a baseline — cheap and frequent, but inherently less objective. |

### External audits

| Type | What it is |
|---|---|
| **Regulatory** | Conducted by/for a regulator to verify legal compliance (your FFIEC exam). |
| **Examinations** | Formal regulator reviews (banking "exams"). |
| **Assessment** | A broader evaluation of security posture, not strictly pass/fail compliance. |
| **Independent third-party audit** | An outside, objective firm audits you — the gold standard for external assurance (e.g., a SOC 2 audit by a CPA firm). |

🎯 **Exam trap** — **Internal self-assessment vs. independent third-party audit.** Self-assessment is *you grading yourself* (subjective, cheap, frequent). Independent audit is an *outside party* grading you (objective, credible, what regulators/customers trust). If the scenario needs *unbiased external assurance* → independent third-party audit.

### Penetration testing

A pen test is an authorized, simulated attack that actively attempts to *exploit* weaknesses — going beyond a vulnerability scan (which only *finds* them) to *prove* exploitability. SY0-701 slices pen testing three ways: by **purpose**, by **environment knowledge**, and by **reconnaissance method**.

**By purpose / posture:**

| Term | Role |
|---|---|
| **Offensive** | The attacking "red team" — actively trying to break in. |
| **Defensive** | The defending "blue team" — detecting and responding to the attack. |
| **Physical** | Testing physical controls — tailgating, lock-picking, badge cloning, facility entry. |
| **Integrated** | Combining offensive and defensive (and often physical/digital) into a coordinated exercise ("purple team" collaboration). |

**By environment knowledge:**

| Term | Tester knows | Old name |
|---|---|---|
| **Known environment** | Full knowledge — architecture, source, credentials provided. | White box |
| **Partially known environment** | Some information provided. | Gray box |
| **Unknown environment** | No prior knowledge — simulates an outside attacker starting from zero. | Black box |

🎯 **Exam trap** — **Known / partially known / unknown** are the SY0-701 names; older material says **white/gray/black box**. Map them: known = white, partially known = gray, unknown = black. **Unknown/black box** best simulates a real external attacker; **known/white box** is most thorough/efficient because nothing is hidden.

**Reconnaissance:**

| Type | What it is |
|---|---|
| **Passive** | Gathering info *without touching* the target — OSINT, public records, DNS, social media, WHOIS. Undetectable by the target. |
| **Active** | Directly *interacting* with the target — port scanning, enumeration, probing. Effective but detectable. |

🎯 **Exam trap** — **Passive vs. active reconnaissance.** Passive = no direct contact, no footprint (Google/OSINT/WHOIS). Active = touching the target (scanning, probing) and risking detection. "Without alerting the target / public sources only" → passive. "Port scan / direct probe" → active.

⚡ **Keyword association** — "red team / attacking" → offensive. "blue team / defending" → defensive. "no knowledge / external attacker" → unknown (black box). "full source and creds" → known (white box). "OSINT / no contact" → passive recon.

🧠 **Mnemonic** — Box-to-knowledge: **"Wide Knowledge, Grey Partial, Black Unknown"** → **W**hite = **K**nown, **G**rey = **P**artially known, **B**lack = **U**nknown.

🔬 **Hands-on** — Do passive recon on your own bank's public footprint with `nslookup`, `whois`, and a Shodan search — no packets sent to internal systems, fully passive. Then in your VirtualBox lab, run an *active* `nmap` scan against a target VM you own and watch it light up the logs (that detectability is exactly the passive-vs-active distinction). Stand up a tiny known-environment test by giving yourself the box's creds and architecture first.

---

## 5.6 Security awareness

The human layer. Technical controls fail to stupid clicks, so a security-awareness program is itself a control (operational, per [[Domain 1 — General Security Concepts]]). SY0-701 walks the program lifecycle.

### Phishing

| Element | What it is |
|---|---|
| **Campaigns** | Simulated phishing emails sent to your own users to *measure and train* susceptibility. Track click rates over time. |
| **Recognizing** | Teaching users the tells — urgency, mismatched/look-alike domains, unexpected attachments, requests for credentials or payment changes. |
| **Responding to reported suspicious messages** | The process *after* a user reports — triage, analysis, and feedback so reporting is reinforced, not ignored. |

⚡ **Keyword association** — "simulated phishing emails to staff / click rate" → phishing campaign.

### Anomalous behavior recognition

Training users (and analysts) to spot behavior that's off. SY0-701 names three flavors.

| Type | Meaning |
|---|---|
| **Risky** | Behavior that knowingly increases risk — using unapproved tools, disabling controls, weak password reuse. |
| **Unexpected** | Behavior out of the ordinary for that user/system — a login from a new country at 3 a.m., bulk downloads. |
| **Unintentional** | Accidental, well-meaning mistakes — misaddressed email, accidental data exposure, falling for a phish. |

🎯 **Exam trap** — **Risky vs. unintentional behavior.** Risky is a *knowing* choice that raises risk (bypassing a control on purpose); unintentional is an *honest mistake* (no intent to harm). The intent/awareness is the dividing line.

### User guidance and training

The curriculum. Cover all of it.

| Topic | Why it's taught |
|---|---|
| **Policy / handbooks** | Users must know the rules (AUP, security policy) they're held to. |
| **Situational awareness** | Staying alert to threats in the moment — recognizing a tailgater, a pretext call, an odd USB on the floor. |
| **Insider threat** | Recognizing and reporting malicious or negligent behavior from within (ties to [[Domain 2 — Threats, Vulnerabilities & Mitigations]]). |
| **Password management** | Strong, unique passwords; password managers; not reusing or sharing. |
| **Removable media and cables** | Risks of unknown USB drives and malicious cables (juice-jacking, USB-borne malware) — don't plug in found devices. |
| **Social engineering** | Recognizing manipulation — phishing, pretexting, vishing, tailgating, authority/urgency pressure. |
| **Operational security (OPSEC)** | Not leaking sensitive operational details (publicly, on social media, in conversation) that an attacker could aggregate. |
| **Hybrid / remote work** | Securing home networks, VPN use, physical/screen privacy, and shadow IT outside the office. |

⚡ **Keyword association** — "found USB / don't plug it in" → removable media training. "don't overshare operational details" → OPSEC. "spot the tailgater / be alert" → situational awareness.

### Reporting, monitoring, development, execution

The program's operational cycle.

| Phase | What it is |
|---|---|
| **Reporting & monitoring — initial** | Establishing baselines and channels when the program launches — first metrics, how users report. |
| **Reporting & monitoring — recurring** | Ongoing measurement (phishing click rates, training completion, reported-incident counts) to track improvement. |
| **Development** | Building/updating the training content to stay current with threats. |
| **Execution** | Actually delivering the training and running the campaigns. |

🎯 **Exam trap** — **Initial vs. recurring** monitoring. Initial sets the baseline at launch; recurring tracks it continuously thereafter. If the scenario describes *first-time setup* → initial; *ongoing measurement* → recurring. A program that trains once and never measures again has skipped the recurring phase — a likely "what's missing?" answer.

🔬 **Hands-on** — Run a real phishing campaign in **Microsoft Defender for Office 365 Attack Simulation Training**: launch a simulated phish, capture the *initial* click-rate baseline, push targeted training to clickers, then re-run quarterly to produce the *recurring* trend. You've now executed every 5.6 phase — development, execution, initial and recurring monitoring — inside your own tenant.

---

## 🎯 Top exam traps

1. **Standard (mandatory, specific) vs. guideline (optional, advisory).** "Must/required" → standard or policy; "should/recommended" → guideline. Policy = high-level *what*; procedure = step-by-step *how*.
2. **BCP vs. DRP.** Business continuity = the whole business stays running; disaster recovery = the IT-restoration slice. DR is part of BC.
3. **SLE is per-incident, ALE is per-year.** Forgetting to multiply by ARO is the #1 math miss. EF is a percentage (0.5, not $); ARO can be a fraction (0.25 = once per four years).
4. **Quantitative = dollars/numbers; qualitative = High/Medium/Low labels.** A dollar figure almost always means quantitative.
5. **RTO vs. RPO.** RTO = time to recover (downtime tolerated); RPO = data loss tolerated → drives backup frequency. RPO is *before* the outage on the timeline, RTO is *after*.
6. **MTTR vs. MTBF.** MTTR = repair time (lower better); MTBF = time between failures / reliability (higher better). Don't confuse MTTR with MTTF (non-repairable).
7. **Risk appetite (how much risk you want, strategic) vs. tolerance (acceptable deviation around it).** Postures: expansionary / conservative / neutral.
8. **Risk strategies:** transfer = move the impact (insurance); accept = keep it (documented exception/exemption); avoid = stop the activity; mitigate = add controls. Transfer ≠ accept; avoid ≠ mitigate.
9. **Owner/controller decide; custodian/processor execute.** Controller holds privacy liability even though the processor (vendor) does the work.
10. **Agreement acronyms:** SLA = measurable service metrics; MOU = non-binding intent; MOA = more formal, defined responsibilities; MSA = master umbrella; SOW = per-project deliverables; NDA = confidentiality; BPA = partnership.
11. **Right-to-audit clause must be negotiated up front**, or you can't inspect the vendor. Independent third-party assessment (SOC 2) beats self-reported internal audit for objective assurance.
12. **Due diligence (research/know) vs. due care (act/do the reasonable thing).** Both needed to rebut negligence.
13. **Right to be forgotten = erasure on the data subject's request** — not minimization, not scheduled retention deletion.
14. **Pen-test environments:** known = white box, partially known = gray, unknown = black. Unknown best simulates an external attacker. **Passive recon** = no contact (OSINT); **active recon** = touching the target (scanning), detectable.
15. **Self-assessment (grade yourself) vs. independent third-party audit (outside, objective).** Regulators trust the latter. Rules of engagement define what a *pen test* may do; SOW defines a *project's* deliverables.

## 🧠 Mnemonic pack

- **Governance levels (top-down) — "People Should Pick Guidance"**
  - **P**eople → **P**olicy (high-level intent, mandatory)
  - **S**hould → **S**tandard (mandatory specifics)
  - **P**ick → **P**rocedure (mandatory steps)
  - **G**uidance → **G**uideline (optional advice)
  - ✅ Maps to Policy, Standard, Procedure, Guideline — the four levels in descending order; the trailing **G** is the one *optional* level, the other three are mandatory.

- **BIA recovery pair — "RTO = Time Off; RPO = Points Of data"**
  - RT**O** → tied to **time** the system is down (recovery *time*)
  - RP**O** → tied to data **points** lost (recovery *point* / data loss)
  - ✅ Correctly separates RTO (downtime tolerated) from RPO (data loss tolerated → backup frequency).

- **Repair vs. reliability — "R is Repair (lower better); B is Between-failures (higher better)"**
  - MTT**R** → **R**epair time — smaller is better
  - MT**B**F → time **B**etween failures (reliability) — larger is better
  - ✅ Maps the distinguishing letter of each metric to its meaning and its "good direction."

- **Pen-test environment knowledge — "Wide Knowledge, Grey Partial, Black Unknown"**
  - **W**ide (white) → **K**nown environment
  - **G**rey → **P**artially known environment
  - **B**lack → **U**nknown environment
  - ✅ Maps the three box colors to the three SY0-701 environment-knowledge names.

## ✅ Self-check

1. A document states "All passwords must be at least 14 characters." Is that a policy, a standard, a procedure, or a guideline?
2. A laptop is worth $1,500. A theft destroys 100% of its value, and you expect 4 thefts per year. Compute the SLE and the ALE.
3. A database worth $80,000 suffers an incident that destroys 25% of its value once every two years. What are the EF, SLE, ARO, and ALE?
4. The board wants the *whole business* — people, alternate sites, communications — to keep running during a hurricane. Which plan governs that: BCP or DRP?
5. Your RPO is 1 hour and your RTO is 4 hours. Which one tells you how frequently to back up, and which tells you how fast you must restore?
6. A bank states it will *not* pursue growth opportunities that carry meaningful security risk, and defines a narrow band of acceptable deviation from that stance. Name the appetite *posture* and distinguish appetite from tolerance.
7. Leadership decides remediating a low-impact legacy risk costs more than the risk itself and signs a documented, time-bound deviation. Which risk strategy, and which acceptance sub-type?
8. You buy cyber-insurance to cover ransomware losses. Which risk strategy is that?
9. A contract promises 99.95% uptime with financial penalties for shortfalls. Which agreement type is this?
10. Two business units sign a *non-binding* statement that they intend to cooperate on a shared service. Which agreement type?
11. Under GDPR-style law, your bank decides why and how customer PII is processed; a SaaS vendor processes it on your instructions. Which party is the controller and which is the processor, and who holds primary legal liability?
12. An assessor gathers information using only WHOIS, DNS records, and the target's public website, never touching internal systems. Is this passive or active reconnaissance, and what's the advantage?
13. A pen tester is given full architecture diagrams, source code, and admin credentials before starting. What is the SY0-701 name for this environment, and the older color name?
14. You need *objective, externally credible* assurance that a vendor's controls work. Do you rely on their self-assessment or on an independent third-party audit (e.g., SOC 2)?
15. A new security-awareness program launches and captures the very first phishing-simulation click-rate baseline. Is that initial or recurring monitoring — and what phase is missing if the org never measures again?

<details>
<summary>Answers</summary>

1. **Standard.** It's a mandatory, specific requirement that enforces the broader password/security policy ("must" + a precise value).
2. **SLE = AV × EF = $1,500 × 1.0 = $1,500.** **ALE = SLE × ARO = $1,500 × 4 = $6,000/yr.**
3. **EF = 0.25.** **SLE = $80,000 × 0.25 = $20,000.** **ARO = 0.5** (once every two years). **ALE = $20,000 × 0.5 = $10,000/yr.**
4. **BCP** (business continuity) — it covers the whole business staying operational; DRP would be the IT-restoration subset.
5. **RPO (1 hr)** dictates backup frequency (max tolerable data loss); **RTO (4 hr)** is how fast you must restore service.
6. Posture: **conservative** (risk-averse, protection over opportunity). **Appetite** = how much risk the org is willing to pursue (strategic, set in advance); **tolerance** = the acceptable deviation/band around that appetite before it must act.
7. Strategy: **risk acceptance**; sub-type: **exception** (documented, time-bound deviation). (A standing pass would be an exemption.)
8. **Transfer** — you shift the financial impact to a third party (the insurer); the classic transfer example.
9. **SLA** (Service Level Agreement) — measurable service levels with penalties.
10. **MOU** (Memorandum of Understanding) — non-binding statement of intent.
11. Your bank is the **controller** (decides purpose and means); the vendor is the **processor** (acts on instructions). The **controller** holds primary legal liability.
12. **Passive** reconnaissance — no direct contact with the target, so it's undetectable and leaves no footprint.
13. **Known environment** (full knowledge); older name **white box**. It's the most thorough/efficient since nothing is hidden.
14. An **independent third-party audit (SOC 2)** — objective and externally credible; a self-assessment is the vendor grading itself.
15. **Initial** monitoring (the launch baseline). If the org never measures again, the missing phase is **recurring** reporting and monitoring.

</details>

⬅ [[Security+ SY0-701 — Course Index]] | ➡ Next: [[Acronym Master List]]
