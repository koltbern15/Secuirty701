> **Domain 4 — Security Operations**
> **Exam weight:** 28% (~25 questions on a 90-question form) — the single largest domain.
> **What it covers:** The day-to-day machinery of running security: hardening and baselines, asset and vulnerability management, monitoring and alerting, the enterprise control stack (firewalls, IDS/IPS, web/DNS/email filtering, EDR/XDR, DLP, NAC), the entire identity and access management lifecycle, automation and orchestration, incident response, and digital forensics.
> **Strategic note:** THE domain — 28%, more than a quarter of the exam, and it leans hard into your IAM wheelhouse. If you bank one domain cold, bank this one. It rewards depth: order of volatility, the IR phases in order, the IAM model distinctions (MAC/DAC/RBAC/ABAC), and the difference between every monitoring tool will all show up. You already live 4.6 at Richwood; the move here is to lock the rote distinctions CompTIA tests in 4.1–4.5 and 4.8–4.9, where the exam mines confusions, and let your operational experience carry the IAM and IR scenarios. This domain reuses everything from [[Domain 1 — General Security Concepts]] (control types, crypto, AAA) and [[Domain 3 — Security Architecture]] (where the controls live).

## Objective map

- **4.1** Secure computing resources — baselines, hardening targets, wireless install, mobile solutions, wireless security, application security, sandboxing, monitoring.
- **4.2** Asset management — acquisition/procurement, assignment/accounting, monitoring/tracking, disposal/decommissioning.
- **4.3** Vulnerability management — identification, analysis (CVSS/CVE), response & remediation, validation, reporting.
- **4.4** Alerting & monitoring — resources, activities (log aggregation, alerting, tuning), tools (SIEM, SCAP, NetFlow, SNMP, DLP).
- **4.5** Modify enterprise capabilities — firewall, IDS/IPS, web filter, OS security, secure protocols, DNS filtering, email security, FIM, DLP, NAC, EDR/XDR, UBA.
- **4.6** Identity & access management — provisioning, permissions, identity proofing, federation, SSO, attestation, access control models, MFA, password concepts, PAM.
- **4.7** Automation & orchestration — use cases, benefits, considerations.
- **4.8** Incident response — IR process, training, testing, RCA, threat hunting, digital forensics (and order of volatility).
- **4.9** Investigation data sources — log data and other sources.

---

## 4.1 Secure computing resources

This objective is the hardening half of operations: take a system from "default and exploitable" to "baselined and locked down," then keep it there.

### Secure baselines

A **secure baseline** is the documented, approved minimum security configuration for a given system type — the standard you measure every instance against. The exam tests three lifecycle verbs:

| Phase | What it means |
|---|---|
| **Establish** | Define the baseline — what settings, services, and controls a system *must* have (often derived from CIS Benchmarks, DISA STIGs, or vendor guidance). |
| **Deploy** | Push the baseline onto systems consistently — via imaging, Group Policy, Intune configuration profiles, Ansible, etc. |
| **Maintain** | Keep systems *at* the baseline over time — detect and remediate drift, update the baseline as threats evolve, re-deploy as needed. |

⚡ **Keyword association** — "minimum required configuration / standard settings every system must meet" → secure baseline. "systems have drifted from the standard over time" → maintain phase (configuration drift).

🎯 **Exam trap** — A **baseline** is the *standard you compare against*; **hardening** is the *act of reducing attack surface*. You harden a system *to* its baseline. Don't treat them as synonyms.

### Hardening targets

**Hardening** is reducing a system's attack surface — disabling unneeded services and ports, removing default accounts, applying least privilege, patching, and tightening configuration. Each target type has its own emphasis:

| Target | Hardening emphasis |
|---|---|
| **Mobile devices** | MDM enrollment, full-device encryption, screen lock/biometrics, remote wipe, app vetting, disabling sideloading. |
| **Workstations** | Endpoint protection, patching, least-privilege local accounts, disk encryption, host firewall, Group Policy/Intune config. |
| **Switches** | Disable unused ports, port security (MAC limiting), disable Telnet for SSH, VLAN segmentation, change default creds, disable unused services. |
| **Routers** | Strong admin auth, ACLs, disable source routing, secure management plane (SSH not Telnet), patch firmware, disable unused interfaces. |
| **Cloud infrastructure** | Least-privilege IAM roles, no public storage buckets, network security groups, logging/CloudTrail on, CIS cloud benchmarks, key management. |
| **Servers** | Remove unneeded roles/services, patch, host-based firewall, least privilege, FIM, secure remote admin, baseline configuration. |
| **ICS/SCADA** | Industrial control systems — *prioritize availability and safety*; segment from IT networks, restrict remote access, patch cautiously (uptime-critical). |
| **Embedded systems** | Purpose-built computers inside larger devices; limited update paths — segment, restrict physical/network access, vendor firmware updates. |
| **RTOS** | Real-Time Operating System — runs time-critical tasks (medical, aerospace, industrial) where timing guarantees matter; minimal footprint, hard to patch, isolate. |
| **IoT devices** | Internet-of-Things devices — change default creds, segment onto isolated VLANs, disable UPnP, keep firmware current; notoriously weak by default. |

🎯 **Exam trap** — **ICS/SCADA and RTOS prioritize availability over confidentiality.** In a bank or office, CIA usually leads with confidentiality. In industrial/embedded/real-time contexts, *availability and safety* dominate — you can't take a turbine controller down for a patch reboot mid-operation. If the scenario is "manufacturing plant / power grid / can't have downtime," the answer leans toward segmentation and compensating controls rather than aggressive patching.

⚡ **Keyword association** — "factory / utility / physical process control" → ICS/SCADA. "tiny computer inside a device, can't easily update" → embedded. "timing-critical, deterministic" → RTOS. "smart thermostat / camera / default password" → IoT.

📌 **Scenario pattern** — "A water-treatment facility's control system can't be taken offline to patch a known flaw." → Don't patch blindly; **segment** it from the corporate network and apply **compensating controls** (this is straight from [[Domain 1 — General Security Concepts]] control types).

### Wireless installation considerations

| Concept | What it is |
|---|---|
| **Site survey** | A physical walk-through measuring existing RF, interference, and coverage needs *before* deploying APs — determines AP placement, channel plan, and power levels. |
| **Heat map** | A visual overlay of wireless signal strength across the floor plan, output from the survey — shows coverage and dead zones. |

⚡ **Keyword association** — "where do we place access points / measure signal before install" → site survey. "color-coded coverage map / signal strength visualization" → heat map.

### Mobile solutions

**MDM (Mobile Device Management)** is the platform that enrolls, configures, secures, and (when needed) wipes mobile devices — pushing policy, enforcing encryption, and separating corporate from personal data. **MAM (Mobile Application Management)** is the narrower cousin that manages just the *corporate apps and their data* on a device, common in BYOD.

**Deployment models:**

| Model | Who owns it | Who picks it | Trade-off |
|---|---|---|---|
| **BYOD** (Bring Your Own Device) | Employee | Employee | Cheapest for the company; hardest to secure — personal device, privacy friction, mixed data. |
| **COPE** (Corporate-Owned, Personally Enabled) | Company | Company | Company owns and controls it; employee may use it personally. Strong control, higher cost. |
| **CYOD** (Choose Your Own Device) | Company | Employee (from a list) | Employee picks from approved devices the company buys/manages — balance of choice and control. |

🎯 **Exam trap** — **BYOD vs. COPE vs. CYOD** by *ownership and choice*. BYOD = employee owns. COPE = company owns, employee uses personally. CYOD = company owns, employee chose from an approved list. The hardest to secure and the privacy minefield is always **BYOD**.

**Connection methods:**

| Method | Note |
|---|---|
| **Cellular** | Carrier network (LTE/5G); broad coverage, harder to intercept locally, but data leaves your control. |
| **Wi-Fi** | Local wireless; secure it with WPA3 — the main wireless attack surface. |
| **Bluetooth** | Short-range device pairing; watch for bluejacking/bluesnarfing, disable when unused. |

### Wireless security settings

| Setting | What it is |
|---|---|
| **WPA3** | The current Wi-Fi security standard. Uses **SAE (Simultaneous Authentication of Equals)** to replace WPA2's pre-shared-key handshake, defeating offline dictionary attacks; adds forward secrecy. |
| **AAA / RADIUS** | Enterprise Wi-Fi auth: each user authenticates to a **RADIUS** server (the AAA back-end) rather than sharing one PSK — this is **WPA3-Enterprise / 802.1X**. |
| **Cryptographic protocols** | The cipher suites protecting the traffic (WPA3 uses AES-based encryption; SAE for the handshake). |
| **Authentication protocols** | The methods inside 802.1X — **EAP** family: EAP-TLS (cert-based, strongest), PEAP, EAP-TTLS, EAP-FAST. |

🎯 **Exam trap** — **WPA3-Personal (SAE/PSK) vs. WPA3-Enterprise (802.1X/RADIUS).** Personal uses a shared passphrase improved by SAE. Enterprise authenticates *each user individually* against RADIUS — that's the answer when the scenario wants per-user accountability or central auth. "Everyone shares the Wi-Fi password" → Personal. "Each employee logs in with their own credentials" → Enterprise/802.1X/RADIUS.

⚡ **Keyword association** — "SAE / dragonfly handshake / replaced PSK weakness" → WPA3. "certificate-based wireless auth" → EAP-TLS. "central auth server for Wi-Fi" → RADIUS.

🔬 **Hands-on** — In your home lab, stand up a RADIUS server (Windows Server NPS role) and point a test AP at it for WPA3-Enterprise. Map an Entra ID group to the NPS policy and you've built per-user 802.1X wireless auth — the exact enterprise pattern the exam describes, using your bank's identity stack.

### Application security

| Technique | What it does |
|---|---|
| **Input validation** | Checking all input against expected format/length/type before processing — the primary defense against injection and XSS (see [[Domain 2 — Threats, Vulnerabilities & Mitigations]]). |
| **Secure cookies** | Cookies flagged `Secure` (HTTPS-only) and `HttpOnly` (no JavaScript access) so session tokens can't be stolen over cleartext or via XSS. |
| **Static code analysis (SAST)** | Examining source code *without running it* for flaws — runs early in development, catches issues before deployment. |
| **Dynamic code analysis (DAST)** | Testing the *running* application (often with fuzzing — feeding malformed input) to find runtime flaws. |
| **Code signing** | Digitally signing software with the developer's private key so users can verify integrity and origin (ties to digital signatures in [[Domain 1 — General Security Concepts]]). |

🎯 **Exam trap** — **Static (SAST) vs. dynamic (DAST) analysis.** Static reads the *source code without executing* it (white-box, early). Dynamic tests the *running application* (black-box, fuzzing). "Analyze code without running it" → static. "Test the live app / fuzzing" → dynamic.

⚡ **Keyword association** — "verify software hasn't been tampered with / publisher is genuine" → code signing. "check input before processing" → input validation. "HttpOnly / Secure flag" → secure cookies.

### Sandboxing and monitoring

**Sandboxing** — running code or opening files in an isolated environment walled off from the host, so malicious behavior can't escape. Used for malware analysis, untrusted attachments (detonation), and testing. **Monitoring** here means watching deployed resources for security-relevant events — the bridge into 4.4.

⚡ **Keyword association** — "detonate a suspicious attachment safely / isolated execution to observe behavior" → sandboxing.

---

## 4.2 Asset management

Managing assets across their whole lifecycle — you can't protect what you don't know you have.

| Phase | What it covers |
|---|---|
| **Acquisition / procurement** | The intake process for new assets — vetting suppliers, ensuring devices enter inventory and get baselined from day one. Supply-chain risk lives here. |
| **Assignment / accounting** | **Ownership** (a named person/team accountable for each asset) and **classification** (labeling by sensitivity — public/internal/confidential/restricted). |
| **Monitoring / asset tracking** | Maintaining the **inventory** (the authoritative asset list) through **enumeration** (actively discovering what's on the network). |
| **Disposal / decommissioning** | Securely retiring assets — see below. The most heavily tested part. |

### Disposal and decommissioning

| Concept | Definition |
|---|---|
| **Sanitization** | Removing data so it can't be recovered, while the media may be reused (overwriting, cryptographic erase, degaussing). |
| **Destruction** | Physically rendering the media unusable — shredding, incineration, pulverizing, drilling. The most thorough. |
| **Certification** | Documented proof that sanitization/destruction was completed (certificate of destruction) — your audit evidence. |
| **Data retention** | Policy defining how long data must be kept (and when it must be destroyed) — driven by regulation. Banks live this (FFIEC/record-retention rules). |

🎯 **Exam trap** — **Sanitization vs. destruction.** Sanitization *can leave reusable media* (wipe and redeploy the drive); destruction *physically annihilates* it (the drive is gone). If the scenario says "reuse the drives internally" → sanitize. "Drives leaving the org / highly sensitive / no reuse" → destroy. **Degaussing** sanitizes magnetic media (HDDs, tape) but does *nothing* to SSDs/flash — that's a classic distractor.

🎯 **Exam trap** — **Certification** is the *proof*, not the act. If the question asks what gives you defensible evidence for an audit that data was destroyed → certification (certificate of destruction), not the shredding itself.

⚡ **Keyword association** — "wipe and reuse" → sanitization. "shred / incinerate / physically gone" → destruction. "certificate of destruction / proof for the auditor" → certification. "how long do we keep records" → data retention.

🔬 **Hands-on** — When you decommission a lab drive, run `cipher /w:` (Windows secure-wipe) on an HDD versus issuing a manufacturer secure-erase (cryptographic erase) on an SSD. Seeing why degaussing/overwriting logic differs between spinning rust and flash makes the sanitization distractors obvious on the exam.

---

## 4.3 Vulnerability management

The continuous cycle: find vulnerabilities, judge them, fix them, prove the fix, report. This whole objective is a process loop — know the order.

### Identification

| Method | What it is |
|---|---|
| **Vulnerability scan** | Automated probing of systems against a database of known flaws (Nessus, Qualys, OpenVAS). Credentialed scans see more (authenticated); non-credentialed see the attacker's view. |
| **Static analysis** | Inspecting source code for flaws without running it (SAST). |
| **Dynamic analysis** | Testing the running application for flaws (DAST). |
| **Package monitoring** | Watching third-party libraries/dependencies for known vulnerabilities (software composition analysis) — critical given supply-chain risk. |
| **Penetration testing** | Authorized simulated attack that *exploits* vulnerabilities to prove real-world impact — goes beyond scanning. |
| **System / process audit** | Formal review of configurations and procedures against policy/standards to surface gaps. |

**Threat feeds** — external intelligence sources that tell you what's being exploited in the wild:

| Source | What it is |
|---|---|
| **OSINT** | Open-Source Intelligence — publicly available info (vendor advisories, security blogs, public CVE data). Free, broad, lower fidelity. |
| **Proprietary / third-party** | Paid commercial threat intelligence (Mandiant, Recorded Future) — curated, higher fidelity. |
| **Information-sharing organizations** | Industry groups (ISACs — e.g., **FS-ISAC** for financial services) where members share threat data. |
| **Dark web** | Monitoring criminal forums/markets for leaked credentials, planned attacks, or your data being sold. |

**Responsible disclosure & bug bounty** — responsible disclosure is the practice of privately reporting a flaw to the vendor and giving them time to fix it before going public; a **bug bounty** is a program that *pays* external researchers for finding and reporting vulnerabilities under those rules.

🎯 **Exam trap** — **Vulnerability scan vs. penetration test.** A scan *identifies* potential flaws (passive, automated, no exploitation). A pen test *exploits* them to prove impact (active, often manual). "List of potential weaknesses" → scan. "Actually breached to demonstrate damage" → pen test.

⚡ **Keyword association** — "free, public, vendor advisories" → OSINT. "financial-sector threat sharing" → FS-ISAC / information-sharing org. "pay researchers to find bugs" → bug bounty. "monitor library dependencies" → package monitoring.

### Analysis

Once you have findings, you judge them.

| Concept | Definition |
|---|---|
| **False positive** | The scanner reports a vulnerability that *isn't* really there — wastes effort, erodes trust. |
| **False negative** | The scanner *misses* a vulnerability that *is* there — the dangerous one; you think you're safe. |
| **Prioritization** | Ranking findings so you fix what matters most first — driven by CVSS plus environmental/business context. |
| **CVE** | **Common Vulnerabilities and Exposures** — the unique *identifier* for a specific known vulnerability (e.g., CVE-2024-1234). It names the flaw. |
| **CVSS** | **Common Vulnerability Scoring System** — the 0–10 *severity score* for a vulnerability. It rates the flaw. |
| **Classification** | Categorizing the vulnerability/asset by type and sensitivity. |
| **Exposure factor** | The *percentage of an asset's value lost* if the vulnerability is exploited — a quantitative risk input. |
| **Environmental variables** | How *your* specific environment changes the real risk (is the box internet-facing? compensating controls present?) — CVSS Environmental metrics. |
| **Industry / organizational impact** | What the flaw means for *your* business and sector specifically (a flaw in core banking software hits a bank harder than a marketing site). |
| **Risk tolerance** | How much risk the organization is willing to accept — sets the threshold for what must be fixed vs. accepted. |

🎯 **Exam trap** — **CVE vs. CVSS.** CVE = the **name/ID** of the vulnerability. CVSS = the **severity score** (0–10). "Which identifier refers to the specific flaw?" → CVE. "Which gives the severity rating?" → CVSS. They almost always appear together as a trap pair.

🎯 **Exam trap** — **False positive vs. false negative.** False *positive* = false *alarm* (reported but not real). False *negative* = *missed* threat (real but not reported). The false negative is the more dangerous one because it creates blind confidence. (Same logic recurs in 4.4 alert tuning and in IDS/IPS.)

⚡ **Keyword association** — "named identifier CVE-####" → CVE. "0 to 10 score / critical-high-medium-low" → CVSS. "scanner cried wolf" → false positive. "scanner missed it" → false negative. "% of value lost" → exposure factor.

### Response & remediation

| Option | When it's used |
|---|---|
| **Patching** | Applying the vendor fix — the preferred remediation when available. |
| **Insurance** | Transferring residual financial risk (cyber insurance) — you *fund* the impact rather than eliminate the flaw. |
| **Segmentation** | Isolating the vulnerable system so the flaw can't be reached/spread — when you can't patch. |
| **Compensating controls** | Alternative safeguards that reduce risk when the primary fix isn't feasible (ties to [[Domain 1 — General Security Concepts]]). |
| **Exceptions / exemptions** | Formally documented, approved decisions to *not* remediate (accept the risk) — must be tracked and time-bound. |

🎯 **Exam trap** — **Patching vs. compensating control vs. exception.** Patch = fix it. Compensating control = mitigate it another way because you can't patch. Exception/exemption = formally *accept* it and document the acceptance. "Legacy app can't be patched, so we isolate it" → compensating control + segmentation. "Business signs off on living with the risk" → exception.

### Validation and reporting

| Step | What it confirms |
|---|---|
| **Rescanning** | Re-run the scan after remediation to confirm the finding is gone. |
| **Audit** | Independent review verifying remediation actually happened and holds. |
| **Verification** | Confirming the fix is effective and the vulnerability is genuinely closed. |
| **Reporting** | Communicating findings, remediation status, and risk to stakeholders/leadership — the output that drives decisions and proves compliance. |

⚡ **Keyword association** — "run the scan again to prove it's fixed" → rescanning. "tell leadership / metrics on remediation" → reporting.

---

## 4.4 Alerting & monitoring

The detection backbone — what you watch, what you do with the signals, and the tools that produce them.

### Monitoring resources

You monitor three layers: **systems** (servers, endpoints, OS health and security events), **applications** (app logs, performance, errors), and **infrastructure** (network devices, firewalls, traffic flows). Comprehensive monitoring covers all three.

### Activities

| Activity | What it is |
|---|---|
| **Log aggregation** | Collecting logs from many sources into one place (the SIEM) for correlation — you can't correlate what's scattered. |
| **Alerting** | Generating notifications when monitored conditions cross a threshold or match a rule. |
| **Scanning** | Actively probing for vulnerabilities/changes as an ongoing activity. |
| **Reporting** | Summarizing monitoring data for stakeholders and compliance. |
| **Archiving** | Retaining logs long-term for compliance and later investigation (immutable, often required for years in banking). |
| **Alert response & remediation** | What you *do* when an alert fires — including **quarantine** (isolating an affected host/file) and the validation that follows. |
| **Alert tuning** | Adjusting alert rules/thresholds to cut noise — reducing false positives without creating false negatives. |

🎯 **Exam trap** — **Alert tuning is about reducing false positives** (noise/alert fatigue) — but tune too aggressively and you create **false negatives** (missed real attacks). The exam wants the balance: tuning improves signal-to-noise without going blind. If a SOC is "drowning in alerts," the answer is alert tuning, not buying more tools.

⚡ **Keyword association** — "collect logs centrally for correlation" → log aggregation. "too many alerts / alert fatigue" → alert tuning. "isolate the infected host" → quarantine. "keep logs for years for compliance" → archiving.

### Tools

| Tool | What it does |
|---|---|
| **SIEM** | **Security Information and Event Management** — aggregates, correlates, and alerts on logs from across the enterprise. The central nervous system of the SOC. |
| **SCAP** | **Security Content Automation Protocol** — a standard that lets tools express and check security configuration/vulnerability state automatically (machine-readable compliance). |
| **Benchmarks** | Consensus secure-configuration standards (CIS Benchmarks, DISA STIGs) used to define and check baselines. |
| **Agents / agentless** | **Agent-based** = software installed on each host reporting in (deeper visibility, more overhead). **Agentless** = monitored remotely without installed software (lighter, less depth). |
| **Antivirus** | Signature/heuristic detection of known malware on endpoints. |
| **DLP** | **Data Loss Prevention** — detects and blocks sensitive data leaving the org (covered in depth in 4.5). |
| **SNMP traps** | **Simple Network Management Protocol** — network devices *push* alerts ("traps") to a manager when an event occurs, rather than waiting to be polled. |
| **NetFlow** | A protocol that records *metadata about network flows* (who talked to whom, ports, bytes, duration) — not packet contents. Great for traffic analysis and anomaly detection. |
| **Vulnerability scanners** | Tools that find known flaws (Nessus, Qualys) — feed 4.3. |

🎯 **Exam trap** — **NetFlow vs. packet capture (PCAP).** NetFlow records *flow metadata* (source/dest/port/volume) — lightweight, great for "who talked to whom." A packet capture records the *actual packet payloads* — heavyweight, full content. "Need to see traffic patterns/volumes without storing payloads" → NetFlow. "Need the actual data in the packets" → full packet capture (4.9).

🎯 **Exam trap** — **SNMP traps are pushed by the device** (asynchronous, event-driven); SNMP *polling* is the manager asking. If the scenario says the device "notified the manager when it failed," that's a **trap**.

🔬 **Hands-on** — Forward your Windows lab's Security event log and Sysmon to a free SIEM (Wazuh or a Microsoft Sentinel trial). Build one correlation rule — say, "5 failed logons then a success" — and watch it fire. You've now built log aggregation, alerting, and a tuning loop, which is exactly what 4.4 describes and what you'd recognize from M365 Defender.

---

## 4.5 Modify enterprise capabilities

The control stack you tune to enforce security. Know what each box does and how the pairs (IDS/IPS, EDR/XDR, SPF/DKIM/DMARC) differ.

### Firewall

| Concept | What it is |
|---|---|
| **Rules** | The ordered list of permit/deny statements the firewall evaluates top-down; most specific first, usually an implicit deny at the end. |
| **Access lists (ACLs)** | The permit/deny entries themselves, matched on source/dest IP, port, protocol. |
| **Ports / protocols** | What the rules filter on — you allow/deny specific services by port (e.g., 443 HTTPS) and protocol (TCP/UDP). |
| **Screened subnet** | The modern term for a **DMZ** — a buffer network between the internet and the internal LAN where public-facing services (web, mail) live, isolated by firewalls. |

🎯 **Exam trap** — **Screened subnet = DMZ.** SY0-701 uses "screened subnet" for what older material calls a demilitarized zone. Its purpose is hosting internet-facing services without exposing the internal network. (See [[Domain 3 — Security Architecture]] for placement.)

⚡ **Keyword association** — "buffer network for public-facing servers" → screened subnet/DMZ. "implicit deny at the end" → firewall rule order.

### IDS / IPS

| | **IDS** | **IPS** |
|---|---|---|
| **Full name** | Intrusion Detection System | Intrusion Prevention System |
| **Action** | Detects and *alerts only* | Detects and *blocks/drops* in-line |
| **Placement** | Out-of-band (sees a copy of traffic) | In-line (traffic flows through it) |
| **Risk** | Misses nothing but stops nothing | Can block legitimate traffic (false positive = outage) |

**Detection methods:**

| Method | How it detects |
|---|---|
| **Signature-based** | Matches traffic against known attack patterns — catches known threats, misses novel ones (zero-days). |
| **Trend / anomaly-based** | Builds a baseline of normal and flags deviations — can catch unknown attacks but generates more false positives. |

🎯 **Exam trap** — **IDS detects (alerts), IPS prevents (blocks).** The IPS sits *in-line* so it can drop traffic; the IDS sits *out-of-band* and only alarms. "Notified us but the attack still got through" → IDS. "Automatically blocked the attack" → IPS.

🎯 **Exam trap** — **Signature-based misses zero-days; anomaly-based catches novel attacks but cries wolf.** "Detected a brand-new attack with no known signature" → anomaly/behavior-based. "Matched a known malware pattern" → signature.

### Web filter

| Concept | What it is |
|---|---|
| **Agent-based** | Filtering software on each endpoint — follows the device off-network. |
| **Centralized proxy** | Traffic routed through a central proxy that filters — easier to manage, only protects on-network (unless tunneled). |
| **URL scanning** | Inspecting requested URLs against policy/threat data before allowing. |
| **Content categorization** | Classifying sites by category (gambling, social media, malware) to allow/block by class. |
| **Block rules** | Explicit allow/deny entries for specific sites/categories. |
| **Reputation** | Allowing/blocking based on a site's/IP's historical trustworthiness score. |

### Operating system security

| Tool | What it is |
|---|---|
| **Group Policy** | Windows mechanism to centrally enforce security settings, restrictions, and configuration across domain-joined machines — your primary Windows hardening lever. |
| **SELinux** | Security-Enhanced Linux — a **mandatory access control (MAC)** implementation for Linux that confines processes to least privilege via labels, even overriding root. |

🎯 **Exam trap** — **SELinux is MAC on Linux.** When a Linux question references enforcing mandatory, label-based access control that even constrains root, the answer is SELinux. Group Policy is the Windows equivalent lever (though policy-based, not strictly MAC).

🔬 **Hands-on** — In your Windows Server lab, build a GPO that disables legacy SMBv1 and enforces a screen-lock timeout, then link it to an OU. That's OS security via Group Policy — the same muscle you use for Entra/Intune configuration profiles, just on-prem.

### Secure protocol implementation

Choosing the *secure* version of a protocol, the right port, and the right transport. This is the "use the encrypted variant" objective — it pairs tightly with the protocol table you should have memorized from [[Domain 3 — Security Architecture]].

| Insecure | Secure replacement | Secure port |
|---|---|---|
| FTP (20/21) | SFTP (SSH) / FTPS (TLS) | 22 / 990 |
| Telnet (23) | SSH | 22 |
| HTTP (80) | HTTPS (TLS) | 443 |
| SMTP (25) | SMTP+TLS / SMTPS | 587 / 465 |
| LDAP (389) | LDAPS | 636 |
| SNMP v1/v2 | SNMPv3 | 161/162 |
| DNS (53) | DNS over TLS / HTTPS (DoT/DoH) | 853 / 443 |

⚡ **Keyword association** — "encrypt the management session" → SSH (replaces Telnet). "secure directory queries" → LDAPS (636). "secure DNS" → DoT/DoH.

### DNS filtering

Blocking or redirecting DNS resolution of known-malicious domains — when a host tries to resolve a phishing/malware/C2 domain, the resolver returns nothing or a sinkhole. A cheap, high-leverage control that stops connections before they're made.

⚡ **Keyword association** — "block resolution of malicious domains / sinkhole bad domains" → DNS filtering.

### Email security

The trio that fights spoofing — memorize what each one does and their order of layering.

| Mechanism | What it does |
|---|---|
| **SPF** (Sender Policy Framework) | A DNS record listing which mail servers are *authorized to send* for your domain. Receiver checks the sending IP against it. |
| **DKIM** (DomainKeys Identified Mail) | The sender *cryptographically signs* outgoing messages; the receiver verifies the signature via a DNS-published public key — proves the message wasn't altered and came from the domain. |
| **DMARC** (Domain-based Message Authentication, Reporting & Conformance) | A DNS policy that ties SPF and DKIM together, tells receivers *what to do* when checks fail (none/quarantine/reject), and *reports* back to the domain owner. |
| **Email gateway** | The appliance/service that filters inbound/outbound mail — spam, malware, phishing, DLP — and enforces these checks. |

🎯 **Exam trap** — **SPF vs. DKIM vs. DMARC.** SPF = authorized **IPs/servers** (who may send). DKIM = cryptographic **signature** (integrity + origin). DMARC = the **policy + reporting** that decides what to do on failure and ties the other two together. The mnemonic in the pack maps these. If the scenario asks "what enforces the action when SPF/DKIM fail and reports it?" → **DMARC**.

⚡ **Keyword association** — "list of allowed sending servers" → SPF. "signs the email" → DKIM. "policy that says reject/quarantine on failure + reports" → DMARC.

### File integrity monitoring (FIM)

Monitoring critical files/configs and alerting when they change unexpectedly — detects tampering, malware, and unauthorized modification by comparing against a known-good hash baseline. Required by PCI DSS; you'll recognize it from change-control discipline.

⚡ **Keyword association** — "alert when a critical system file is modified / hash changed" → FIM.

### DLP

**Data Loss Prevention** detects and blocks sensitive data (PII, PCI, PHI, IP) from leaving the organization through monitored channels. It works on data **in use** (endpoint), **in motion** (network/email), and **at rest** (storage scanning), matching content against policies (patterns like SSNs/card numbers, fingerprints, classifications).

🎯 **Exam trap** — **DLP stops data exfiltration; it's about data leaving, not getting in.** If the scenario is "an employee tried to email a spreadsheet of customer SSNs externally and it was blocked," that's **DLP**. Don't confuse it with a firewall (network access) or NAC (device admission).

🔬 **Hands-on** — Turn on a Microsoft Purview DLP policy in M365 that blocks emails containing credit-card patterns to external recipients, then test it from a lab mailbox. You've implemented content-based DLP on data in motion — the exact control 4.5 describes and a direct extension of your M365 work.

### Network access control (NAC)

Controls which devices are *admitted to the network* based on their identity and posture (patched? AV running? compliant?). Non-compliant devices are quarantined, denied, or shunted to a remediation VLAN. Often built on **802.1X**.

🎯 **Exam trap** — **NAC controls device admission to the network** (posture/health check at the door). DLP controls *data leaving*. Firewall controls *traffic between zones*. "Device must pass a health check before joining the LAN" → NAC.

### EDR / XDR

| | **EDR** | **XDR** |
|---|---|---|
| **Full name** | Endpoint Detection and Response | Extended Detection and Response |
| **Scope** | Endpoints (hosts) — detects, investigates, and responds to threats on devices | Correlates across *multiple* domains — endpoints, network, email, cloud, identity |
| **Think** | One layer (endpoints) | Many layers stitched together |

🎯 **Exam trap** — **EDR is endpoint-only; XDR extends across multiple telemetry sources** (endpoint + network + email + cloud + identity). "Detect and respond on the host" → EDR. "Correlate threats across endpoint, email, and cloud in one platform" → XDR (think Microsoft Defender XDR).

### User behavior analytics (UBA / UEBA)

Establishing a baseline of normal user (and entity) behavior and flagging anomalies — impossible-travel logins, mass downloads, off-hours access — to catch compromised accounts and insider threats that signature tools miss.

⚡ **Keyword association** — "impossible travel / account suddenly behaving abnormally / insider anomaly" → UBA/UEBA.

---

## 4.6 Identity & access management

Your wheelhouse, and the densest IAM coverage on the exam. The distinctions here (the access models, the MFA factors, PAM mechanisms) are heavily tested — lock them.

### Provisioning and de-provisioning accounts

**Provisioning** is creating an account and granting the access a user needs (joiner). **De-provisioning** is disabling/removing access when it's no longer needed (leaver). The exam's emphasis: **de-provisioning must be timely** — orphaned accounts from terminated employees are a top breach vector. The full lifecycle is **JML — Joiner / Mover / Leaver**.

🎯 **Exam trap** — **Disable, don't immediately delete, on termination.** Best practice is to *disable* a departing user's account first (preserving data, logs, and ownership for investigation/forensics) rather than deleting it outright. Immediate deletion destroys evidence and breaks ownership chains.

### Permission assignments and implications

Assigning the rights a subject has to resources. The implication CompTIA cares about is **privilege creep / accumulation** — as users change roles (the "mover" in JML), they accumulate access they no longer need unless old permissions are revoked. The corrective is regular **attestation/recertification** and **least privilege**.

⚡ **Keyword association** — "user changed roles and kept old access" → privilege creep → fixed by recertification + least privilege.

### Identity proofing

Verifying that a person *is who they claim to be* before issuing them an identity/credential — resolving the claimed identity, validating evidence (documents), and verifying it belongs to them. This is the onboarding "are you really this person" step, distinct from authentication (proving it each login afterward).

🎯 **Exam trap** — **Identity proofing (onboarding, one-time) vs. authentication (every login).** Proofing happens *once* when you establish the identity ("prove you're Kolton before we issue the account"). Authentication happens *every time* the account is used. Don't conflate them.

### Federation

Letting users authenticate in *their own* organization/identity provider and access resources in *another* organization without a separate account — trust is established between identity providers. SAML and OIDC are the common protocols. This is how you log into a third-party SaaS with your Entra ID credentials.

⚡ **Keyword association** — "use my home org's credentials to access a partner's app / trust between IdPs" → federation.

### SSO and its protocols

**Single Sign-On** lets a user authenticate *once* and access multiple applications without re-authenticating to each.

| Protocol | What it is / role |
|---|---|
| **LDAP** | Lightweight Directory Access Protocol — the protocol for *querying and authenticating against a directory* (Active Directory). The directory back-end, not an SSO/federation protocol per se. |
| **SAML** | Security Assertion Markup Language — XML-based; exchanges authentication/authorization **assertions** between an IdP and a service provider. The classic *enterprise web SSO / federation* protocol. |
| **OAuth** | An **authorization** framework — delegates access (grants an app a token to act on your behalf) without sharing your password. *Authorization, not authentication.* **OIDC** is the authentication layer built on top of OAuth. |

🎯 **Exam trap** — **SAML vs. OAuth.** SAML = primarily **authentication/SSO** via XML assertions, enterprise/web-app focused. OAuth = **authorization/delegation** (API access via tokens), modern/mobile/API focused. "Enterprise web SSO with assertions" → SAML. "Grant an app permission to my data via a token without giving my password" → OAuth. OAuth handles authorization; OIDC adds authentication on top of it.

🎯 **Exam trap** — **LDAP is the directory protocol, not the federation protocol.** It authenticates against a directory (AD). If the scenario is cross-organization web SSO, that's SAML/OIDC — not LDAP.

### Interoperability and attestation

**Interoperability** — the ability of different identity systems/protocols to work together (e.g., SAML and OIDC federations bridging across vendors). **Attestation** — formally confirming/recertifying that access rights are correct and appropriate; an access review where managers/owners *attest* that their people still need what they have.

⚡ **Keyword association** — "managers review and certify their team's access quarterly" → attestation/recertification.

🔬 **Hands-on** — Run an access certification campaign in SailPoint (or Entra ID Access Reviews) where each manager attests to their reports' entitlements. That campaign *is* attestation, and revoking what they don't re-certify is how you kill privilege creep — the exact 4.6 loop, straight from your IGA work.

### Access control models

The model set from [[Domain 1 — General Security Concepts]], expanded:

| Model | Access decided by | Best for | Key trait |
|---|---|---|---|
| **MAC** (Mandatory) | System-enforced labels/clearances; users *cannot* change | Military, classified, SELinux | Most rigid; central authority sets it |
| **DAC** (Discretionary) | The data *owner's* discretion | General OS file permissions | Owner grants access; flexible, weaker |
| **RBAC** (Role-Based) | The subject's *role/job function* | Enterprises, banks | Permissions attach to roles |
| **Rule-Based** | Admin-defined *rules* applied to everyone | Firewalls, global conditions | Uniform regardless of identity |
| **ABAC** (Attribute-Based) | Evaluated *attributes* (user, resource, environment, time) | Cloud, Zero Trust, Conditional Access | Most flexible, fine-grained |

**Additional concepts:**

| Concept | What it is |
|---|---|
| **Time-of-day restrictions** | Limiting access to certain hours (no logins after 6pm) — a rule-based condition. |
| **Least privilege** | Granting only the minimum access needed to do the job, nothing more — the foundational IAM principle. |

🎯 **Exam trap** — **RBAC (Role-Based) vs. Rule-Based.** On SY0-701, **RBAC = Role-Based**. Job function → role-based. A condition/rule applied to everyone (time-of-day, firewall rule) → rule-based. This pair recurs from Domain 1 and is a guaranteed trap.

🎯 **Exam trap** — **MAC vs. DAC by who controls it.** MAC = the *system/central authority* enforces labels; the owner *cannot* override. DAC = the *owner* decides. "Users can share their own files" → DAC. "Classification labels enforced regardless of owner wishes" → MAC.

📌 **Scenario pattern** — "Access depends on the user's clearance, the document's classification label, and the request's time and location, evaluated together." → **ABAC** (multiple attributes). If it were *only* the clearance-vs-label with no owner discretion → MAC.

### Multifactor authentication

The **factors** — the categories MFA combines. Real MFA requires factors from *different categories*, not two of the same.

| Factor | Definition | Examples |
|---|---|---|
| **Something you know** | Knowledge | Password, PIN, security question |
| **Something you have** | Possession | Hard/soft token, smart card, security key, phone |
| **Something you are** | Inherence (biometric) | Fingerprint, face, iris, voice |
| **Somewhere you are** | Location | GPS, IP geolocation, network location |

**Implementations:**

| Implementation | What it is |
|---|---|
| **Biometrics** | "Something you are" — fingerprint/face/iris readers. |
| **Hard token** | A physical device generating one-time codes (RSA SecurID fob). |
| **Soft token** | A software app generating one-time codes (Microsoft Authenticator, Google Authenticator). |
| **Security keys** | Hardware authenticators (FIDO2/YubiKey) — phishing-resistant, "something you have." |

🎯 **Exam trap** — **Two of the same factor is NOT MFA.** A password + a security question are *both* "something you know" — that's single-factor (two knowledge items). True MFA crosses categories (know + have, or have + are). This is a favorite trap.

🎯 **Exam trap** — **Hard token (physical hardware) vs. soft token (software app).** Both are "something you have," but a hard token is a dedicated *device*; a soft token is an *app* on a device you already own. Security keys (FIDO2) are the phishing-resistant flavor.

⚡ **Keyword association** — "fingerprint/face" → something you are. "code from an app" → soft token (something you have). "GPS/IP location requirement" → somewhere you are. "phishing-resistant hardware" → FIDO2 security key.

### Password concepts

| Concept | What it is / exam angle |
|---|---|
| **Length** | The number of characters — the *single most important* factor in password strength. Longer beats complex. |
| **Complexity** | Mixing character types (upper/lower/number/symbol) — helps, but length matters more. |
| **Reuse** | Using the same password across accounts/over time — prohibited; enables credential stuffing. |
| **Expiration** | Forcing periodic password changes — *modern guidance (NIST) has moved away from forced expiration* unless compromise is suspected, because it drives weak, predictable patterns. |
| **Age** | Minimum/maximum password age — minimum age stops users cycling back to a favorite immediately. |
| **Managers** | Password managers — generate and store unique strong passwords; the recommended human solution. |
| **Passwordless** | Eliminating passwords entirely (FIDO2, Windows Hello, passkeys, biometrics) — the modern direction; nothing to phish or reuse. |

🎯 **Exam trap** — **Length over complexity.** Current guidance: a long passphrase beats a short complex string. And **forced periodic expiration is now discouraged** by NIST (it produces predictable `Spring2024!` → `Summer2024!` patterns) — change on compromise, not on a calendar. CompTIA has updated toward this; watch for it.

⚡ **Keyword association** — "nothing to phish / FIDO2 / passkey / Windows Hello" → passwordless. "stop reverting to the old password immediately" → minimum password age.

### Privileged access management (PAM)

Tools and practices for controlling, securing, and auditing *privileged* (admin) accounts — the highest-value targets.

| Mechanism | What it is |
|---|---|
| **Just-in-time (JIT) permissions** | Elevated access is granted *only for the moment it's needed*, then automatically revoked — no standing admin rights. |
| **Password vaulting** | Privileged credentials stored in a secure vault; admins check them out, sessions are brokered/recorded, passwords auto-rotated. |
| **Ephemeral credentials** | Short-lived credentials that expire quickly (minutes/hours), minimizing the window of exposure if stolen. |

🎯 **Exam trap** — **JIT vs. standing privilege.** JIT eliminates *standing* (always-on) admin rights — access is granted on request, for a window, then removed. "Admin rights only when actively needed, auto-revoked after" → JIT. This is the modern PAM answer and exactly what you implement with CyberArk and Entra PIM.

⚡ **Keyword association** — "check out the admin password, session recorded, rotated after" → password vaulting. "access granted for 1 hour then revoked" → JIT / ephemeral credentials.

🔬 **Hands-on** — You already live this: a CyberArk safe brokering and rotating privileged credentials *is* password vaulting; Entra **PIM** activating Global Admin for an hour with approval *is* just-in-time + ephemeral. Map your production PAM controls to these three exam terms and 4.6's PAM section is free points.

---

## 4.7 Automation & orchestration (secure operations)

Using scripting/SOAR/IaC to make security operations faster, more consistent, and more scalable. Know the use cases, the benefits, and — the part the exam loves — the *downsides*.

### Use cases

| Use case | What it automates |
|---|---|
| **User provisioning** | Auto-creating/disabling accounts and access on hire/role-change/termination (JML automation). |
| **Resource provisioning** | Standing up infrastructure (VMs, storage, networks) via code with security built in. |
| **Guard rails** | Automated policy boundaries that prevent insecure actions (e.g., blocking public S3 buckets by default). |
| **Security groups** | Programmatically managing group memberships/firewall security groups. |
| **Ticket creation** | Auto-generating tickets from alerts so nothing is dropped. |
| **Escalation** | Automatically routing/raising incidents that aren't handled in time. |
| **Enabling/disabling services and access** | Toggling access or services automatically based on triggers/conditions. |
| **Continuous integration and testing (CI/CD)** | Automated build/test/security-scan pipelines so flaws are caught before release. |
| **Integrations and APIs** | Stitching tools together via APIs so they act in concert (the orchestration glue). |

### Benefits

Efficiency/time saving, enforcing baselines, standard configurations, scaling securely, employee retention (less drudgery), faster reaction time, and acting as a **workforce multiplier** (a small team does the work of a large one).

### Considerations

| Consideration | The risk |
|---|---|
| **Complexity** | Automation can be intricate and hard to understand/troubleshoot. |
| **Cost** | Upfront tooling and development investment. |
| **Single point of failure** | If the automation platform breaks, *everything* it runs breaks at once. |
| **Technical debt** | Quick automations accumulate as brittle, undocumented scripts that become liabilities. |
| **Ongoing supportability** | Someone must maintain it as systems/APIs change — automation rots if unmaintained. |

🎯 **Exam trap** — Automation's headline benefit is **consistency/enforcing baselines and being a workforce multiplier**, but its headline risk is becoming a **single point of failure** and breeding **technical debt**. If the scenario asks for the *downside* of heavy automation, those two are the answers — not "it's too fast."

⚡ **Keyword association** — "small team does the work of many" → workforce multiplier. "one broken script takes everything down" → single point of failure. "pile of brittle undocumented scripts" → technical debt.

🔬 **Hands-on** — Your SailPoint joiner-mover-leaver workflows and any PowerShell that bulk-manages Entra accounts *are* user-provisioning automation and a workforce multiplier. Note where a single connector failure would stall all provisioning — that's the single-point-of-failure consideration in your own environment.

---

## 4.8 Incident response

The most procedurally tested section in the domain. Know the **phases in order**, the testing types, and the forensics concepts — especially the order of volatility.

### The IR process (in order)

| # | Phase | What happens |
|---|---|---|
| 1 | **Preparation** | Build the capability *before* an incident — IR plan, tools, training, communication paths, logging. The only phase that's continuous/proactive. |
| 2 | **Detection** | Identify that an incident is occurring (alerts, reports, monitoring). |
| 3 | **Analysis** | Validate and scope it — is it real? what's affected? how bad? |
| 4 | **Containment** | Stop the bleeding — isolate affected systems to prevent spread (short-term and long-term containment). |
| 5 | **Eradication** | Remove the threat — delete malware, close the vulnerability, eliminate the attacker's foothold. |
| 6 | **Recovery** | Restore systems to normal operation and validate they're clean and functioning. |
| 7 | **Lessons learned** | Post-incident review — what happened, what worked, what to improve. Feeds back into preparation. |

🎯 **Exam trap** — **Memorize the order.** Containment comes *before* eradication (you isolate before you clean), and eradication comes *before* recovery (you remove the threat before you restore). A scenario that "restored from backup while the attacker still had access" skipped **containment/eradication** — the recovery was premature.

🎯 **Exam trap** — **Containment vs. eradication.** Containment = *isolate/stop spread* (pull the network cable, quarantine the VLAN). Eradication = *remove the threat* (wipe the malware, patch the hole). "Disconnected the host to stop lateral movement" → containment. "Reimaged and removed the backdoor" → eradication.

🧠 **Mnemonic** — Phases in order: **"People Don't Always Catch Every Real Loss"** → **P**reparation, **D**etection, **A**nalysis, **C**ontainment, **E**radication, **R**ecovery, **L**essons learned.

### Training and testing

| Activity | What it is |
|---|---|
| **Training** | Preparing responders and staff to execute the IR plan and recognize incidents. |
| **Tabletop exercise** | A *discussion-based* walkthrough — the team talks through a scenario around a table. Low cost, no systems touched. |
| **Simulation** | A *hands-on / functional* exercise that actually executes parts of the response (e.g., a simulated phishing campaign or live drill). |

🎯 **Exam trap** — **Tabletop (talk through it) vs. simulation (do it).** A tabletop is discussion-only — cheap, validates the plan on paper. A simulation actually exercises the response/systems. "Team gathered to discuss how they'd respond" → tabletop. "Ran a live drill / launched a fake phishing test" → simulation.

### Root cause analysis and threat hunting

**Root cause analysis (RCA)** — determining the *underlying* cause of an incident (not just the symptom) so it can be permanently fixed. Happens around lessons-learned. **Threat hunting** — *proactively* searching for threats already in the environment that evaded detection, rather than waiting for an alert; hypothesis-driven, using your telemetry.

🎯 **Exam trap** — **Threat hunting is proactive (no alert yet)**; incident detection is reactive (responding to an alert). "Analyst proactively searched logs for signs of an undetected compromise" → threat hunting.

### Digital forensics

Collecting, preserving, and analyzing evidence in a legally defensible way.

| Concept | What it is |
|---|---|
| **Legal hold** | A formal directive to *preserve* all potentially relevant data once litigation/investigation is anticipated — overrides normal retention/deletion. |
| **Chain of custody** | Documentation of *who handled the evidence, when, and why* from collection onward — proves it wasn't tampered with; breaks → evidence inadmissible. |
| **Acquisition** | The forensically sound *collection* of evidence (bit-for-bit imaging), preserving the original. |
| **Reporting** | Documenting findings, methods, and conclusions for legal/management use. |
| **Preservation** | Protecting evidence from alteration/loss (write blockers, working from copies, hashing to prove integrity). |
| **E-discovery** | The legal process of identifying, collecting, and producing electronically stored information for litigation. |

🎯 **Exam trap** — **Legal hold vs. chain of custody.** Legal hold = the *duty to preserve* (don't delete). Chain of custody = the *documented handling trail* (who touched it). "Notice to stop deleting relevant data" → legal hold. "Log proving the evidence wasn't tampered with" → chain of custody. Breaking chain of custody makes evidence inadmissible — that's the high-stakes detail.

### Order of volatility

When acquiring evidence, collect from **most volatile to least volatile** — capture the data that disappears fastest *first*, before it's lost.

| Order | Data source | Why this position |
|---|---|---|
| 1 (most volatile) | **CPU registers & cache** | Vanishes in nanoseconds. |
| 2 | **RAM / memory & running state** | Lost on power-off; holds processes, keys, network connections. |
| 3 | **Network state / routing table / ARP cache / running connections** | Changes constantly, lost quickly. |
| 4 | **Running processes / temporary files / swap** | Volatile but lasts a bit longer. |
| 5 | **Disk / persistent storage** | Survives reboot; relatively stable. |
| 6 | **Remote logging / monitoring data** | Off-system copies, durable. |
| 7 (least volatile) | **Archival / backups / physical config (printouts)** | Most durable, collect last. |

🎯 **Exam trap** — **Most volatile FIRST.** The classic trap reverses it. You grab **RAM/memory before disk** because powering off destroys memory but the disk survives. If a question asks what to collect first in a live forensic acquisition → the most volatile source available (registers/cache, then RAM).

🧠 **Mnemonic** — Order of volatility (top-down): **"Real Memory Needs Proper Disk Logging Always"** → **R**egisters/cache, **M**emory (RAM), **N**etwork state, **P**rocesses/temp files, **D**isk, **L**ogging (remote), **A**rchives/backups.

🔬 **Hands-on** — In a lab VM, capture RAM with a tool like FTK Imager or `winpmem`, *then* image the disk — and hash both with `Get-FileHash` to establish integrity. Doing memory-before-disk once cements order of volatility, and the hash step is your preservation/chain-of-custody discipline in miniature.

---

## 4.9 Investigation data sources

Where you look when you investigate — the logs and the other artifacts.

### Log data

| Source | What it tells you |
|---|---|
| **Firewall logs** | Allowed/denied connections — source/dest/port; shows scanning, blocked traffic, exfil attempts. |
| **Application logs** | App-specific events, errors, transactions — what the software did. |
| **Endpoint logs** | Host-level activity (EDR telemetry) — process execution, file changes, local events. |
| **OS-specific security logs** | Authentication, privilege use, account changes (Windows Security event log, Linux auth.log) — the who-logged-in-and-what-they-did record. |
| **IPS/IDS logs** | Detected/blocked intrusion attempts and signature/anomaly matches. |
| **Network logs** | Device and connection events across the network (switch/router logs, flow data). |
| **Metadata** | Data *about* data — email headers, file timestamps, EXIF, sender/recipient — often pivotal in tracing origin without the content itself. |

⚡ **Keyword association** — "who logged in / failed authentications / privilege changes" → OS security log. "email headers / timestamps / data about the data" → metadata. "allowed and denied connections by port" → firewall logs.

### Other sources

| Source | What it provides |
|---|---|
| **Vulnerability scans** | Point-in-time view of weaknesses (feeds 4.3). |
| **Automated reports** | Scheduled summaries from tools/SIEM — trends and status without manual querying. |
| **Dashboards** | Real-time visual at-a-glance state of security posture/alerts (SIEM/SOC dashboards). |
| **Packet captures (PCAP)** | Full recorded packet contents — the deepest network evidence; shows actual payloads (Wireshark/tcpdump). |

🎯 **Exam trap** — **Packet capture vs. NetFlow vs. logs (recurring).** PCAP = *full packet payloads* (heaviest, deepest — actual content). NetFlow = *flow metadata only* (who/what/how much, no content). Logs = *event records* from a device/app. "Need the actual data inside the traffic" → packet capture. "Need traffic patterns/volumes" → NetFlow. "Need a record of system/app events" → logs.

⚡ **Keyword association** — "see the actual payload of the traffic" → packet capture. "at-a-glance real-time view" → dashboard. "scheduled summary" → automated report.

---

## 🎯 Top exam traps

1. **IR phases in order** — Preparation → Detection → Analysis → Containment → Eradication → Recovery → Lessons learned. Containment before eradication; eradication before recovery. Restoring while the attacker is still in = skipped containment.
2. **Order of volatility: most volatile FIRST.** RAM/memory before disk — powering off destroys memory, disk survives. The classic trap reverses it.
3. **Containment (isolate/stop spread) vs. eradication (remove the threat).** Pulling the cable = containment; reimaging = eradication.
4. **CVE (the named identifier) vs. CVSS (the 0–10 severity score).** They appear together as a trap pair.
5. **False positive (false alarm, reported but not real) vs. false negative (missed real threat).** The false negative is the dangerous one — applies to scans, IDS, and alert tuning.
6. **IDS detects/alerts (out-of-band); IPS prevents/blocks (in-line).** Signature-based misses zero-days; anomaly-based catches novel attacks but cries wolf.
7. **SPF (authorized sending IPs) vs. DKIM (cryptographic signature) vs. DMARC (policy + reporting tying them together, decides reject/quarantine on failure).**
8. **NetFlow (flow metadata, no payload) vs. packet capture (full payload).** "Traffic patterns" → NetFlow; "actual data in the packets" → PCAP.
9. **SAML (authentication/SSO via XML assertions) vs. OAuth (authorization/delegation via tokens).** LDAP is the directory protocol, not federation.
10. **RBAC = Role-Based on this exam** (job function); rule-based = a condition applied to everyone. **MAC** = system/central labels (owner can't override); **DAC** = owner's discretion.
11. **Two of the same factor is not MFA** (password + security question = both "something you know"). Cross categories.
12. **JIT eliminates standing admin rights** — access granted for a window then auto-revoked (CyberArk/Entra PIM).
13. **Sanitization (wipe, media reusable) vs. destruction (physically gone).** Degaussing works on HDD/tape, not SSD/flash. **Certification** is the proof, not the act.
14. **Identity proofing (one-time, onboarding) vs. authentication (every login).** **Legal hold (duty to preserve) vs. chain of custody (handling trail).**
15. **NAC (device admission/posture) vs. DLP (data leaving) vs. firewall (traffic between zones).** **EDR (endpoint-only) vs. XDR (multi-source correlation).**
16. **Password length beats complexity, and forced periodic expiration is now discouraged** (NIST) — change on compromise, not on a calendar.

## 🧠 Mnemonic pack

- **Incident response phases (in order) — "People Don't Always Catch Every Real Loss"**
  - **P**eople → **P**reparation
  - **D**on't → **D**etection
  - **A**lways → **A**nalysis
  - **C**atch → **C**ontainment
  - **E**very → **E**radication
  - **R**eal → **R**ecovery
  - **L**oss → **L**essons learned
  - ✅ Maps to Preparation, Detection, Analysis, Containment, Eradication, Recovery, Lessons learned — all seven phases, in exam order, correct.

- **Order of volatility (most volatile first) — "Real Memory Needs Proper Disk Logging Always"**
  - **R**eal → **R**egisters & cache (most volatile)
  - **M**emory → **M**emory / RAM
  - **N**eeds → **N**etwork state (ARP/routing/connections)
  - **P**roper → **P**rocesses / temp files / swap
  - **D**isk → **D**isk / persistent storage
  - **L**ogging → remote **L**ogging / monitoring data
  - **A**lways → **A**rchives / backups (least volatile)
  - ✅ Maps to Registers/cache, Memory, Network state, Processes, Disk, Logging, Archives — most-volatile-to-least order, correct.

- **Email anti-spoofing trio — "Signed Keys Decide"**
  - **S**igned → **S**PF (which **S**ervers may **S**end)
  - **K**eys → **D**KIM (cryptographic **K**ey signature) — *K cues the signing key*
  - **D**ecide → **D**MARC (the policy that **D**ecides reject/quarantine + reports)
  - ✅ Maps to SPF (authorized senders), DKIM (key-based signature), DMARC (decision policy). The three email-security mechanisms, correct. (Use the letter cues: **S**enders, **K**ey, **D**ecide.)

- **Mobile deployment ownership — "Buy Cheap, Company Owns, Choose Yours"**
  - **B**uy Cheap → **B**YOD (employee owns — cheapest for the company)
  - **C**ompany **O**wns → **C**OPE (Corporate-Owned, Personally Enabled)
  - **C**hoose **Y**ours → **C**YOD (Choose Your Own Device from an approved list)
  - ✅ Maps to BYOD, COPE, CYOD — the three deployment models, correct.

## ✅ Self-check

1. During a live intrusion, you can collect evidence from RAM, the routing table, an attached disk, and last night's backup tape. In what order do you acquire them, and why?
2. A SOC analyst restores affected servers from backup, but the attacker still has an active foothold and reinfects them. Which IR phase(s) were skipped?
3. A vulnerability is referenced as "CVE-2025-0420, CVSS 9.8." Which token is the identifier and which is the severity?
4. A scanner reports a critical flaw on a server that, on inspection, isn't actually vulnerable. What is this called, and which type of error is the *more dangerous* one to have?
5. Your wireless needs each employee to authenticate with their own credentials against a central server. Which WPA3 mode and which back-end server?
6. An email fails authentication; the receiving server rejects it and sends a report to your domain. Which of SPF/DKIM/DMARC enforced the rejection and reporting?
7. A user changed from Teller to Loan Officer and kept all their old teller permissions plus the new ones. What is this condition, and what two practices correct it?
8. You need to log into a partner company's SaaS app using your own Entra ID credentials with no separate account. What capability is this, and which protocol class enables the web SSO?
9. A password ("knowledge") plus a security question — is this MFA? Why or why not?
10. A privileged session is granted only for one hour with manager approval, then automatically revoked. Which PAM concept is this?
11. A decommissioned HDD will be reused internally; another batch of SSDs is leaving the building permanently. What is the appropriate disposal method for each, and which one does degaussing *not* work on?
12. An analyst proactively searches the SIEM for signs of a compromise that never triggered an alert. What is this activity?
13. You need to see the actual contents (payload) of suspicious traffic, not just who talked to whom. Which data source — NetFlow or packet capture?
14. A control admits devices to the LAN only after verifying they're patched and running AV. Which enterprise capability is this?
15. The team gathers in a conference room to talk through how they'd respond to a ransomware scenario, touching no systems. What kind of IR test is this?

<details>
<summary>Answers</summary>

1. **RAM → routing table → disk → backup tape**, following **order of volatility (most volatile first)**. RAM and network state vanish on power-off/quickly; the disk survives reboot; the backup is the most durable. Capture the perishable data before it's lost.
2. **Containment and eradication.** Recovery (restoring) was performed before the threat was isolated and removed, so the attacker was still present — recovery was premature.
3. **CVE-2025-0420** is the identifier (names the specific vulnerability); **CVSS 9.8** is the severity score (0–10).
4. A **false positive** (false alarm). The more dangerous error is a **false negative** — a real vulnerability the scanner missed, because it creates false confidence.
5. **WPA3-Enterprise (802.1X)** with a **RADIUS** server as the AAA back-end (per-user authentication, not a shared PSK).
6. **DMARC** — it ties SPF/DKIM results to a policy (reject/quarantine) and provides reporting. SPF and DKIM perform the checks; DMARC decides the action and reports.
7. **Privilege creep (accumulation)** from a role change (the "mover"). Corrected by **least privilege** and regular **attestation/access recertification** (revoking what's no longer needed).
8. **Federation** (trust between identity providers). **SAML** (or OIDC) enables the web SSO/assertion exchange — not LDAP, which is the directory protocol.
9. **No.** Both a password and a security question are "something you know" — two of the *same* factor is single-factor. True MFA crosses categories (e.g., know + have).
10. **Just-in-time (JIT) permissions** (with ephemeral/short-lived access) — eliminating standing admin rights. Implemented in tools like CyberArk and Entra PIM.
11. **HDD for internal reuse → sanitization** (overwrite/secure-wipe, media reusable). **SSDs leaving permanently → destruction** (physically destroy) or cryptographic erase. **Degaussing does not work on SSDs/flash** — only magnetic media (HDD/tape).
12. **Threat hunting** — proactively searching for undetected threats without waiting for an alert.
13. **Packet capture (PCAP)** — it records full payloads. NetFlow only records flow metadata (source/dest/port/volume), not contents.
14. **Network access control (NAC)** — admits devices to the network based on identity and posture (health/compliance check).
15. A **tabletop exercise** — discussion-based, no systems touched (as opposed to a simulation/live drill).

</details>

⬅ [[Security+ SY0-701 — Course Index]] | ➡ Next: [[Domain 5 — Security Program Management & Oversight]]
