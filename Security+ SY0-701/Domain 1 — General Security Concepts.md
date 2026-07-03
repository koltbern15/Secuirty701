> **Domain 1 — General Security Concepts**
> **Exam weight:** 12% (~11 questions on a 90-question form).
> **What it covers:** The vocabulary layer of the entire exam — control categories and types, the CIA triad and AAA, non-repudiation, gap analysis, Zero Trust architecture, physical security, deception tech, change-management discipline, and the full cryptography toolkit (PKI, encryption levels, hashing, signatures, certificates).
> **Strategic note:** Small domain, outsized leverage. It seeds the language every later domain reuses — you cannot reason about [[Domain 2 — Threats, Vulnerabilities & Mitigations]], [[Domain 3 — Security Architecture]], or [[Domain 4 — Security Operations]] without these definitions locked. Bank them cold. Most of the easy points here are pure recall, so the move is to memorize the distinctions CompTIA repeatedly tests rather than to "understand" them loosely.

## Objective map

- **1.1** Security controls — categories (technical, managerial, operational, physical) and types (preventive, deterrent, detective, corrective, compensating, directive).
- **1.2** Fundamental concepts — CIA triad, non-repudiation, AAA, gap analysis, Zero Trust, physical security, deception & disruption.
- **1.3** Change management — business processes, technical implications, documentation, version control.
- **1.4** Cryptographic solutions — PKI, encryption, tools, obfuscation, hashing, salting, digital signatures, key stretching, blockchain, certificates.

---

## 1.1 Security controls

A **security control** is any safeguard or countermeasure that reduces risk to an asset. On this exam you must do two things with every control you encounter: place it in a **category** (what *kind* of thing it is) and assign it a **type** (what *job* it does in the timeline of an incident). These are two independent axes — a single control has exactly one category but the *same* control can serve as a different type depending on how it's deployed. CompTIA loves that ambiguity.

### Control categories — what the control *is*

| Category | Definition | Recognize it by | Examples |
|---|---|---|---|
| **Technical** | A control implemented in hardware, software, or firmware — enforced by a system, not a human. | "The system does it." | Firewall rules, encryption, MFA, ACLs, antivirus, IDS/IPS |
| **Managerial** | A control that governs *how security is managed* — policy, planning, risk decisions made by people in charge. Also called *administrative*. | Strategy, risk assessments, governance documents. | Risk assessments, security policy, background checks (as policy), change-management process |
| **Operational** | A control executed by *people* in the course of day-to-day operations. | A human performing a recurring procedure. | Security awareness training, guards doing rounds, incident response handling, configuration management done by staff |
| **Physical** | A control you can touch that protects the physical environment and assets. | Tangible barriers and the real world. | Fences, locks, bollards, badge readers, lighting, cameras, guards (the body, not the policy) |

🎯 **Exam trap** — Managerial vs. operational. Both involve people. The split: managerial is *deciding and planning* (writing the policy, assessing risk), operational is *doing the recurring work* (running the training, performing the procedure). If a person sets direction → managerial. If a person carries out a process → operational.

🎯 **Exam trap** — A **security guard** can be physical *or* operational depending on framing. As a body standing at a door deterring entry → physical. As a person executing a patrol/monitoring procedure → operational. CompTIA usually means **physical** when the guard is a barrier presence; read the verb in the question.

🧠 **Mnemonic** — Categories: **"To Manage Our Property"** → **T**echnical, **M**anagerial, **O**perational, **P**hysical.

### Control types — what the control *does*

Think of these on a timeline relative to an incident: before, during, and after.

| Type | Job | Timing | Examples |
|---|---|---|---|
| **Preventive** | Stop an incident from happening. | Before | Firewall, ACL, MFA, encryption, door lock, separation of duties |
| **Deterrent** | Discourage an attacker from trying — psychological, not physical blocking. | Before | Warning banner, "Beware of Dog" sign, visible cameras, lighting, guard presence |
| **Detective** | Identify and alert that an incident is happening or happened. | During / After | IDS, log review, SIEM alert, motion sensor, audit trail, CCTV review |
| **Corrective** | Fix damage and restore after an incident. | After | Backup restoration, patching the exploited flaw, IPS blocking active attack, re-imaging |
| **Compensating** | An alternative when the primary control isn't feasible — does the job a different way. | Any | Network segmentation in place of an unpatchable legacy box; manual review when an automated control is down |
| **Directive** | Tell people what they must do — guidance/mandate that directs behavior. | Before | Acceptable use policy, signage that *instructs*, procedures, compliance mandates |

🎯 **Exam trap** — **Deterrent vs. preventive.** A deterrent works on the attacker's *mind* (a sign, visible camera, lighting). A preventive *physically/logically blocks* the action (a lock, an ACL). A camera you can *see* and that makes you reconsider = deterrent; the *recording* you review later = detective. Same camera, two type-answers depending on what the question emphasizes.

🎯 **Exam trap** — **Deterrent vs. directive.** Both can be signs. A "Beware of Dog" sign scares you off → deterrent. A "Authorized Personnel Only — badge required" sign that *instructs required behavior* → directive. Directive *tells you what to do*; deterrent *makes you not want to*.

🎯 **Exam trap** — **Corrective vs. compensating.** Corrective fixes the *aftermath* of a specific incident (restore from backup). Compensating *substitutes* for a control you can't deploy (you can't patch the legacy app, so you isolate it). If the question is "we couldn't do X, so we did Y instead" → compensating.

⚡ **Keyword association** — "sign / warning / fence visible to discourage" → deterrent. "alternative / can't implement the real control / temporary workaround" → compensating. "restore / backup / repair / undo" → corrective. "alert / log / detect / camera footage" → detective. "policy / mandate / acceptable use / must" → directive.

📌 **Scenario pattern** — "A legacy system cannot support MFA, so the team places it on an isolated VLAN with strict ACLs." → The isolation is a **compensating** control (category: technical). The ACL itself is **preventive**.

🧠 **Mnemonic** — Types: **"Please Don't Date Corrupt Computer Dorks"** → **P**reventive, **D**eterrent, **D**etective, **C**orrective, **C**ompensating, **D**irective.

🔬 **Hands-on** — Map your own bank stack: a Conditional Access policy in Entra ID that blocks legacy auth is **technical/preventive**. Your CyberArk session recording is **technical/detective** (it captures evidence) and the recording's existence deters insiders → **deterrent**. The quarterly access recertification campaign you run in SailPoint is **operational/detective** (it finds inappropriate access after the fact). Run through ten controls in your environment and label both axes — that drill is exactly the exam task.

---

## 1.2 Fundamental concepts

### CIA triad

The three properties security exists to protect.

| Property | Definition | Protected by | Violated when |
|---|---|---|---|
| **Confidentiality** | Information is disclosed only to those authorized to see it. | Encryption, access controls, MFA, data masking | Data is leaked/exposed |
| **Integrity** | Information is accurate and unaltered except by authorized change. | Hashing, digital signatures, checksums | Data is tampered with |
| **Availability** | Information and systems are accessible when authorized users need them. | Redundancy, backups, DDoS protection, load balancing | A DoS or outage blocks access |

⚡ **Keyword association** — "unauthorized disclosure / leaked" → confidentiality. "altered / tampered / modified / corrupted" → integrity. "DoS / outage / can't reach / downtime" → availability.

🎯 **Exam trap** — Don't confuse the **AAA** "Authentication" with CIA. Different framework. CIA is *what you protect*; AAA is *how you control access to it*.

### Non-repudiation

A guarantee that an actor cannot credibly deny having performed an action. Achieved primarily through **digital signatures** (sign with your private key; anyone verifies with your public key, proving only you could have signed) and through tamper-evident **audit logging**.

⚡ **Keyword association** — "cannot deny," "proof of origin," "prove who sent it" → non-repudiation → think **digital signature**.

🎯 **Exam trap** — Symmetric encryption does **not** provide non-repudiation, because both parties share the same key — either could have produced the message. Non-repudiation requires an *asymmetric* private key that only one party holds. This is a frequently tested distinction.

### AAA — Authentication, Authorization, Accounting

| A | Question it answers | Mechanisms |
|---|---|---|
| **Authentication** | "Are you who you claim to be?" | Passwords, MFA, biometrics, certificates, tokens |
| **Authorization** | "What are you allowed to do?" | Access control models (below), permissions, ABAC policies |
| **Accounting** | "What did you do?" | Logging, audit trails, session recording, usage metering |

**Authenticating people** uses something you know / have / are / do / somewhere you are (factors). **Authenticating systems** is machine-to-machine — a server or device proves its identity using **certificates** (e.g., a device cert issued by your PKI, or mutual TLS) rather than a human factor. This matters for Zero Trust, where *every* subject — user *or* workload — must authenticate.

**Authorization models:**

| Model | Access decided by | Best for | Note |
|---|---|---|---|
| **MAC** (Mandatory) | System-enforced labels/clearances; users cannot change | Military, classified environments | Most rigid; SELinux is an example |
| **DAC** (Discretionary) | The data *owner's* discretion | General-purpose OS file permissions | Owner grants access; flexible, weaker |
| **RBAC** (Role-Based) | The subject's *role/job function* | Enterprises, banks | Permissions attach to roles, users get roles |
| **ABAC** (Attribute-Based) | Evaluated *attributes* (user, resource, environment, time) | Dynamic/fine-grained, cloud, Zero Trust | Most flexible; powers Conditional Access |
| **Rule-Based** | Admin-defined rules applied to all (e.g., firewall ACLs, time-of-day) | Network/global enforcement | Applies uniformly regardless of role |

🎯 **Exam trap** — **RBAC vs. Rule-based** both abbreviate to "RBAC" in some sources. On SY0-701, **RBAC = Role-Based**. Rule-based is described by the word "rules" applied globally (think firewall rules, "no logins after 6pm"). If the scenario hinges on *job function* → role-based. If it hinges on *a condition/rule applied to everyone* → rule-based.

🎯 **Exam trap** — **MAC** here is **Mandatory Access Control**, not Media Access Control (the NIC address). Context decides; in 1.2 it's always the access model.

📌 **Scenario pattern** — "Access is granted based on the user's department, the resource's sensitivity label, and the time of day." → **ABAC** (multiple attributes evaluated together).

🔬 **Hands-on** — Entra ID Conditional Access *is* ABAC in production: it evaluates user risk, device compliance, location, and app sensitivity as attributes before granting. Your role assignments in Entra (e.g., Global Reader) are RBAC. Build one CA policy that requires compliant device + MFA for a sensitive app and you've implemented attribute-based authorization hands-on.

### Gap analysis

A structured comparison of your **current state** against a **desired/target state** (often a framework or compliance baseline like NIST CSF, PCI DSS, or FFIEC guidance), producing a prioritized list of the gaps to close. It's the planning step that precedes remediation and often feeds a POA&M.

⚡ **Keyword association** — "current vs. desired," "where we are vs. where we need to be," "compare against the standard" → gap analysis.

### Zero Trust

The model that abandons implicit trust based on network location. Core mantra: **never trust, always verify** — every request is authenticated, authorized, and continuously evaluated regardless of whether it originates inside or outside the perimeter. SY0-701 splits Zero Trust into two planes; memorize which components live where.

**Control Plane** — *makes* the access decisions (the brains):

| Component | Role |
|---|---|
| **Policy Engine (PE)** | Evaluates the request against policy and produces the *decision* (grant/deny). The actual decision-maker. |
| **Policy Administrator (PA)** | *Executes* the PE's decision — establishes/tears down the session and issues credentials/tokens to the enforcement point. |
| **Adaptive identity** | Identity decisions that flex with context/risk signals (location, device posture, behavior) rather than a static yes/no. |
| **Threat scope reduction** | Shrinking the attack surface and blast radius — least privilege, microsegmentation, limiting what any subject can reach. |
| **Policy-driven access control** | Access governed by centrally defined policy that the PE/PA enforce, not by ad-hoc rules at each resource. |

**Data Plane** — *enforces* and *carries* the access (the muscle):

| Component | Role |
|---|---|
| **Policy Enforcement Point (PEP)** | The gateway that actually *allows or blocks* the connection, enforcing what the control plane decided. Sits in the data path. |
| **Subject / System** | The entity (user or machine/workload) requesting access to a resource. |
| **Implicit trust zones** | Areas where, once past the PEP, communication is permitted — kept as small as possible to limit lateral movement. |

🎯 **Exam trap** — **Policy Engine vs. Policy Administrator.** PE *decides* (the verdict). PA *acts on* the decision (sets up/tears down the session, hands out the access token). "Who makes the decision?" → Engine. "Who establishes the session / issues credentials?" → Administrator.

🎯 **Exam trap** — **Control plane vs. data plane.** Control plane = decision logic (PE, PA, adaptive identity, policy). Data plane = where traffic flows and is enforced (PEP, subject/system, implicit trust zones). The **PEP lives in the data plane** even though it enforces a control-plane decision — that's the most-missed placement.

🧠 **Mnemonic** — **"Engines Decide, Admins Do"** → Policy **E**ngine = the **D**ecision; Policy **A**dministrator = **D**oes/executes it.

⚡ **Keyword association** — "never trust always verify," "no implicit trust," "verify every request" → Zero Trust. "gateway that blocks the connection" → PEP.

🔬 **Hands-on** — Entra ID is a real-world Zero Trust control plane: Conditional Access is your **Policy Engine** (evaluates signals, renders a verdict), the token issuance/session establishment is the **Policy Administrator** function, and the application/proxy enforcing the token (e.g., an app behind App Proxy or a Defender-protected resource) is the **PEP**. Trace one sign-in through this and the abstract model becomes concrete.

### Physical security

| Control | What it is | Type tendency |
|---|---|---|
| **Bollards** | Short sturdy posts that stop vehicles from ramming a building/entrance. | Preventive/deterrent |
| **Access control vestibule** | A two-door "trap" (mantrap) — the first door must close before the second opens, blocking tailgating. | Preventive |
| **Fencing** | Perimeter barrier; height/gauge determine deterrence vs. real prevention. | Deterrent/preventive |
| **Video surveillance (CCTV)** | Cameras recording activity. | Detective (footage) / deterrent (visible) |
| **Security guard** | Human presence controlling and monitoring access. | Physical / deterrent / operational |
| **Access badge** | Credential (often RFID/smartcard) presented to a reader to authenticate entry. | Preventive |
| **Lighting** | Illumination removing concealment; supports cameras and discourages intrusion. | Deterrent |
| **Sensors** | Devices that detect presence/movement/intrusion. | Detective |

**Sensor types:**

| Sensor | Detects via |
|---|---|
| **Infrared (IR)** | Body heat / changes in radiated heat (PIR motion sensors). |
| **Pressure** | Weight/force on a surface (floor mats, fence tension). |
| **Microwave** | Reflected microwave energy; senses motion across a wide area, through some materials. |
| **Ultrasonic** | Reflected high-frequency sound waves; detects movement by Doppler shift. |

🎯 **Exam trap** — **Access control vestibule** is the SY0-701 term for what older material calls a **mantrap**. Its specific job is stopping **tailgating/piggybacking** by ensuring only one person/door cycle at a time. If the scenario is "someone followed an employee in through the door," the vestibule is the control.

⚡ **Keyword association** — "stop a vehicle / ram" → bollards. "two doors / one at a time / tailgating" → vestibule. "footage / recording / after the fact" → CCTV (detective). "heat / motion" → infrared.

### Deception and disruption

| Tool | What it is | Purpose |
|---|---|---|
| **Honeypot** | A single decoy system/service designed to look attractive to attackers. | Lure, study, and detect attackers; waste their time. |
| **Honeynet** | A whole network of honeypots — a fake environment. | Observe attacker behavior at scale; realistic decoy network. |
| **Honeyfile** | A bait file (e.g., "passwords.xlsx") that should never be touched. | Detection — any access signals an intruder. |
| **Honeytoken** | A bait piece of data (fake credential, API key, DB record, canary URL). | Tracking/alerting — its use reveals a breach and can trace the path. |

🎯 **Exam trap** — **Honeypot (a system) vs. honeyfile/honeytoken (data).** A honeypot is a *machine/service*; a honeyfile is a *file*; a honeytoken is a *piece of data/credential*. A honeynet is *many honeypots networked together*. Scale and object type are the distinctions.

⚡ **Keyword association** — "decoy network" → honeynet. "fake credential / canary / bait record that alerts when used" → honeytoken. "single decoy server to lure attackers" → honeypot.

📌 **Scenario pattern** — "An analyst plants a fake admin credential that is never used legitimately; an alert fires the moment it appears in a login attempt." → **honeytoken**.

---

## 1.3 Change management

Change management is the disciplined process for making changes to systems so that you don't introduce outages, security gaps, or compliance failures. For Security+, the security angle is the point: unmanaged change is a top cause of misconfiguration and breach. As an IAM analyst in a regulated bank, you live this — these are the items the exam tests.

### Business processes

| Element | What it is / why it matters |
|---|---|
| **Approval process** | Formal sign-off (often via a Change Advisory Board) before a change proceeds. Prevents unauthorized/risky changes. |
| **Ownership** | A named owner accountable for the change end-to-end. No owner = no accountability. |
| **Stakeholders** | Everyone affected/interested — owners, users, security, downstream teams — who must be identified and informed. |
| **Impact analysis** | Assessment of what the change could break, its security and operational consequences, and likelihood. |
| **Test results** | Evidence the change was validated (in a non-prod environment) before deployment. |
| **Backout plan** | The pre-defined procedure to roll back/restore if the change fails. Mandatory before you proceed. |
| **Maintenance window** | The scheduled, approved time block (usually low-traffic) when the change is applied to limit disruption. |
| **Standard operating procedure (SOP)** | The documented, repeatable steps for performing a recurring change consistently and safely. |

🎯 **Exam trap** — **Backout plan vs. maintenance window.** Backout plan = *how to undo it*. Maintenance window = *when you do it*. If the question is "the change failed and we must restore service" → backout plan.

⚡ **Keyword association** — "roll back / undo / restore if it fails" → backout plan. "scheduled low-traffic time" → maintenance window. "who must approve / CAB" → approval process. "what could break" → impact analysis.

### Technical implications

| Implication | What to watch for |
|---|---|
| **Allow lists / deny lists** | Changing what's explicitly permitted/blocked (apps, IPs, ports). A change can break access or, if sloppy, open holes. *Allow list = default deny, only listed is permitted (more secure). Deny list = default allow, only listed is blocked.* |
| **Restricted activities** | Some changes touch sensitive systems and require extra scope/approval limits. |
| **Downtime** | The change may make a service unavailable; must be planned and communicated. |
| **Service and application restart** | Many changes require restarting services/apps to take effect — itself a source of disruption. |
| **Legacy applications** | Old systems may not tolerate changes, lack support, or break with dependencies; handle with care (often need compensating controls). |
| **Dependencies** | Changing one component can cascade to others that rely on it; map them first. |

🎯 **Exam trap** — **Allow list vs. deny list** posture. An **allow list** is *default-deny* (only what's listed runs) — more secure but higher maintenance. A **deny list** is *default-allow* (only listed items blocked) — easier but weaker. CompTIA tests which is more secure: **allow list**.

### Documentation

| Item | Why |
|---|---|
| **Diagrams** | Network/architecture diagrams must be updated to reflect the change, or future troubleshooting and audits fail. |
| **Policies / procedures** | Governing documents and step-by-steps must stay current with reality. |

🎯 **Exam trap** — Out-of-date diagrams are a classic *aftermath* of poor change management. If a scenario describes responders working from a wrong/old network diagram, the root cause is **documentation not updated during change management**.

### Version control

Tracking and managing changes to files/code/configs over time so you can see what changed, who changed it, and revert to a known-good prior version. Critical for **infrastructure-as-code** and configuration management — and itself a backout mechanism.

⚡ **Keyword association** — "track changes over time / revert to a previous version / who changed what" → version control (think Git).

🔬 **Hands-on** — Treat your Conditional Access and IAM policy definitions as code: export Entra CA policies to JSON and commit them to a Git repo. Now every policy change is reviewable, attributable, and revertible — that's version control as a real backout plan, exactly the discipline 1.3 is testing.

---

## 1.4 Cryptographic solutions

The big section. Crypto recurs in [[Domain 2 — Threats, Vulnerabilities & Mitigations]] (attacks against it) and [[Domain 3 — Security Architecture]] (where it's deployed). Lock the vocabulary and the symmetric/asymmetric distinctions cold.

### PKI — Public Key Infrastructure

The framework of policies, hardware, software, and procedures that creates, manages, distributes, and revokes **digital certificates** and the **asymmetric key pairs** behind them.

| Term | Definition |
|---|---|
| **Public key** | Freely shared half of the pair. Used to *encrypt to* you and to *verify* your signatures. |
| **Private key** | Secret half, never shared. Used to *decrypt* what's sent to you and to *sign* (proving it's you). |
| **Key escrow** | A trusted third party holds a copy of private keys so data can be recovered if a key is lost. |

🎯 **Exam trap** — **Encrypt with public, decrypt with private** (for confidentiality). **Sign with private, verify with public** (for authenticity/non-repudiation). Flipping these is the most common crypto error on the exam. Public encrypts *to* you; only your private can open it. Your private *signs*; everyone's copy of your public verifies it.

🎯 **Exam trap** — **Key escrow** trades recoverability for risk — the escrow holder is a single point of compromise. If a scenario stresses *recovery of lost keys* → escrow. If it stresses *risk of a third party holding keys* → the downside of escrow.

### Encryption levels

You can encrypt at many scopes. Know what each protects.

| Level | Encrypts | Notes |
|---|---|---|
| **Full-disk (FDE)** | The entire drive, including OS. | BitLocker; protects data at rest if device is lost/stolen. |
| **Partition** | A specific partition. | Subset of FDE. |
| **Volume** | A logical volume (which may span disks). | Encrypted container/volume. |
| **File** | Individual files. | EFS; granular. |
| **Record** | Individual records within a database. | Fine-grained field/row protection. |
| **Database** | The database (TDE — transparent data encryption). | Protects DB at rest as a whole. |

**Transport / communication encryption** protects data **in transit** (TLS, IPsec, SSH) — distinct from the at-rest levels above.

⚡ **Keyword association** — "laptop stolen, data unreadable" → full-disk. "protect one column/field" → record-level. "data moving across the network" → transport/TLS.

🎯 **Exam trap** — **Full-disk encryption protects data at rest only.** Once the system is booted and unlocked, FDE does nothing against malware or an authenticated attacker. If the threat is "running system / authorized session," FDE is the wrong answer.

### Symmetric vs. asymmetric

| | **Symmetric** | **Asymmetric** |
|---|---|---|
| **Keys** | One shared secret key (encrypt = decrypt) | Key pair: public + private |
| **Speed** | Fast — good for bulk data | Slow — good for small data/key exchange |
| **Key distribution** | Hard (must share the secret securely) | Easy (publish the public key) |
| **Provides non-repudiation?** | No | Yes (private-key signatures) |
| **Algorithms** | AES, 3DES, ChaCha20, RC4, Blowfish | RSA, ECC, Diffie-Hellman, ElGamal, DSA |

**Key exchange** solves symmetric's distribution problem: use asymmetric crypto (e.g., Diffie-Hellman, RSA) to securely agree on/transport a symmetric **session key**, then switch to fast symmetric encryption for the bulk traffic. This *hybrid* approach is how TLS actually works.

**Algorithms & key length:** Security scales with key length, but you can't compare lengths *across* families — a 256-bit AES (symmetric) key is not "weaker" than a 2048-bit RSA (asymmetric) key; the math differs. ECC achieves equivalent strength to RSA at *much* shorter key lengths (a ~256-bit ECC key ≈ 3072-bit RSA), which is why ECC dominates mobile/IoT.

🎯 **Exam trap** — **AES is symmetric; RSA/ECC/DH are asymmetric.** Misclassifying AES as asymmetric is a frequent miss. Also: **Diffie-Hellman does key *exchange*, not bulk encryption** — it agrees on a shared secret, it doesn't encrypt your files.

🎯 **Exam trap** — Don't compare key *lengths* across symmetric and asymmetric. "Which is stronger, 256-bit AES or 2048-bit RSA?" is a category error the exam may bait you with — strength is family-specific.

🧠 **Mnemonic** — Asymmetric algorithms: **"Real Engineers Don't Encrypt Dumbly"** → **R**SA, **E**CC, **D**iffie-Hellman, **E**lGamal, **D**SA. Everything *not* on that list (AES, 3DES, ChaCha20, RC4, Blowfish) is symmetric.

⚡ **Keyword association** — "bulk / fast / large amount of data" → symmetric (AES). "key exchange / agree on a shared secret" → Diffie-Hellman. "small footprint / mobile / IoT / short keys" → ECC. "non-repudiation / signing" → asymmetric private key.

### Cryptographic tools

| Tool | What it is | Use |
|---|---|---|
| **TPM** (Trusted Platform Module) | A chip *on the motherboard* of a single device that stores keys and supports crypto operations. | Stores BitLocker keys, attests boot integrity, binds keys to *this* machine. |
| **HSM** (Hardware Security Module) | A dedicated, often network/rack appliance for generating, storing, and processing keys at scale, tamper-resistant. | Enterprise/CA key protection, high-volume crypto, FIPS-validated environments. |
| **Key management system (KMS)** | Software/service that handles the full key lifecycle — generation, distribution, rotation, revocation, storage. | Centralized key governance (e.g., Azure Key Vault, AWS KMS). |
| **Secure enclave** | An isolated, hardware-backed region of a processor for sensitive operations/keys, walled off from the main OS. | Apple Secure Enclave, biometric/key protection on the chip. |

🎯 **Exam trap** — **TPM vs. HSM.** TPM = built into **one** device's motherboard, protects *that* machine (think BitLocker). HSM = a **separate, scalable** appliance protecting keys for *many* systems/an enterprise. "Per-device / motherboard chip" → TPM. "Enterprise key appliance / high volume" → HSM.

🔬 **Hands-on** — On your home-lab Windows box, BitLocker stores its volume master key sealing material in the **TPM** so the drive auto-unlocks only on that hardware. Spin up an **Azure Key Vault** (a managed HSM-backed KMS) and store a secret — you've now touched TPM, KMS, and HSM-backed storage in one afternoon.

### Obfuscation

Making data hard to interpret without (necessarily) classic encryption.

| Technique | What it does |
|---|---|
| **Steganography** | Hides data *inside* other data (a message concealed in an image/audio/video) — hides the existence of the message. |
| **Tokenization** | Replaces sensitive data with a non-sensitive **token**; the real value is stored in a separate secure vault and mapped back. No mathematical relationship to the original. |
| **Data masking** | Obscures part of the data (e.g., showing only the last 4 of an SSN/PAN), often irreversibly for display. |

🎯 **Exam trap** — **Tokenization vs. encryption vs. masking.** Encryption is reversible *with a key* and the ciphertext is derived from the plaintext by an algorithm. **Tokenization** has *no algorithmic relationship* — the token is a random stand-in mapped in a vault (heavily used for PCI card data). **Masking** typically hides characters for display and is often *not* reversible. If PCI/credit-card scope-reduction is the theme → tokenization.

⚡ **Keyword association** — "hide that a message even exists / inside an image" → steganography. "replace card number with a random stand-in, vault holds the real one" → tokenization. "show only last four digits" → data masking.

### Hashing, salting, key stretching

**Hashing** — a one-way function producing a fixed-length **digest** from input. Same input → same hash; any change → wildly different hash. Used for **integrity verification** and password storage. *Not reversible.* Algorithms: SHA-256/SHA-3 (good), MD5/SHA-1 (broken — collisions).

**Salting** — adding a unique random value to each input *before* hashing. Defeats **rainbow tables** and ensures two identical passwords hash differently. Each user gets a unique salt.

**Key stretching** — deliberately making a key/password slow to compute by hashing it many thousands of times (PBKDF2, bcrypt, scrypt, Argon2). Makes brute-force/dictionary attacks computationally expensive.

🎯 **Exam trap** — **Hashing ≠ encryption.** Hashing is *one-way* (no key, no decryption) and serves **integrity**; encryption is *reversible* with a key and serves **confidentiality**. "Verify the file wasn't altered" → hashing. "Keep the file secret" → encryption.

🎯 **Exam trap** — **Salting vs. key stretching.** Salt defeats *precomputed* attacks (rainbow tables) by making each hash unique. Key stretching defeats *brute force* by making each guess slow. They're complementary, not the same; modern password storage uses both (bcrypt/Argon2 salt *and* stretch).

⚡ **Keyword association** — "rainbow table / two users same password different stored value" → salting. "slow down brute force / bcrypt / Argon2 / PBKDF2" → key stretching. "verify integrity / file unchanged" → hashing.

### Digital signatures

Produced by **hashing** the message, then **encrypting that hash with the signer's private key**. The recipient decrypts it with the signer's public key and re-hashes the message; a match proves three things at once: **integrity** (unaltered), **authentication** (it's from the claimed signer), and **non-repudiation** (only the private key holder could have signed).

🎯 **Exam trap** — A digital signature gives **integrity + authentication + non-repudiation but NOT confidentiality** — the message itself isn't hidden by the signature. If you also need secrecy, encrypt separately. CompTIA tests "what does a signature provide" — confidentiality is the wrong inclusion.

### Blockchain / open public ledger

A **distributed, append-only ledger** where each block is cryptographically chained to the previous (via hashes) and replicated across many nodes. Properties: decentralized, tamper-evident (altering one block breaks the chain and is rejected by consensus), and transparent. Underpins cryptocurrencies, but the exam cares about it as an **integrity/immutability** mechanism — an "open public ledger" anyone can verify.

⚡ **Keyword association** — "distributed / immutable / append-only / publicly verifiable ledger" → blockchain.

### Certificates

Digital certificates (X.509) bind a **public key** to an **identity**, vouched for by a trusted **Certificate Authority**.

| Term | Definition |
|---|---|
| **Certificate Authority (CA)** | Trusted entity that issues and signs certificates, vouching for the identity-to-key binding. |
| **CRL** (Certificate Revocation List) | A CA-published *list* of revoked certificates. Client downloads and checks it — can be large/stale. |
| **OCSP** (Online Certificate Status Protocol) | Real-time *query* to the CA for one certificate's status. Faster/fresher than a CRL. (OCSP *stapling* lets the server present the response to avoid each client querying the CA.) |
| **Self-signed** | A certificate signed by its own key, not a trusted CA. Free, fine for internal/testing, but not trusted by default — browsers warn. |
| **Third-party** | Issued by a publicly trusted CA (DigiCert, etc.). Trusted automatically by clients via the root store. |
| **Root of trust** | The anchor — the **root CA** certificate that clients inherently trust; trust chains down to issued certs through intermediate CAs. |
| **CSR** (Certificate Signing Request) | The request you generate (containing your public key + identity info) and submit to a CA to obtain a signed certificate. Your private key stays with you. |
| **Wildcard** | A single cert (e.g., `*.bank.com`) valid for *all* subdomains at one level. Convenient but a broader blast radius if the key is compromised. |

🎯 **Exam trap** — **CRL vs. OCSP.** CRL = a downloadable *list* (can be outdated, larger). OCSP = a *real-time single-cert query* (current, lighter). "Real-time status check" → OCSP. "Published list of revoked certs" → CRL.

🎯 **Exam trap** — **CSR generation keeps your private key private.** A CSR contains your *public* key and identity; you never send the private key to the CA. A scenario implying you "submit your private key to the CA" is wrong.

🎯 **Exam trap** — **Wildcard scope.** `*.bank.com` covers `mail.bank.com`, `vpn.bank.com`, etc., at one level — but **not** `bank.com` itself or multi-level subdomains, and compromising it exposes every subdomain. SAN certs (multiple explicit names) are the alternative.

⚡ **Keyword association** — "browser warns, untrusted, internal/test" → self-signed. "real-time revocation check" → OCSP. "covers all subdomains" → wildcard. "request sent to CA to get a cert" → CSR.

🔬 **Hands-on** — Stand up Active Directory Certificate Services (AD CS) in your Windows Server lab to be your own internal **CA**. Generate a **CSR** with `certreq` or OpenSSL, issue a cert, then revoke it and watch it land on the **CRL** / show as revoked via **OCSP**. Doing the full issue-and-revoke loop once makes every certificate term on this exam permanent.

---

## 🎯 Top exam traps

1. **Control category vs. type are two separate axes.** Every control has one category (technical/managerial/operational/physical) and one type (preventive/deterrent/detective/corrective/compensating/directive). The *same* control can be a different type depending on deployment.
2. **Managerial vs. operational:** deciding/planning vs. doing the recurring work. A security guard can be physical *or* operational — read the verb.
3. **Deterrent vs. preventive vs. directive:** deterrent works on the mind, preventive blocks the action, directive instructs required behavior. A visible camera deters; its recording detects.
4. **Corrective vs. compensating:** corrective fixes a specific incident's aftermath; compensating substitutes for a control you can't deploy.
5. **Symmetric provides no non-repudiation** — shared key means either party could've done it. Non-repudiation needs an asymmetric private key.
6. **RBAC = Role-Based on this exam**, not rule-based. Job function → role-based; condition applied to everyone → rule-based.
7. **Zero Trust: Policy Engine decides, Policy Administrator executes; the PEP lives in the data plane** even though it enforces a control-plane decision.
8. **Encrypt with public / decrypt with private; sign with private / verify with public.** Flipping these is the #1 crypto error.
9. **AES is symmetric; RSA/ECC/DH are asymmetric.** Diffie-Hellman does key *exchange*, not bulk encryption.
10. **Hashing ≠ encryption** (one-way, integrity vs. reversible, confidentiality). **Salting** beats rainbow tables; **key stretching** beats brute force — different jobs.
11. **Digital signatures give integrity + authentication + non-repudiation, NOT confidentiality.**
12. **TPM = one device's motherboard chip; HSM = enterprise key appliance.** **CRL = list; OCSP = real-time query.** **CSR sends your public key, never your private key.**
13. **Allow list = default-deny (more secure); deny list = default-allow.** Tokenization (random vaulted stand-in) ≠ encryption (algorithmic, key-reversible) ≠ masking (display hiding).
14. **Access control vestibule** is the SY0-701 term for a mantrap; its job is stopping **tailgating**.

## 🧠 Mnemonic pack

- **Control categories — "To Manage Our Property"**
  - **T**o → **T**echnical
  - **M**anage → **M**anagerial
  - **O**ur → **O**perational
  - **P**roperty → **P**hysical
  - ✅ Maps to Technical, Managerial, Operational, Physical — all four categories, correct.

- **Control types — "Please Don't Date Corrupt Computer Dorks"**
  - **P**lease → **P**reventive
  - **D**on't → **D**eterrent
  - **D**ate → **D**etective
  - **C**orrupt → **C**orrective
  - **C**omputer → **C**ompensating
  - **D**orks → **D**irective
  - ✅ Maps to Preventive, Deterrent, Detective, Corrective, Compensating, Directive — all six types, correct.

- **Zero Trust decision roles — "Engines Decide, Admins Do"**
  - **Engine** → Policy **E**ngine makes the **D**ecision (grant/deny verdict)
  - **Admin** → Policy **A**dministrator **D**oes it (establishes the session, issues the token)
  - ✅ Correctly separates the deciding component (PE) from the executing component (PA).

- **Asymmetric algorithms — "Real Engineers Don't Encrypt Dumbly"**
  - **R**eal → **R**SA
  - **E**ngineers → **E**CC
  - **D**on't → **D**iffie-Hellman
  - **E**ncrypt → **E**lGamal
  - **D**umbly → **D**SA
  - ✅ Maps to RSA, ECC, Diffie-Hellman, ElGamal, DSA — the asymmetric set. Anything not listed (AES, 3DES, ChaCha20, RC4, Blowfish) is symmetric.

## ✅ Self-check

1. A company posts a highly visible sign reading "Premises monitored by armed response." No camera is actually installed. What control **type** is the sign?
2. A legacy HVAC controller can't run the required endpoint agent, so the team isolates it on a dedicated VLAN with tight ACLs. What control **type** is the isolation, and what **category**?
3. Which CIA property is violated when an attacker alters transaction amounts in a database without authorization?
4. Why can't symmetric encryption alone provide non-repudiation?
5. A policy grants access based on the user's department, the file's classification label, and whether the request comes from a compliant device. Which access control model is this?
6. In Zero Trust, which component renders the grant/deny *decision*, and which one *establishes the session and issues the token*?
7. On which Zero Trust plane does the Policy Enforcement Point reside?
8. An auditor compares the bank's current control posture against the FFIEC-aligned target baseline and lists what's missing. What is this activity called?
9. You need to encrypt a message so that only Grace can read it. Whose key do you use, and is it public or private?
10. You need to prove a document came from you and wasn't altered. What cryptographic construct do you use, and which three properties does it provide?
11. Two users choose the same password but their stored hashes differ. What technique caused that, and what attack does it defeat?
12. A team needs per-device key storage tied to a single laptop's motherboard for BitLocker. Which hardware tool is correct — and which tool would you choose instead to protect keys for thousands of servers?
13. A merchant replaces stored card numbers with random values mapped back through a secure vault, removing the real PANs from scope. Encryption, tokenization, or masking?
14. A client needs to check, in real time, whether a single certificate has been revoked. CRL or OCSP?
15. What does a Certificate Signing Request contain, and what must it **never** include?

<details>
<summary>Answers</summary>

1. **Deterrent.** It discourages an attacker psychologically; there's no actual detection or blocking, so it isn't detective or preventive.
2. **Type: compensating** (it substitutes for the control the device can't run). **Category: technical** (VLAN/ACLs are system-enforced). The ACLs themselves act preventively.
3. **Integrity** — the data was modified without authorization.
4. Both parties share the *same* secret key, so either one could have produced the message; there's no way to attribute it to a single, uniquely held key. Non-repudiation requires an asymmetric private key only one party holds.
5. **ABAC** (attribute-based) — multiple attributes (department, classification, device posture) are evaluated together.
6. The **Policy Engine** renders the decision; the **Policy Administrator** establishes the session and issues credentials/tokens.
7. The **data plane** (it enforces the decision in the traffic path, even though the decision came from the control plane).
8. **Gap analysis** (current state vs. desired/target baseline).
9. Use **Grace's public key** to encrypt; only her matching private key can decrypt it.
10. A **digital signature**. It provides **integrity, authentication, and non-repudiation** (not confidentiality).
11. **Salting** (a unique random value per user). It defeats **rainbow tables / precomputed hash attacks**.
12. **TPM** for the single laptop (motherboard-bound key storage). For thousands of servers, use an **HSM** (scalable, tamper-resistant key appliance).
13. **Tokenization** — random stand-ins with no algorithmic relationship to the original, mapped in a vault (classic PCI scope reduction).
14. **OCSP** — real-time, single-certificate status query (CRL is a downloadable list that can be stale).
15. A CSR contains your **public key** and **identity information** (and is signed by your private key to prove possession). It must **never** include your **private key**.

</details>

⬅ [[Security+ SY0-701 — Course Index]] | ➡ Next: [[Domain 2 — Threats, Vulnerabilities & Mitigations]]
