# Acronym Master List — Security+ SY0-701

Exhaustive, domain-grouped reference for every testable acronym on SY0-701. Where the same letters mean two different things (MAC, RA, EF), both are listed and disambiguated. Use this as the lookup layer behind the concept notes — memorize the expansion *and* the one-line distinction, because CompTIA writes distractors out of the look-alikes.

> 💡 Callout key: 💡 = exam-relevant nuance · ⚠️ = common trap / distractor bait

---

## Domain 1 — General Security Concepts

| Acronym | Expansion | One-line plain-English meaning |
|---|---|---|
| CIA | Confidentiality, Integrity, Availability | The three properties every control ultimately protects. |
| AAA | Authentication, Authorization, Accounting | Prove who you are, get what you're allowed, log what you did. |
| MAC | Mandatory Access Control | OS/labels enforce access; users can't change permissions (Bell-LaPadula style). |
| DAC | Discretionary Access Control | Resource owner decides who gets access (NTFS ACLs are the classic example). |
| RBAC | Role-Based Access Control | Permissions attach to roles, users get roles — the IGA default. |
| ABAC | Attribute-Based Access Control | Access decided by attributes/policy (user, resource, environment) — Conditional Access territory. |
| RuBAC | Rule-Based Access Control | Access governed by explicit if/then rules (firewall-style), independent of identity role. |
| PoLP / POLP | Principle of Least Privilege | Grant the minimum access needed, nothing more. |
| ACL | Access Control List | Ordered list of allow/deny entries on a resource or interface. |
| SoD | Separation of Duties | Split a sensitive task so no single person controls it end-to-end. |
| ZT | Zero Trust | Never trust by location; verify every request explicitly. |
| PEP | Policy Enforcement Point | The gate that actually allows or blocks a request in a Zero Trust model. |
| PDP | Policy Decision Point | The brain that evaluates policy and tells the PEP yes/no. |
| PA | Policy Administrator | Component that issues the PEP's allow/deny based on the PDP's verdict. |
| PKI | Public Key Infrastructure | The whole system of CAs, keys, and certs that makes asymmetric trust work. |
| CA | Certificate Authority | Trusted issuer that signs certificates. |
| RA | Registration Authority | Vets identities and forwards requests to the CA (offloads CA verification). |
| CRL | Certificate Revocation List | Published list of certs the CA has revoked. |
| OCSP | Online Certificate Status Protocol | Real-time "is this cert still good?" query (replaces fetching a full CRL). |
| CSR | Certificate Signing Request | The request you send a CA containing your public key and identity. |
| PKCS | Public Key Cryptography Standards | Family of crypto formats (PKCS#12 for cert+key bundles, PKCS#7 for signed data). |
| OID | Object Identifier | Dotted-number used to name cert attributes and policies. |
| CN | Common Name | The primary hostname/identity field in a certificate's subject. |
| SAN | Subject Alternative Name | Cert field listing additional names/IPs the cert is valid for. |
| TPM | Trusted Platform Module | Onboard chip that stores keys and supports measured boot / FDE. |
| HSM | Hardware Security Module | Dedicated tamper-resistant appliance for key generation and storage. |
| KMS | Key Management Service / System | Centralized lifecycle management of cryptographic keys. |
| FDE | Full Disk Encryption | Encrypts the entire volume (BitLocker, LUKS). |
| SED | Self-Encrypting Drive | Drive that encrypts in hardware at the controller level. |
| OPAL | (TCG storage spec, not an acronym expansion tested) | Standard governing SED key management. |
| IV | Initialization Vector | Random/nonce input that makes identical plaintexts encrypt differently. |
| TOCTOU | Time-of-Check to Time-of-Use | Race condition where state changes between validation and use. |

💡 MAC appears twice on the exam (access control vs. addressing) and once more in crypto (Message Authentication Code) — context is everything. ⚠️ RA also appears twice: Registration Authority here vs. Recovery Agent in key management.

---

## Domain 2 — Threats, Vulnerabilities, and Mitigations

| Acronym | Expansion | One-line plain-English meaning |
|---|---|---|
| APT | Advanced Persistent Threat | Well-resourced attacker (often nation-state) that stays in quietly for the long haul. |
| BEC | Business Email Compromise | Fraud via spoofed/hijacked exec email to redirect money or data. |
| IoC | Indicator of Compromise | Forensic breadcrumb (hash, IP, domain) showing a system was hit. |
| TTP | Tactics, Techniques, and Procedures | The how/why of attacker behavior (MITRE ATT&CK vocabulary). |
| CVE | Common Vulnerabilities and Exposures | The unique public ID for a specific known vulnerability. |
| CVSS | Common Vulnerability Scoring System | 0–10 severity score for a vulnerability. |
| CWE | Common Weakness Enumeration | Catalog of vulnerability *types* (the class, not the instance). |
| CCE | Common Configuration Enumeration | Standardized IDs for system misconfigurations. |
| CPE | Common Platform Enumeration | Standardized naming for products/platforms in scan data. |
| OSINT | Open-Source Intelligence | Recon from publicly available sources. |
| TI | Threat Intelligence | Curated, actionable info about threats and actors. |
| ISAC | Information Sharing and Analysis Center | Sector-specific threat-sharing org (FS-ISAC for banking). |
| TAXII | Trusted Automated eXchange of Indicator Information | Transport protocol for sharing threat intel. |
| STIX | Structured Threat Information eXpression | Standardized format/language for threat intel. |
| SQLi | SQL Injection | Injecting SQL through input to manipulate the database. |
| XSS | Cross-Site Scripting | Injecting script that runs in another user's browser session. |
| CSRF / XSRF | Cross-Site Request Forgery | Tricking a logged-in user's browser into making unwanted requests. |
| SSRF | Server-Side Request Forgery | Forcing the server to make attacker-chosen requests (cloud metadata theft). |
| RCE | Remote Code Execution | Attacker runs arbitrary code on the target remotely. |
| LFI / RFI | Local / Remote File Inclusion | Tricking an app into including unauthorized local or remote files. |
| XXE | XML External Entity | Abusing XML parsers to read files or reach internal services. |
| DoS | Denial of Service | Overwhelming a system so it can't serve legitimate users. |
| DDoS | Distributed Denial of Service | DoS launched from many sources at once (botnet). |
| RAT | Remote Access Trojan | Malware giving an attacker hands-on remote control. |
| C2 / C&C | Command and Control | The infrastructure malware phones home to for orders. |
| PUP | Potentially Unwanted Program | Borderline software (adware/toolbars) bundled with installs. |
| UEBA | User and Entity Behavior Analytics | Baselines normal behavior to flag anomalies/insider threats. |
| MITM / OtP | Man-in-the-Middle / On-Path | Attacker silently relays/alters traffic between two parties. |
| ARP | Address Resolution Protocol | Maps IP to MAC on a LAN — spoofable for on-path attacks. |
| TOR | The Onion Router | Anonymizing overlay network; relevant to threat actor obfuscation. |

⚠️ CWE (weakness *type*) vs. CVE (specific instance) is a frequent swap on the exam. CVSS scores the CVE; it does not enumerate it.

---

## Domain 3 — Security Architecture

| Acronym | Expansion | One-line plain-English meaning |
|---|---|---|
| NGFW | Next-Generation Firewall | Firewall with app awareness, IPS, and deep inspection built in. |
| UTM | Unified Threat Management | All-in-one appliance bundling firewall, AV, IPS, filtering. |
| WAF | Web Application Firewall | Filters HTTP/S traffic to protect web apps (blocks SQLi/XSS). |
| IDS | Intrusion Detection System | Detects and alerts on malicious activity (passive). |
| IPS | Intrusion Prevention System | Detects *and blocks* inline (active). |
| HIDS / HIPS | Host-based IDS / IPS | Detection/prevention running on an individual host. |
| NIDS / NIPS | Network-based IDS / IPS | Detection/prevention monitoring network traffic. |
| NAC | Network Access Control | Gatekeeps device admission based on posture/identity (802.1X). |
| DLP | Data Loss Prevention | Detects and blocks sensitive data leaving the org. |
| FIM | File Integrity Monitoring | Alerts when protected files change unexpectedly. |
| SIEM | Security Information and Event Management | Central log aggregation, correlation, and alerting. |
| SOAR | Security Orchestration, Automation, and Response | Automates and orchestrates incident response playbooks. |
| EDR | Endpoint Detection and Response | Deep endpoint telemetry + response capability. |
| XDR | Extended Detection and Response | Correlates detection across endpoint, network, cloud, identity. |
| MDR | Managed Detection and Response | Outsourced/managed EDR+monitoring service. |
| NAT | Network Address Translation | Rewrites IPs so private hosts share public addresses. |
| PAT | Port Address Translation | NAT overload — many hosts behind one public IP via ports. |
| VLAN | Virtual Local Area Network | Logical network segmentation on a switch. |
| VPN | Virtual Private Network | Encrypted tunnel across an untrusted network. |
| IPSec | Internet Protocol Security | Suite that encrypts/authenticates IP traffic (VPN backbone). |
| IKE | Internet Key Exchange | Negotiates the IPSec security association and keys. |
| ESP | Encapsulating Security Payload | IPSec component providing encryption + integrity. |
| AH | Authentication Header | IPSec component providing integrity/auth only (no encryption). |
| SSL | Secure Sockets Layer | Deprecated predecessor to TLS (term still tested). |
| TLS | Transport Layer Security | Standard protocol encrypting data in transit. |
| SSH | Secure Shell | Encrypted remote shell and tunneling protocol. |
| DTLS | Datagram TLS | TLS for UDP/datagram traffic. |
| SD-WAN | Software-Defined Wide Area Network | Centralized, policy-driven WAN over commodity links. |
| SASE | Secure Access Service Edge | Cloud-delivered convergence of SD-WAN + ZTNA + SWG + CASB. |
| CASB | Cloud Access Security Broker | Policy enforcement point sitting between users and cloud apps. |
| SWG | Secure Web Gateway | Filters and inspects outbound web traffic. |
| ZTNA | Zero Trust Network Access | Per-session, identity-aware access replacing implicit VPN trust. |
| DMZ / Screened Subnet | Demilitarized Zone | Buffer network for public-facing services. |
| DNS | Domain Name System | Resolves names to IP addresses. |
| DNSSEC | DNS Security Extensions | Signs DNS records to prevent spoofing/poisoning. |
| SPF | Sender Policy Framework | DNS record listing who may send mail for a domain. |
| DKIM | DomainKeys Identified Mail | Cryptographically signs outbound mail to prove authenticity. |
| DMARC | Domain-based Message Authentication, Reporting & Conformance | Policy tying SPF+DKIM together and reporting failures. |
| SMTP | Simple Mail Transfer Protocol | Protocol for sending email. |
| S/MIME | Secure/Multipurpose Internet Mail Extensions | Signs and encrypts email using certificates. |
| FDE | Full Disk Encryption | (Architecture context) volume-level encryption for data at rest. |
| OT | Operational Technology | Hardware/software that runs physical industrial processes. |
| ICS | Industrial Control System | Umbrella for systems controlling industrial equipment. |
| SCADA | Supervisory Control and Data Acquisition | Large-scale ICS for monitoring distributed infrastructure. |
| PLC | Programmable Logic Controller | Ruggedized controller running industrial logic. |
| HMI | Human-Machine Interface | Operator screen/panel for an ICS/SCADA system. |
| RTOS | Real-Time Operating System | OS guaranteeing deterministic timing for embedded/ICS use. |
| IoT | Internet of Things | Network-connected embedded smart devices. |
| IaC | Infrastructure as Code | Provisioning infrastructure via declarative code/templates. |
| IaaS | Infrastructure as a Service | Cloud model renting raw compute/storage/network. |
| PaaS | Platform as a Service | Cloud model providing a managed app platform/runtime. |
| SaaS | Software as a Service | Cloud model delivering finished applications. |
| CSP | Cloud Service Provider | The vendor delivering cloud services (AWS/Azure/GCP). |
| VM | Virtual Machine | Software-emulated computer running on a hypervisor. |
| VDI | Virtual Desktop Infrastructure | Centrally hosted virtual desktops streamed to users. |
| CI/CD | Continuous Integration / Continuous Delivery (Deployment) | Automated pipeline to build, test, and ship code. |
| API | Application Programming Interface | Defined interface for software to talk to software. |
| SDN | Software-Defined Networking | Decouples network control plane from the data plane. |
| GPO | Group Policy Object | Windows mechanism to push config/security settings at scale. |
| SELinux | Security-Enhanced Linux | Kernel MAC framework enforcing mandatory policy on Linux. |
| SCAP | Security Content Automation Protocol | Standard for automated config/vuln compliance checking. |
| SNMP | Simple Network Management Protocol | Monitors/manages network devices (use v3 for security). |
| NetFlow | (Cisco flow protocol) | Records network flow metadata for traffic analysis. |
| PCAP | Packet Capture | Full captured packet data for deep analysis. |
| RAID | Redundant Array of Independent Disks | Combines disks for redundancy and/or performance. |
| UPS | Uninterruptible Power Supply | Battery backup bridging short power loss. |
| PDU | Power Distribution Unit | Manages/distributes rack power. |
| HVAC | Heating, Ventilation, and Air Conditioning | Environmental control protecting equipment availability. |
| EMI / EMP | Electromagnetic Interference / Pulse | Electromagnetic energy that disrupts or destroys electronics. |
| SoC | System on a Chip | Full computer integrated onto a single chip (embedded). |
| FPGA | Field-Programmable Gate Array | Reconfigurable hardware logic chip. |

💡 SASE is the umbrella; ZTNA, SWG, CASB, and SD-WAN are the components inside it — the exam tests that nesting.

---

## Domain 4 — Security Operations

| Acronym | Expansion | One-line plain-English meaning |
|---|---|---|
| MFA | Multi-Factor Authentication | Two+ factors from different categories (know/have/are). |
| 2FA | Two-Factor Authentication | Exactly two factors — a subset of MFA. |
| SSO | Single Sign-On | One authentication grants access to many services. |
| SAML | Security Assertion Markup Language | XML-based federation standard (enterprise web SSO). |
| OAuth | Open Authorization | Delegated *authorization* framework (grants access, not identity). |
| OIDC | OpenID Connect | Identity/authentication layer built on top of OAuth 2.0. |
| LDAP | Lightweight Directory Access Protocol | Protocol to query/modify directory services (AD). |
| LDAPS | LDAP over SSL/TLS | Encrypted LDAP. |
| RADIUS | Remote Authentication Dial-In User Service | Centralized AAA; encrypts only the password. |
| TACACS+ | Terminal Access Controller Access-Control System Plus | Cisco AAA; encrypts the full payload, separates AuthN/AuthZ. |
| EAP | Extensible Authentication Protocol | Framework that carries many auth methods over 802.1X. |
| PEAP | Protected EAP | Wraps EAP inside a server-side TLS tunnel. |
| EAP-TLS | EAP-Transport Layer Security | Strongest EAP — mutual cert-based auth both sides. |
| EAP-TTLS | EAP-Tunneled TLS | Server cert + tunneled legacy client auth. |
| 802.1X | (IEEE port-based NAC standard) | Port-based authentication before granting network access. |
| Kerberos | (named after Cerberus) | Ticket-based SSO auth protocol (AD's default). |
| TGT | Ticket-Granting Ticket | Kerberos credential used to request service tickets. |
| KDC | Key Distribution Center | Kerberos server issuing tickets (AuthN + TGS). |
| MFA | (see above) | — |
| OTP | One-Time Password | Single-use code for authentication. |
| TOTP | Time-based One-Time Password | OTP that rotates on a clock (authenticator apps). |
| HOTP | HMAC-based One-Time Password | OTP based on an incrementing counter. |
| FIDO2 | Fast Identity Online 2 | Passwordless/phishing-resistant auth standard (passkeys). |
| PAM | Privileged Access Management | Controls, vaults, and audits privileged accounts. |
| IdP | Identity Provider | System that authenticates users and issues assertions. |
| SP | Service Provider | The relying app that trusts the IdP's assertion. |
| JIT | Just-in-Time (access/provisioning) | Grant privileged access only for the moment it's needed. |
| SCIM | System for Cross-domain Identity Management | Standard for automated user provisioning/deprovisioning. |
| IRP | Incident Response Plan | Documented process for handling security incidents. |
| CSIRT / CIRT | Computer Security Incident Response Team | The team that executes incident response. |
| SOC | Security Operations Center | Team/facility that monitors and defends 24/7. |
| MTTR | Mean Time to Repair/Respond | Average time to fix or recover from an incident. |
| MTBF | Mean Time Between Failures | Average uptime between failures (repairable systems). |
| MTTF | Mean Time to Failure | Average lifespan of a non-repairable component. |
| RTO | Recovery Time Objective | Max tolerable downtime before recovery. |
| RPO | Recovery Point Objective | Max tolerable data loss measured in time. |
| WORM | Write Once, Read Many | Immutable storage for tamper-proof logs/retention. |
| CHAP | Challenge-Handshake Authentication Protocol | Auth using a hashed challenge/response (no cleartext password). |
| PAP | Password Authentication Protocol | Legacy cleartext password auth (insecure). |
| MDM | Mobile Device Management | Centralized control/policy for mobile endpoints. |
| MAM | Mobile Application Management | Manages just the apps/data, not the whole device. |
| BYOD | Bring Your Own Device | Employees use personal devices for work. |
| COPE | Corporate-Owned, Personally Enabled | Company device with allowed personal use. |
| CYOD | Choose Your Own Device | Employee picks from a company-approved list. |
| COBO | Corporate-Owned, Business Only | Locked-down company device, no personal use. |
| UEM | Unified Endpoint Management | Single platform managing all endpoint types. |
| WPA2 | Wi-Fi Protected Access 2 | Wi-Fi security using AES-CCMP. |
| WPA3 | Wi-Fi Protected Access 3 | Current Wi-Fi standard using SAE. |
| WEP | Wired Equivalent Privacy | Broken legacy Wi-Fi encryption. |
| PSK | Pre-Shared Key | Shared password mode for Wi-Fi auth. |
| SAE | Simultaneous Authentication of Equals | WPA3's handshake replacing PSK's 4-way (Dragonfly). |
| AAA | (see Domain 1) | Also the framework behind RADIUS/TACACS+ in ops. |
| TACACS+ | (see above) | — |

⚠️ OAuth is authorization, OIDC is authentication — CompTIA loves swapping them. SAML is enterprise/XML; OIDC is modern/JSON/REST.

---

## Domain 5 — Security Program Management and Oversight

| Acronym | Expansion | One-line plain-English meaning |
|---|---|---|
| SLE | Single Loss Expectancy | Dollar loss from one occurrence of a risk (AV × EF). |
| ALE | Annualized Loss Expectancy | Expected yearly loss (SLE × ARO). |
| ARO | Annualized Rate of Occurrence | How many times per year the loss is expected. |
| AV | Asset Value | Dollar value of the asset at risk. |
| EF | Exposure Factor | Percent of asset value lost per incident. |
| ROI | Return on Investment | Financial payback justifying a control. |
| ROSI | Return on Security Investment | ROI framed specifically for security spend. |
| BIA | Business Impact Analysis | Identifies critical functions and the cost of losing them. |
| BCP | Business Continuity Plan | Keep the business running during disruption. |
| DRP | Disaster Recovery Plan | Restore IT systems after a disaster. |
| COOP | Continuity of Operations Plan | Government/federal continuity equivalent of BCP. |
| SLA | Service Level Agreement | Contractual performance/uptime commitment. |
| MOU | Memorandum of Understanding | Non-binding statement of mutual intent. |
| MOA | Memorandum of Agreement | More formal, often binding, cooperative agreement. |
| MSA | Master Service Agreement | Umbrella contract governing the overall relationship. |
| SOW | Statement of Work | Defines specific deliverables, scope, timeline. |
| WO | Work Order | Authorizes specific work under an existing agreement. |
| NDA | Non-Disclosure Agreement | Legally binds parties to confidentiality. |
| BPA | Business Partners Agreement | Defines terms between business partners. |
| KPI | Key Performance Indicator | Metric tracking operational performance. |
| KRI | Key Risk Indicator | Metric signaling rising risk exposure. |
| RACI | Responsible, Accountable, Consulted, Informed | Matrix assigning roles for a task/process. |
| GRC | Governance, Risk, and Compliance | The discipline tying policy, risk, and regulation together. |
| FFIEC | Federal Financial Institutions Examination Council | Body setting exam standards for U.S. financial institutions. |
| GLBA | Gramm-Leach-Bliley Act | U.S. law requiring financial institutions to protect customer data. |
| HIPAA | Health Insurance Portability and Accountability Act | U.S. law protecting health information. |
| PCI DSS | Payment Card Industry Data Security Standard | Card-data protection standard for anyone handling card payments. |
| GDPR | General Data Protection Regulation | EU privacy law with strict data-subject rights. |
| CCPA | California Consumer Privacy Act | California state consumer-privacy law. |
| SOX | Sarbanes-Oxley Act | U.S. law on financial reporting integrity/controls. |
| NIST | National Institute of Standards and Technology | U.S. body publishing security frameworks (CSF, 800-series). |
| RMF | Risk Management Framework | NIST's structured process for managing system risk. |
| CSF | Cybersecurity Framework | NIST's Identify/Protect/Detect/Respond/Recover model. |
| ISO | International Organization for Standardization | Issuer of 27001/27002 security management standards. |
| CIS | Center for Internet Security | Publisher of the CIS Controls and Benchmarks. |
| SOC 1 | System and Organization Controls 1 | Audit report on controls over financial reporting. |
| SOC 2 | System and Organization Controls 2 | Audit report on security/availability/confidentiality controls. |
| SOC 3 | System and Organization Controls 3 | Public-facing summary version of a SOC 2. |
| DPO | Data Protection Officer | Role accountable for privacy compliance (GDPR). |
| PII | Personally Identifiable Information | Data that can identify an individual. |
| PHI | Protected Health Information | Health data protected under HIPAA. |
| SPI | Sensitive Personal Information | Heightened-sensitivity personal data category. |
| AUP | Acceptable Use Policy | Rules for permitted use of org systems. |
| RA | Recovery Agent | Authorized party who can recover encrypted data/keys. |

💡 The risk math chain is the heavily tested one: AV × EF = SLE, then SLE × ARO = ALE. ⚠️ MOU (intent) vs. MOA (agreement) vs. MSA (master umbrella) vs. SOW (specific deliverables) — know the gradient.

---

## Cross-cutting / General

These span multiple domains or are foundational cryptography/identity terms you'll see anywhere.

| Acronym | Expansion | One-line plain-English meaning |
|---|---|---|
| MAC | Media Access Control (address) | The hardware burned-in NIC address used on a LAN. |
| MAC | Message Authentication Code | Crypto tag proving integrity + authenticity of a message. |
| HMAC | Hash-based Message Authentication Code | MAC built from a hash + secret key. |
| AES | Advanced Encryption Standard | The modern symmetric block cipher (128/192/256-bit). |
| DES | Data Encryption Standard | Obsolete 56-bit symmetric cipher. |
| 3DES | Triple DES | DES applied three times; deprecated. |
| RSA | Rivest-Shamir-Adleman | Classic asymmetric algorithm for encryption/signatures. |
| ECC | Elliptic Curve Cryptography | Asymmetric crypto with small, efficient keys. |
| ECDSA | Elliptic Curve Digital Signature Algorithm | ECC-based signing algorithm. |
| DH | Diffie-Hellman | Key-exchange method to derive a shared secret. |
| DHE | Diffie-Hellman Ephemeral | DH with throwaway keys for forward secrecy. |
| ECDHE | Elliptic Curve Diffie-Hellman Ephemeral | ECC + ephemeral DH; the modern PFS standard. |
| PFS | Perfect Forward Secrecy | Past sessions stay safe even if the long-term key leaks. |
| SHA | Secure Hash Algorithm | Family of cryptographic hash functions (SHA-256, etc.). |
| MD5 | Message Digest 5 | Broken 128-bit hash; collision-prone. |
| PBKDF2 | Password-Based Key Derivation Function 2 | Stretches passwords into keys via salted iterations. |
| GCM | Galois/Counter Mode | AEAD block-cipher mode (encryption + integrity in one). |
| CBC | Cipher Block Chaining | Block mode chaining each block with the previous. |
| ECB | Electronic Codebook | Weak block mode; identical blocks encrypt identically. |
| CTR | Counter (mode) | Turns a block cipher into a stream cipher. |
| AEAD | Authenticated Encryption with Associated Data | Modes giving confidentiality + integrity together (GCM). |
| KEK | Key Encryption Key | A key used to encrypt other keys. |
| DEK | Data Encryption Key | The key that actually encrypts the data. |
| RFID | Radio-Frequency Identification | Wireless tag/reader tech (badges, asset tags). |
| NFC | Near-Field Communication | Very-short-range wireless (tap-to-pay, pairing). |
| GPS | Global Positioning System | Satellite location used for geolocation/geofencing. |
| OS | Operating System | The base software managing hardware and apps. |
| BIOS | Basic Input/Output System | Legacy firmware that boots the hardware. |
| UEFI | Unified Extensible Firmware Interface | Modern BIOS replacement supporting Secure Boot. |
| DRM | Digital Rights Management | Controls to restrict use/copying of protected content. |
| VLSM / CIDR | Classless Inter-Domain Routing | Flexible IP subnetting notation. |
| TCP / IP | Transmission Control Protocol / Internet Protocol | The core reliable connection + addressing suite. |
| UDP | User Datagram Protocol | Connectionless, low-overhead transport. |
| ICMP | Internet Control Message Protocol | Network diagnostics/error messaging (ping). |
| HTTP / HTTPS | HyperText Transfer Protocol (Secure) | Web protocol; HTTPS is HTTP over TLS. |
| FTP / SFTP / FTPS | File Transfer Protocol (Secure variants) | File transfer; SFTP rides SSH, FTPS rides TLS. |
| RDP | Remote Desktop Protocol | Microsoft's remote graphical session protocol. |
| TFTP | Trivial File Transfer Protocol | Lightweight, unauthenticated UDP file transfer. |
| NTP | Network Time Protocol | Synchronizes clocks across systems (critical for logs/Kerberos). |
| DHCP | Dynamic Host Configuration Protocol | Hands out IP addresses automatically. |
| QoS | Quality of Service | Prioritizes traffic to guarantee performance. |
| CASB | (see Domain 3) | Bridges cloud usage policy across domains. |
| FIM | Federated Identity Management | (Identity context) trusting identities across organizations. |

💡 Note the FIM collision: File Integrity Monitoring (Domain 3) vs. Federated Identity Management (identity) — same letters, unrelated concepts. Likewise RA appears three ways across this list: Registration Authority, Recovery Agent, and (in risk) Risk Assessment.

---

⬅ [[Security+ SY0-701 — Course Index]]
