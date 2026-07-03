> **Domain 3 — Security Architecture**
> **Exam weight:** 18% (~16 questions on a 90-question form).
> **What it covers:** The design-and-placement layer — how you build resilient, defensible systems. Architecture models and their security implications (cloud, IaC, serverless, microservices, virtualization, containers, IoT, ICS/SCADA), secure infrastructure design (device placement, zones, firewalls, secure comms), data protection (types, classifications, states, sovereignty, protection methods), and resilience/recovery (HA, sites, backups, power).
> **Strategic note:** This is the *where does the control sit and how does it fail* domain. For every component, ask three questions: **Where** is it placed in the topology? **How** does it fail — fail-open (availability wins) or fail-closed (security wins)? What's the **trade-off** — availability vs. cost vs. resilience vs. patchability? CompTIA tests judgment about placement and trade-offs more than rote definitions here.

## Objective map

- **3.1** Architecture models — cloud (responsibility matrix, hybrid, third-party), IaC, serverless, microservices, network infra (air-gap, segmentation, SDN), on-prem, centralized vs. decentralized, containerization, virtualization, IoT, ICS/SCADA, RTOS, embedded, high availability; and the design considerations (availability, resilience, cost, responsiveness, scalability, deployment ease, risk transference, recovery ease, patch availability, inability to patch, power, compute).
- **3.2** Secure enterprise infrastructure — device placement, security zones, attack surface, connectivity, failure modes (fail-open/fail-closed), device attributes (active/passive, inline/tap), appliances (jump server, proxy, IPS/IDS, load balancer, sensors), port security (802.1X, EAP), firewall types (WAF, UTM, NGFW, L4 vs. L7), secure comms/access (VPN, tunneling, TLS, IPsec, SD-WAN, SASE).
- **3.3** Protecting data — data types (regulated, trade secret, IP, legal, financial, human/non-human-readable), classifications (sensitive, confidential, public, restricted, private, critical), states (at rest, in transit, in use), sovereignty, geolocation, methods (geographic restriction, encryption, hashing, masking, tokenization, obfuscation, segmentation, permission restrictions).
- **3.4** Resilience & recovery — HA (load balancing vs. clustering), sites (hot/warm/cold/dispersion), platform diversity, multi-cloud, COOP, capacity planning, testing (tabletop, failover, simulation, parallel), backups (onsite/offsite, frequency, encryption, snapshots, recovery, replication, journaling), power (generators, UPS).

---

## 3.1 Architecture models & implications

There is no single "secure" architecture — each model trades security properties against cost, agility, and control. The exam wants you to recognize the **security implications** of each choice. Lead with what *changes about who is responsible* and *what new attack surface appears* when you pick a model.

### Cloud

| Concept | Definition | Security implication |
|---|---|---|
| **Shared responsibility model** | The division of security duties between the cloud provider and the customer. Varies by service model. | The provider secures *of* the cloud (hardware, hypervisor, facilities); you secure *in* the cloud (data, identity, config). Misreading this line is the #1 cause of cloud breaches. |
| **Responsibility matrix** | The explicit document/table mapping every control to provider, customer, or shared. | You must *read* it per service. IaaS = you own OS/patching up; SaaS = provider owns nearly everything except data and access. |
| **Hybrid considerations** | Mixing on-prem and cloud, connected over VPN/dedicated link. | Larger attack surface, identity must federate across both, data crosses trust boundaries, and the seam between environments is where misconfig lives. |
| **Third-party vendors** | Cloud services and their subcontractors that handle your data/operations. | Their risk is *your* risk. Supply-chain exposure; you inherit their breaches. Tie to vendor assessment in [[Domain 5 — Security Program Management & Oversight]]. |

**Who-owns-what by service model:**

| Layer | IaaS | PaaS | SaaS |
|---|---|---|---|
| Data & access | **You** | **You** | **You** |
| Application | **You** | **You** | Provider |
| OS / runtime | **You** | Provider | Provider |
| Hypervisor / hardware / facility | Provider | Provider | Provider |

🎯 **Exam trap** — **Data and identity are ALWAYS the customer's responsibility**, in every service model including SaaS. If a question asks "what does the customer always remain responsible for regardless of cloud model?" the answer is **data and access/identity**. The provider never owns your data classification or your IAM decisions.

⚡ **Keyword association** — "who patches the guest OS in IaaS" → customer. "shared responsibility / division of duties" → responsibility matrix. "on-prem + cloud connected" → hybrid. "vendor's subcontractor handles data" → third-party/supply-chain risk.

📌 **Scenario pattern** — "An organization moves email to a SaaS provider but a phishing campaign harvests user credentials and exfiltrates mailboxes." → The provider's platform was fine; the *customer-owned* identity/access control (MFA, Conditional Access) failed. SaaS does not offload identity.

### Infrastructure as Code (IaC)

Defining and provisioning infrastructure through machine-readable definition files (Terraform, ARM/Bicep, CloudFormation) instead of manual clicks. **Security implication:** consistency and repeatability *eliminate configuration drift* and make every change reviewable and version-controlled — but a single bad template propagates the same flaw everywhere instantly, and secrets hardcoded in IaC files are a real leak vector.

⚡ **Keyword association** — "provision infrastructure from code / repeatable / no drift / Terraform" → IaC. "the same misconfig deployed to 200 servers" → IaC blast radius.

🔬 **Hands-on** — Export your Entra Conditional Access policies to JSON (or define them in Terraform's `azuread` provider) and store them in Git. That's IaC for identity: every policy change is reviewable, diffable, and revertible — and you'll see firsthand why a single bad commit is dangerous at scale. Ties to version control in [[Domain 1 — General Security Concepts]].

### Serverless

A model where the cloud provider runs your code in ephemeral functions (AWS Lambda, Azure Functions) with no server for you to manage or patch. **Security implication:** you shed OS/patch responsibility (provider's job) but gain a sprawl of tiny event-triggered entry points, heavy reliance on the provider's IAM for function permissions, and harder monitoring. Over-permissioned function roles are the classic serverless flaw.

🎯 **Exam trap** — **Serverless removes your OS-patching burden but NOT your code/config burden.** "No server" does not mean "no responsibility" — you still own the function code, its dependencies, and its IAM role scope.

### Microservices

Decomposing a monolithic application into many small, independently deployable services that communicate over APIs. **Security implication:** each service is a separate attack surface and the **API traffic between them** must be secured (mTLS, auth tokens). Upside: a compromise of one service is contained if segmentation is right; downside: massively more east-west traffic and more things to authenticate.

⚡ **Keyword association** — "monolith broken into small independent services talking over APIs" → microservices. "lots of east-west API calls to secure" → microservices.

### Network infrastructure

| Approach | Definition | Use / implication |
|---|---|---|
| **Physical isolation / air-gap** | No physical network connection between the protected system and any other network. | Maximum isolation for the most sensitive systems (ICS, classified). Defeated by removable media (Stuxnet) — air-gap ≠ invulnerable. |
| **Logical segmentation** | Separating networks logically (VLANs, subnets, security groups) on shared physical hardware. | Cheaper and flexible; limits lateral movement and broadcast domains. The everyday segmentation control. |
| **Software-defined networking (SDN)** | Decoupling the network **control plane** (decisions) from the **data plane** (forwarding), centrally programmable via controller/API. | Agile, policy-driven, automatable. New risk: the SDN controller is a high-value single point of compromise. |

🎯 **Exam trap** — **Air-gap vs. logical segmentation.** Air-gap = *no physical connection at all*. Logical segmentation = *separated but still on shared physical media* (VLANs). If the scenario demands the absolute strongest isolation for a critical system, the answer is **air-gap**; if it's "separate the IoT devices from the corporate LAN cost-effectively," it's **segmentation/VLAN**.

🎯 **Exam trap** — **SDN echoes the Zero Trust control-plane/data-plane split** from [[Domain 1 — General Security Concepts]]. Control plane = decisions; data plane = forwarding traffic. Same vocabulary, different context.

### On-premises

Infrastructure you own and run in your own facility. **Security implication:** maximum control and data custody (good for FFIEC/data-sovereignty), but *you* own every layer — patching, physical security, capacity, power, and recovery. No risk transference to a provider.

### Centralized vs. decentralized

| | **Centralized** | **Decentralized** |
|---|---|---|
| Control/management | Single authority, one location/system | Distributed across nodes/locations |
| Strength | Consistent policy, easier to monitor and audit | No single point of failure; resilient; scales locally |
| Weakness | Single point of failure; bottleneck; high-value target | Inconsistent policy, harder to govern and audit |
| Example | Central SIEM, single identity provider | Blockchain, edge computing, regional autonomy |

⚡ **Keyword association** — "single point of control / one console / easier to audit but single point of failure" → centralized. "distributed / no single point of failure / harder to govern" → decentralized.

### Containerization

Packaging an application with its dependencies into an isolated, portable unit (Docker) that shares the **host OS kernel** — lighter than a VM. **Security implication:** isolation is weaker than a VM because containers share the kernel — a kernel exploit can break out. Image provenance (unsigned/poisoned images) and the orchestrator (Kubernetes) are key attack surfaces.

### Virtualization

Running multiple virtual machines, each with its **own guest OS**, on one physical host via a **hypervisor**. **Security implication:** strong isolation between VMs (separate kernels), but the hypervisor is the crown jewel — **VM escape** lets an attacker break from a guest to the host/other guests. **VM sprawl** (unmanaged, unpatched VMs) is the operational risk.

🎯 **Exam trap** — **Containers vs. VMs.** VMs each have their **own OS/kernel** (heavier, stronger isolation, hypervisor risk = VM escape). Containers **share the host kernel** (lighter, faster, weaker isolation, risk = container breakout). "Shares the host OS kernel" → container. "Each has its own guest OS" → VM.

⚡ **Keyword association** — "hypervisor / VM escape / each VM has its own OS" → virtualization. "Docker / shares host kernel / image / portable" → containerization. "VM escape" → guest-to-host breakout.

### IoT, ICS/SCADA, RTOS, embedded systems

| System | Definition | Security implication |
|---|---|---|
| **IoT** | Internet-connected everyday/consumer devices (cameras, sensors, thermostats). | Weak/default credentials, rarely patched, huge fleet = big botnet surface (Mirai). Segment them off. |
| **ICS / SCADA** | Industrial Control Systems / Supervisory Control and Data Acquisition — control physical processes (power, water, manufacturing). | Prioritize **availability and safety over confidentiality**; legacy protocols, often unpatchable, frequently air-gapped or segmented. A breach can cause physical harm. |
| **RTOS** | Real-Time Operating System — guarantees response within a strict time bound (avionics, medical devices, automotive). | Determinism is the priority; limited resources for security agents; patching can violate timing/safety certification. |
| **Embedded systems** | Purpose-built computing inside a larger device (firmware in a router, printer, controller). | Fixed function, hard or impossible to patch, long lifecycles, often forgotten in inventories. |

🎯 **Exam trap** — **In ICS/SCADA the CIA priority inverts: Availability first.** Unlike a typical IT system where confidentiality often leads, an industrial system that controls a turbine cannot go down — and a forced patch/reboot can be more dangerous than the vulnerability. If a scenario stresses "we can't take this down to patch / safety / uptime is paramount," think ICS/SCADA and **compensating controls** rather than patching.

⚡ **Keyword association** — "default creds / consumer device fleet / botnet" → IoT. "industrial process / turbine / water plant / can't be patched without downtime" → ICS/SCADA. "strict timing guarantee / avionics / medical" → RTOS. "firmware inside a device / fixed function" → embedded.

### High availability

Designing systems so a single failure does not cause an outage — through redundancy, failover, and load distribution. Quantified as uptime (e.g., "five nines" = 99.999%). Covered in depth in **3.4**; here it's one of the architectural goals that drives the considerations below.

### Considerations (the trade-off vocabulary)

This is a high-yield list — CompTIA phrases questions as "which consideration is the engineer prioritizing?"

| Consideration | What it means | Keyword cue |
|---|---|---|
| **Availability** | The system is up and reachable when needed. | "uptime," "always reachable" |
| **Resilience** | The system absorbs failures and keeps functioning / self-recovers. | "withstand failure," "degrade gracefully" |
| **Cost** | Capital and operating expense of the design. | "budget," "cheapest option" |
| **Responsiveness** | Latency/speed of response to requests. | "low latency," "fast response" |
| **Scalability** | Ability to grow (up/out) with demand. | "handle growth," "scale out" |
| **Ease of deployment** | How quickly/simply it can be stood up. | "fast to deploy," "minimal setup" |
| **Risk transference** | Shifting risk to a third party (cloud provider, insurance). | "offload to provider," "insurance" |
| **Ease of recovery** | How fast/simply you can restore after failure. | "quick to restore," "easy rollback" |
| **Patch availability** | Whether vendor patches exist for the component. | "vendor still issues updates" |
| **Inability to patch** | The component cannot be patched (EoL, ICS, embedded). | "no patch exists / can't take it down" |
| **Power** | Electrical supply and its redundancy. | "UPS," "generator," "power draw" |
| **Compute** | Processing capacity available/required. | "CPU/RAM," "processing power" |

🎯 **Exam trap** — **Risk transference is shifting risk, not eliminating it.** Buying cyber insurance or moving to a cloud provider transfers *financial/operational* risk but you retain *accountability* (and reputational/regulatory risk). Tie to [[Domain 5 — Security Program Management & Oversight]] risk-handling strategies.

🎯 **Exam trap** — **Inability to patch** is the trigger for **compensating controls** (segmentation, isolation, monitoring), not for ignoring the risk. When patching is impossible, you wrap the asset.

---

## 3.2 Secure enterprise infrastructure

The core question of this objective: **where do you put the control, and what happens when it dies?** Placement and failure mode dominate.

### Device placement, security zones, attack surface, connectivity

| Concept | Definition |
|---|---|
| **Device placement** | Where in the topology a control sits — at the perimeter, in a DMZ, internal, at the host. Placement determines what traffic it sees and protects. |
| **Security zones** | Network segments grouped by trust level (untrusted internet, DMZ/screened subnet, trusted internal, restricted). Traffic between zones is controlled at the boundaries. |
| **Attack surface** | The total sum of points where an attacker could attempt entry — open ports, services, interfaces, accounts. Architecture aims to *minimize* it. |
| **Connectivity** | How systems link (wired, wireless, VPN, dedicated circuit). Each connection is attack surface and must be secured/justified. |

A **screened subnet (DMZ)** is the classic placement pattern: public-facing servers (web, mail, DNS) sit in a buffer zone reachable from the internet but isolated from the internal trusted network, so a compromise there doesn't immediately reach the LAN.

⚡ **Keyword association** — "public-facing server isolated from the internal LAN" → screened subnet/DMZ. "reduce the number of exposed services/ports" → reduce attack surface. "grouped by trust level" → security zones.

### Failure modes — fail-open vs. fail-closed

The single most important placement question on this domain: **when the device fails, does traffic keep flowing or stop?**

| Mode | On failure | Prioritizes | Typical use |
|---|---|---|---|
| **Fail-open** | Traffic continues to pass (control stops enforcing) | **Availability** | Where uptime matters more than blocking (e.g., a monitoring sensor; some IDS) |
| **Fail-closed** | Traffic is blocked (no flow until restored) | **Security** | Where a breach is worse than an outage (e.g., a firewall protecting a vault; secure facility door that locks on failure) |

🎯 **Exam trap** — **Fail-open vs. fail-closed is a security-vs-availability decision, and context flips the "right" answer.** A firewall that **fails-open** is a security hole (everything passes when it dies). A firewall that **fails-closed** maintains security but causes an outage. For life-safety, doors usually **fail-open** (people must escape a fire); for data security, controls usually **fail-closed**. Always read what the scenario values: *life safety → fail-open (egress); data security → fail-closed.*

📌 **Scenario pattern** — "An inline IPS loses power. The business needs the e-commerce site to stay online above all else." → Configure **fail-open** (availability wins; accept the temporary loss of inspection). If instead it said "the segment processes classified data and must never pass uninspected traffic," → **fail-closed**.

### Device attributes — active vs. passive, inline vs. tap/monitor

| Attribute | Meaning | Implication |
|---|---|---|
| **Active** | The device can *take action* on traffic (block, drop, reset). | An IPS is active — it stops attacks but can drop legitimate traffic / be a chokepoint. |
| **Passive** | The device only *observes/alerts*, never blocks. | An IDS is passive — it can't stop an attack, only report it. |
| **Inline** | The device sits *in the traffic path*; all traffic flows through it. | Can block (active) but is a single point of failure and adds latency. |
| **Tap / monitor (out-of-band)** | The device sits *off to the side*, receiving a copy of traffic (SPAN/mirror port or network tap). | Can only observe — cannot block — but has zero impact on the live path. |

🎯 **Exam trap** — **Inline enables blocking; tap/monitor cannot block.** An IPS must be **inline** to drop traffic. An IDS on a **tap/SPAN** sees a copy and can only **alert**. If the requirement is "stop the attack," you need inline + active (IPS). If it's "monitor without affecting traffic," you need a tap (passive IDS).

### Network appliances

| Appliance | What it does | Placement / note |
|---|---|---|
| **Jump server (jump box)** | A hardened, monitored host you connect *through* to reach and administer sensitive systems. | Single controlled entry point into a protected segment; reduces attack surface for admin access. Maps to PAM. |
| **Proxy server** | An intermediary that forwards requests on a client's behalf (forward proxy) or on a server's behalf (reverse proxy). | Forward = controls/filters outbound user traffic + caching/anonymity. Reverse = sits in front of servers for load distribution, TLS termination, hiding origin. |
| **IDS** | Intrusion Detection System — *detects and alerts* on malicious traffic. | **Passive**, out-of-band (tap/SPAN). Cannot block. |
| **IPS** | Intrusion Prevention System — *detects and blocks*. | **Active**, **inline**. Can drop/reset. |
| **Load balancer** | Distributes traffic across multiple backend servers. | HA + scalability; health-checks members; can do TLS offload. |
| **Sensors** | Collection points that gather traffic/telemetry for analysis (taps, agents, network sensors feeding a SIEM/IDS). | Placement determines visibility; feed [[Domain 4 — Security Operations]] monitoring. |

🎯 **Exam trap** — **IDS vs. IPS** is the most-tested pair in 3.2. **IDS = detect + alert, passive, out-of-band (tap).** **IPS = detect + prevent, active, inline.** An IPS *can* be a single point of failure and introduce latency (it's in the path); an IDS cannot block. "Sees a copy and alerts" → IDS. "In the path and drops the packet" → IPS.

⚡ **Keyword association** — "connect through one hardened host to admin servers" → jump server. "front of web servers, distributes load, hides origin" → reverse proxy. "filters/caches outbound user web traffic" → forward proxy. "distributes across backends + health checks" → load balancer.

🔬 **Hands-on** — Your CyberArk PSM is effectively a **jump server** with session recording: admins connect *through* it to reach targets, never touching the target's credentials directly. Map that to the exam's jump-server concept and the abstract definition becomes your day job.

### Port security — 802.1X, EAP

| Term | Definition |
|---|---|
| **Port security** | Controlling which devices can connect to a physical switch port (e.g., by MAC, or by authentication). Prevents rogue devices on the wired LAN. |
| **802.1X** | The IEEE standard for **port-based network access control** — a device must authenticate before the switch port grants network access. The framework for "authenticate before you're on the network." |
| **EAP** | Extensible Authentication Protocol — the **framework** that carries authentication methods inside 802.1X (EAP-TLS, PEAP, EAP-TTLS, etc.). EAP is the envelope; the variants are the methods. |

**The 802.1X cast:** the **supplicant** (the device authenticating), the **authenticator** (the switch/AP enforcing access), and the **authentication server** (usually RADIUS) that makes the decision.

🎯 **Exam trap** — **802.1X is port-based NAC; EAP is the authentication framework it carries.** They're not interchangeable. 802.1X = *when/where* (authenticate at the port before access). EAP = *how* (the method, e.g., EAP-TLS using certificates). **EAP-TLS** (mutual certificate auth) is the strongest common variant.

⚡ **Keyword association** — "authenticate before the switch port lets you on" → 802.1X. "certificate-based mutual auth method" → EAP-TLS. "decides yes/no for 802.1X" → RADIUS authentication server.

### Firewall types

| Firewall | Operates at | What it inspects | Use |
|---|---|---|---|
| **Layer 4 (stateful/packet-filter)** | Transport (L4) | IPs, ports, protocol, connection state | Classic firewall; allow/deny by 5-tuple. Cannot see application content. |
| **Layer 7 (application)** | Application (L7) | Actual application data/payload | Deep inspection; understands HTTP, can block specific app behavior. |
| **NGFW** | L4–L7 | Everything an L4 firewall does **plus** app awareness, deep packet inspection, integrated IPS, identity awareness | Modern enterprise firewall; the "do-it-all" edge device. |
| **UTM** | L4–L7 | Bundles firewall + IPS + antivirus + content filtering + VPN in one appliance | All-in-one for SMB; convenience, but one box = one point of failure. |
| **WAF** | L7 (HTTP/S only) | Web application traffic specifically — SQLi, XSS, OWASP Top 10 | Protects web apps; placed in front of web servers. |

🎯 **Exam trap** — **WAF vs. NGFW vs. UTM.** A **WAF** is *narrow and deep* — only web app (HTTP) traffic, stops SQLi/XSS. An **NGFW** is the *broad modern firewall* (L4–L7, IPS, app/identity awareness). **UTM** is the *all-in-one bundle* aimed at SMB. If the scenario is specifically "protect a web application from SQL injection," the answer is **WAF**, not NGFW.

🎯 **Exam trap** — **Layer 4 vs. Layer 7.** L4 sees ports/IPs/state but **not content**; L7 sees the **application payload**. "Block traffic to TCP 3389" → L4 is enough. "Block requests containing a SQL injection string" → needs L7.

⚡ **Keyword association** — "SQL injection / XSS / protect a website" → WAF. "all-in-one box for a small office" → UTM. "app-aware + integrated IPS + deep inspection" → NGFW. "ports and IPs only" → L4.

### Secure communication / access

| Technology | What it is | Use / distinction |
|---|---|---|
| **VPN** | Encrypted tunnel over an untrusted network connecting a remote user or site to a private network. | Remote access (user→network) or site-to-site (network→network). |
| **Remote access** | Any method letting users reach internal resources from outside (VPN, VDI, ZTNA). | Must be authenticated, encrypted, and ideally posture-checked. |
| **Tunneling** | Encapsulating one protocol inside another, usually encrypted, to traverse an untrusted path. | The mechanism under VPNs. |
| **TLS** | Transport Layer Security — encrypts data in transit; the basis of HTTPS and many VPNs (SSL VPN). | Operates high in the stack; easy through firewalls (port 443). Per-application/session. |
| **IPsec** | Suite that encrypts/authenticates at the **network layer (L3)** — AH (integrity) and ESP (confidentiality+integrity); tunnel vs. transport mode. | Site-to-site VPNs; protects *all* IP traffic, not just one app. |
| **SD-WAN** | Software-Defined WAN — centrally managed, policy-driven WAN that routes across multiple links (MPLS, broadband, LTE) intelligently. | Replaces rigid MPLS; cost + agility for multi-site orgs. |
| **SASE** | Secure Access Service Edge — converges SD-WAN networking with cloud-delivered security (SWG, CASB, ZTNA, FWaaS) at the edge. | "Network + security as a cloud service"; the modern Zero-Trust-aligned remote/branch model. |

🎯 **Exam trap** — **IPsec vs. TLS for VPNs.** **IPsec** secures at **L3** and protects *all* IP traffic (full site-to-site tunnels). **TLS/SSL VPN** secures at the **application/transport layer**, is firewall-friendly (443), and is common for clientless/remote-user access. "Site-to-site, all traffic, network layer" → IPsec. "Remote user, browser/clientless, through 443" → TLS/SSL VPN.

🎯 **Exam trap** — **SD-WAN vs. SASE.** SD-WAN is **networking** (smart routing across links). SASE is **SD-WAN + cloud-delivered security** converged together. If the scenario adds "and enforce ZTNA/SWG/CASB security from the cloud," it's **SASE**, not plain SD-WAN.

⚡ **Keyword association** — "encrypt all IP traffic site-to-site at the network layer" → IPsec. "clientless / browser-based VPN over 443" → TLS/SSL VPN. "smart routing across MPLS+broadband+LTE" → SD-WAN. "network + security converged in the cloud / ZTNA at the edge" → SASE.

### Selecting effective controls

Choosing a control is a **trade-off and placement** exercise, not a "more is better" exercise. Match the control to: the asset's value, the threat, the failure mode you can tolerate, and cost. The exam rewards the *minimum effective, correctly placed* control — e.g., a WAF (not an NGFW) for SQLi; a tap-based IDS (not inline IPS) when zero traffic impact is required; segmentation (not patching) when a device can't be patched.

---

## 3.3 Protecting data

Two questions drive this objective: **what kind of data is it (type + classification)?** and **what state is it in (rest/transit/use)?** Those two answers dictate which protection method applies.

### Data types

| Type | Definition | Example |
|---|---|---|
| **Regulated** | Data governed by law/standard requiring specific handling. | PII, PHI (HIPAA), cardholder data (PCI DSS), GLBA-covered financial data. |
| **Trade secret** | Proprietary business info that derives value from being secret. | Formulas, algorithms, the Coca-Cola recipe, internal methods. |
| **Intellectual property (IP)** | Creations of the mind protected by law (patents, copyrights, trademarks). | Source code, designs, branded content. |
| **Legal information** | Data tied to legal proceedings/obligations. | Contracts, litigation holds, court records. |
| **Financial information** | Monetary records and account data. | Account numbers, balances, transactions, statements. |
| **Human-readable** | Data a person can directly read/interpret. | A plaintext document, a spreadsheet. |
| **Non-human-readable** | Data needing processing/decoding to interpret. | Binary, machine code, encrypted blobs, packed/encoded formats. |

⚡ **Keyword association** — "governed by HIPAA/PCI/GLBA" → regulated. "secret recipe / proprietary method that loses value if known" → trade secret. "patent/copyright/trademark" → IP. "litigation hold / court" → legal.

### Data classifications

Classification is *how sensitive* the data is — it drives handling rules. Labels vary by org; know the common set and the *relative* ordering.

| Classification | Meaning |
|---|---|
| **Public** | No harm if disclosed; intended for release. |
| **Private** | Personal data about an individual; restricted to need. |
| **Sensitive** | Disclosure would cause some harm; broad "needs protection" bucket. |
| **Confidential** | Disclosure would cause significant harm; limited to authorized parties. |
| **Restricted** | Tightly controlled; access strictly limited (often above confidential). |
| **Critical** | Essential to operations/mission; loss is severe (availability-focused as much as confidentiality). |

🎯 **Exam trap** — **Classification ≠ type.** *Type* is the nature of the data (financial, PHI, IP). *Classification* is the sensitivity level (public, confidential, restricted). A single piece of *financial* data (type) might be classified *confidential* (level). Questions sometimes ask you to separate the two.

⚡ **Keyword association** — "free to release / website content" → public. "would cause significant harm if leaked / authorized only" → confidential. "essential to the mission, loss is catastrophic" → critical.

### Data states

| State | Definition | Protected primarily by |
|---|---|---|
| **At rest** | Stored data (disk, DB, backup, object storage). | Encryption (FDE, TDE), access controls. |
| **In transit (in motion)** | Data moving across a network. | TLS, IPsec, VPN. |
| **In use** | Data actively being processed in memory/CPU. | Hardest to protect — access controls, secure enclaves, memory protection, confidential computing. |

🎯 **Exam trap** — **Data in use is the hardest state to protect** because the data must be decrypted/plaintext in memory to be processed. FDE and TLS protect rest and transit respectively but do nothing for data being actively used. "Protecting data while it's being processed in memory" → in use → secure enclave / confidential computing.

⚡ **Keyword association** — "stored on disk / in a database / backup" → at rest. "crossing the network / being transmitted" → in transit. "being processed / in RAM / decrypted for use" → in use.

### Data sovereignty & geolocation

| Concept | Definition | Implication |
|---|---|---|
| **Data sovereignty** | Data is subject to the laws of the country where it physically resides. | A cloud region's *location* dictates which government can compel/regulate that data (GDPR, FFIEC, national data-residency laws). Choose regions deliberately. |
| **Geolocation** | Determining/using the physical location of a device or data. | Drives access decisions (geofencing) and compliance (keep data in-country). |
| **Geographic restrictions** | Limiting access based on physical location (geofencing, IP-geo blocking). | Block logins from disallowed countries; keep regulated data within borders. |

🎯 **Exam trap** — **Data sovereignty is about *which laws apply* based on physical location**, not about encryption. If a scenario says "the regulator requires citizen data never leave the country," the control is choosing an **in-country cloud region / geographic restriction**, driven by **data sovereignty**.

### Methods to secure data

| Method | What it does | Reversible? | Best for |
|---|---|---|---|
| **Geographic restrictions** | Limit access/storage by location. | N/A | Compliance, geofencing logins. |
| **Encryption** | Algorithmically scrambles data; reversible with the key. | Yes (with key) | Confidentiality at rest/in transit. |
| **Hashing** | One-way digest for integrity/verification. | No | Integrity checks, password storage. |
| **Masking** | Hides part of the data for display (last-4 of an SSN). | Usually no | Showing data to those who don't need the full value. |
| **Tokenization** | Replaces sensitive value with a random token mapped in a vault. | Yes (via vault) | PCI scope reduction; no algorithmic link to original. |
| **Obfuscation** | Makes data harder to interpret (broad umbrella incl. steganography). | Varies | Hiding meaning/existence. |
| **Segmentation** | Isolates data/systems into separated zones. | N/A | Limiting blast radius and access. |
| **Permission restrictions** | Access controls (ACLs, RBAC) limiting who can touch the data. | N/A | Enforcing least privilege on the data. |

🎯 **Exam trap** — These overlap with [[Domain 1 — General Security Concepts]] crypto. Keep the distinctions sharp: **encryption** is reversible *with a key* (confidentiality); **hashing** is one-way (integrity); **tokenization** has *no algorithmic relationship* (vault-mapped, PCI favorite); **masking** hides characters for display. "Remove card numbers from PCI scope with random stand-ins" → tokenization. "Show only the last four digits" → masking. "Prove the file wasn't altered" → hashing.

📌 **Scenario pattern** — "A support rep must verify a customer's identity but should never see the full SSN; the app shows ***-**-1234." → **Data masking** (display-time, not reversible by the rep).

---

## 3.4 Resilience & recovery

This objective is about surviving failure: keeping running (HA), recovering when you can't (sites/backups), and keeping the lights on (power). Think in terms of **RTO/RPO** trade-offs (recovery speed vs. data loss) — that framing decides most answers.

### High availability — load balancing vs. clustering

| | **Load balancing** | **Clustering** |
|---|---|---|
| Purpose | Distribute incoming traffic across multiple **independent** servers | Group servers so they act as **one logical system** with failover |
| Goal | Performance + availability (spread the load) | Availability (a node fails, another takes over the workload/state) |
| Node awareness | Members don't need to know each other | Nodes are coordinated/aware (shared state, heartbeat) |
| Example | Web servers behind an L4/L7 LB | Database failover cluster, ESXi HA cluster |

🎯 **Exam trap** — **Load balancing vs. clustering.** Load balancing *spreads traffic* across servers (primarily performance, plus availability). Clustering makes nodes *act as one* and *fail over* (primarily availability/continuity, often with shared state). "Distribute load across web servers" → load balancing. "If the active database node dies, the standby takes over seamlessly" → clustering.

### Site considerations (recovery sites)

| Site | Equipment | Data | Time to recover | Cost |
|---|---|---|---|---|
| **Hot** | Fully equipped, running, mirrored | Live/current | Minutes — near-immediate | Highest |
| **Warm** | Equipped (hardware/connectivity) but not live | Needs recent restore/sync | Hours to a day | Medium |
| **Cold** | Empty space, power/cooling only | None — bring everything | Days to weeks | Lowest |
| **Geographic dispersion** | Sites spread across distant regions | — | — | Mitigates regional disasters |

🎯 **Exam trap** — **Hot vs. warm vs. cold** is a classic cost-vs-recovery-time trade-off. **Hot = fastest recovery, highest cost** (running and current). **Cold = cheapest, slowest** (an empty room). **Warm = the middle** (hardware ready, data needs restoring). If the scenario emphasizes "near-zero downtime, budget no object" → hot; "cheapest, can tolerate days down" → cold.

🎯 **Exam trap** — **Geographic dispersion** specifically defeats a *regional* disaster (hurricane, regional power loss). A hot site *next door* to the primary doesn't help if the whole region floods — that's why dispersion (distance) is its own consideration.

⚡ **Keyword association** — "running, mirrored, switch in minutes" → hot. "empty room, power only, cheapest" → cold. "hardware ready, restore data, hours" → warm. "spread far apart to survive a regional event" → geographic dispersion.

### Platform diversity & multi-cloud

| Concept | Definition | Why |
|---|---|---|
| **Platform diversity** | Using different vendors/technologies for the same function (different OS, firewall vendors, hypervisors). | A single vulnerability or vendor failure doesn't take down everything — reduces correlated/monoculture risk. |
| **Multi-cloud** | Spreading workloads across multiple cloud providers (AWS + Azure + GCP). | Avoids vendor lock-in and provider-wide outages; adds resilience (and management complexity). |

🎯 **Exam trap** — **Platform diversity defeats monoculture risk.** If every server runs the same OS/firmware, one exploit owns the fleet. Diversity (and multi-cloud) trades simplicity for resilience against a single common failure. Downside: more complexity to manage and patch.

### Continuity of operations (COOP)

The plan and capability to keep *essential functions* running during and after a disruption — the umbrella that pulls together BCP/DRP, alternate sites, and capacity planning. It answers "how does the mission continue when normal operations are unavailable?" Tie to BIA/RTO/RPO in [[Domain 5 — Security Program Management & Oversight]].

### Capacity planning

Forecasting and provisioning enough resources to meet demand — including during and after a disruption. Three dimensions:

| Dimension | What you plan for |
|---|---|
| **People** | Enough trained staff (and cross-training) to run/recover operations. |
| **Technology** | Enough compute/storage/network/licenses to carry the load and surge. |
| **Infrastructure** | Enough facility, power, cooling, and space to host it. |

🎯 **Exam trap** — Capacity planning includes **people**, not just hardware. A recovery plan that provisions servers but has no trained staff to run them fails. If a scenario stresses "we had the equipment but no one trained to operate it," that's a **people capacity-planning** gap.

### Testing

Ordered roughly from least to most disruptive/realistic:

| Test | What happens | Disruption | Realism |
|---|---|---|---|
| **Tabletop exercise** | Team talks through the plan in a meeting; no systems touched. | None | Low — discussion only |
| **Simulation** | A realistic scenario is enacted/drilled (e.g., a simulated phishing or disaster) without real impact to production. | Low–medium | Medium |
| **Parallel processing** | The recovery site is brought up and runs *alongside* production to validate it works — production stays primary. | Medium | High |
| **Failover** | Actually cut production over to the backup/recovery system to prove it takes the load. | High | Highest |

🎯 **Exam trap** — **Tabletop vs. failover.** A **tabletop** is a *discussion* — no systems are touched, lowest cost/risk, used to validate the plan on paper. A **failover** test *actually switches* production to the backup — highest realism and highest risk. "Walk through the plan in a conference room" → tabletop. "Actually cut over to the DR site" → failover. **Parallel** runs the DR site *alongside* prod without cutting over.

⚡ **Keyword association** — "discuss in a meeting, no systems" → tabletop. "run DR alongside prod" → parallel processing. "actually switch to backup" → failover. "enact a realistic scenario/drill" → simulation.

### Backups

| Concept | Definition | Note |
|---|---|---|
| **Onsite** | Backup kept locally. | Fast restore; lost if the site is destroyed. |
| **Offsite** | Backup kept at a remote location. | Survives a site disaster; slower to retrieve. |
| **Frequency** | How often backups run. | Drives **RPO** — more frequent = less data loss. |
| **Encryption** | Encrypting backup data. | Protects backups at rest/in transit (a stolen backup tape is a breach). |
| **Snapshots** | Point-in-time capture of a system/volume state. | Fast rollback (VMs, storage); not a substitute for full backups. |
| **Recovery** | The restore process itself. | Untested backups = unknown recovery; test restores. |
| **Replication** | Continuously copying data to another system/site (often real-time). | Low RPO/HA; but replicates corruption/ransomware too. |
| **Journaling** | Logging changes so you can replay/roll back to a consistent point. | Enables precise recovery and integrity (DB/filesystem journals). |

🎯 **Exam trap** — **Replication/snapshots are NOT a complete backup strategy.** Replication faithfully copies *everything* — including ransomware encryption or accidental deletion — to the replica in real time. A true backup is a *separate, point-in-time, ideally offline/immutable* copy. If a scenario says "ransomware encrypted prod and the replica too," the lesson is they needed real (offline/offsite, versioned) backups, not just replication. This is the spirit of the **3-2-1 rule** (3 copies, 2 media, 1 offsite).

🎯 **Exam trap** — **Backup frequency sets your RPO; recovery time/site sets your RTO.** "How much data can we afford to lose?" → frequency/RPO. "How fast must we be back up?" → RTO (sites, recovery method).

⚡ **Keyword association** — "real-time copy to another site, copies corruption too" → replication. "point-in-time VM rollback" → snapshot. "how often we back up = how much data we can lose" → frequency/RPO. "stolen backup tape is readable" → unencrypted backups.

### Power

| Component | Definition | Role |
|---|---|---|
| **UPS** | Uninterruptible Power Supply — battery providing **immediate, short-term** power on an outage. | Bridges the gap until the generator starts; allows graceful shutdown. |
| **Generator** | Engine (often diesel/gas) providing **sustained, long-term** power. | Carries the load for extended outages; takes seconds to start. |

🎯 **Exam trap** — **UPS vs. generator** is about duration. A **UPS** is *instant but short* (battery, minutes) — its job is to cover the moment of failure and the seconds until the generator spins up. A **generator** is *sustained but not instant* (takes seconds to start, runs for hours/days on fuel). You need **both**: UPS for the gap, generator for the long haul. "Keeps systems up the instant power drops" → UPS. "Powers the datacenter for a multi-day outage" → generator.

---

## 🎯 Top exam traps

1. **Shared responsibility: data and identity are ALWAYS the customer's**, in every model including SaaS. The provider never owns your data classification or IAM.
2. **Fail-open vs. fail-closed is a security-vs-availability decision, and context flips it.** Data security → fail-closed; life-safety egress → fail-open. Read what the scenario values.
3. **IDS vs. IPS:** IDS = detect/alert, passive, out-of-band (tap). IPS = detect/block, active, inline. Only an inline/active device can stop traffic.
4. **Air-gap (no physical connection) vs. logical segmentation (VLANs on shared media).** Strongest isolation → air-gap; cheap separation → VLAN.
5. **Containers share the host kernel (weaker isolation, breakout); VMs have their own OS (hypervisor risk = VM escape).**
6. **ICS/SCADA invert CIA — availability/safety first**, often unpatchable → compensating controls, not forced patching.
7. **WAF (web-only, stops SQLi/XSS) vs. NGFW (broad L4–L7 + IPS) vs. UTM (SMB all-in-one).** Web app protection → WAF.
8. **L4 sees ports/IPs/state, not content; L7 sees the payload.** Block by port → L4; block a SQLi string → L7.
9. **IPsec (L3, all traffic, site-to-site) vs. TLS/SSL VPN (clientless, port 443, remote user). SD-WAN (routing) vs. SASE (SD-WAN + cloud security).**
10. **802.1X = port-based NAC (when/where); EAP = the auth framework it carries (how).** EAP-TLS is the strong cert-based variant.
11. **Data in use is the hardest state to protect** (must be plaintext in memory) → secure enclave/confidential computing.
12. **Data sovereignty = which laws apply based on physical location**, solved by region choice/geographic restriction, not encryption.
13. **Hot/warm/cold = recovery-speed vs. cost.** Geographic dispersion defeats *regional* disasters specifically.
14. **Load balancing (spread traffic) vs. clustering (nodes act as one, fail over).** Tabletop (discuss) vs. failover (actually cut over) vs. parallel (run alongside).
15. **Replication/snapshots aren't real backups — they copy corruption/ransomware too.** Frequency sets RPO; recovery method/site sets RTO. UPS = instant/short; generator = sustained/delayed.

## 🧠 Mnemonic pack

- **Cloud — what the customer always owns: "Data Is Always Mine"**
  - **D**ata → **D**ata
  - **I**s → **I**dentity
  - **A**lways → **A**ccess
  - **M**ine → **M**anagement of the above (your config/policy)
  - ✅ Maps to the customer-retained responsibilities — Data, Identity, Access — in every cloud model (IaaS/PaaS/SaaS). Correct.

- **Failure modes — "Open = Online, Closed = Cordoned"**
  - **Open** → traffic stays **Online** (availability wins; control stops enforcing)
  - **Closed** → traffic is **Cordoned** off / blocked (security wins; nothing flows)
  - ✅ Correctly maps fail-**open** to continued availability and fail-**closed** to blocked/secured traffic.

- **IDS vs. IPS — "Detection Stays Side, Prevention Plants Path"**
  - **D**etection (ID**S**) → **S**ide / out-of-band tap, passive, alerts only
  - **P**revention (IP**S**) → **P**ath / inline, active, blocks
  - ✅ IDS = out-of-band/passive (Side), IPS = inline/active (Path). Correct.

- **Recovery sites by speed/cost — "Hot Helps Hastily, Cold Costs Cheaply"**
  - **Hot** → **H**elps **H**astily (fastest recovery, highest cost, running + current)
  - **Cold** → **C**osts **C**heaply (slowest recovery, lowest cost, empty room)
  - (Warm sits between — hardware ready, data needs restoring.)
  - ✅ Maps hot to fast/expensive and cold to slow/cheap, with warm in the middle. Correct.

- **DR test escalation — "Talk, Simulate, Pair, Pull"**
  - **T**alk → **T**abletop (discussion only, no systems)
  - **S**imulate → **S**imulation (enact a realistic scenario)
  - **P**air → **P**arallel processing (run DR alongside prod)
  - **P**ull → **Pull** the switch = **F**ailover (actually cut over to backup)
  - ✅ Orders the four tests from least to most disruptive: Tabletop, Simulation, Parallel, Failover. Correct.

- **Power continuity — "UPS Until Power Steadies"**
  - **UPS** → bridges **Until** the generator starts (instant, short, battery)
  - **Generator** → provides the **Steady**, sustained power (delayed start, long runtime)
  - ✅ UPS = instant/short bridge; generator = sustained long-haul. Correct.

## ✅ Self-check

1. A bank migrates its email to a SaaS provider. After a breach, the post-mortem finds attackers used phished credentials because MFA wasn't enforced. Whose responsibility was the failed control, and what principle explains it?
2. An inline IPS protecting a public e-commerce site loses power. The business says uptime is the top priority over inspection. Should it fail-open or fail-closed, and why?
3. Your team needs a device that can *detect* malicious traffic without any risk of dropping legitimate packets or adding latency. IDS or IPS, and inline or tap?
4. A critical turbine controller in a manufacturing plant has a known vulnerability, but the vendor's patch requires a reboot that would halt production and isn't safety-certified. What control approach fits, and which CIA property is being prioritized?
5. What is the key isolation difference between a container and a virtual machine, and what's the corresponding breakout risk for each?
6. A web application is being hit with SQL injection and cross-site scripting. Which firewall type is purpose-built to stop this, and at which OSI layer does it operate?
7. You must build a VPN that encrypts *all* IP traffic between two branch offices at the network layer. Which technology — IPsec or TLS/SSL VPN?
8. A regulator mandates that citizen financial data never physically leave the country. What concept drives this requirement, and what's the practical control?
9. Which data state is the hardest to protect, why, and what technology addresses it?
10. A support agent must confirm a customer's identity but the app must never display the full SSN — it shows ***-**-6789. Which data-protection method is this?
11. Distinguish load balancing from clustering in one sentence each.
12. A company wants the cheapest recovery site and can tolerate being down for several days while it ships in equipment. Which site type?
13. Ransomware encrypts the production file server, and because of real-time replication the encrypted files immediately propagate to the secondary site. What backup principle did they violate?
14. Walk the four DR tests from least to most disruptive and name what each one actually does.
15. During a 12-hour grid outage, which power component keeps systems alive for the first few seconds, and which carries them for the remaining hours?

<details>
<summary>Answers</summary>

1. The **customer's** responsibility. **Shared responsibility model** — data and identity/access are always the customer's, in every model including SaaS. The provider secured the platform; the customer owned MFA/Conditional Access.
2. **Fail-open** — when availability is the stated priority, the control stops enforcing so traffic keeps flowing (accepting temporary loss of inspection). Fail-closed would block all traffic and cause the outage the business wants to avoid.
3. **IDS** (detect/alert only, passive) on a **tap/SPAN (out-of-band)** so it sees a copy of traffic and cannot affect the live path. An inline IPS could drop packets and add latency.
4. **Compensating controls** — isolate/segment the controller, restrict access, and monitor it, since patching isn't feasible. The prioritized property is **Availability** (and safety); ICS/SCADA inverts the usual CIA ordering.
5. A **container shares the host OS kernel** (lighter, weaker isolation; risk = container **breakout** to the host). A **VM has its own guest OS** on a hypervisor (stronger isolation; risk = **VM escape** from guest to host/other guests).
6. A **WAF** (Web Application Firewall), operating at **Layer 7** (application), inspecting HTTP/S payloads for SQLi/XSS.
7. **IPsec** — it operates at the network layer (L3) and protects all IP traffic in a site-to-site tunnel. TLS/SSL VPN is application/transport-layer and typically for remote-user/clientless access.
8. **Data sovereignty** (data is subject to the laws of the country where it physically resides). The practical control is choosing an **in-country cloud region / geographic restriction** to keep the data within borders.
9. **In use** — the data must be decrypted to plaintext in memory/CPU to be processed, so encryption at rest and in transit don't help. **Secure enclaves / confidential computing** address it.
10. **Data masking** (display-time obscuring of part of the value; not reversible by the agent).
11. **Load balancing** distributes incoming traffic across multiple independent servers for performance and availability. **Clustering** groups coordinated nodes to act as one logical system so a failed node fails over to another.
12. **Cold site** — cheapest, just space/power/cooling, slowest recovery (days/weeks to bring in equipment and data).
13. They relied on **replication/snapshots as if they were backups** — replication copies corruption/ransomware in real time. They needed true, separate, point-in-time, **offline/offsite/immutable** backups (the 3-2-1 principle).
14. **Tabletop** (discuss the plan in a meeting, no systems touched) → **Simulation** (enact a realistic scenario/drill without production impact) → **Parallel processing** (run the DR site alongside production to validate it) → **Failover** (actually cut production over to the backup).
15. The **UPS** (battery) covers the first few seconds instantly; the **generator** starts within seconds and carries the load for the remaining hours.

</details>

⬅ [[Security+ SY0-701 — Course Index]] | ➡ Next: [[Domain 4 — Security Operations]]
