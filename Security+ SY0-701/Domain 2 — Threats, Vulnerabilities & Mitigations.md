> **Domain 2 — Threats, Vulnerabilities & Mitigations**
> **Exam weight:** 22% (~20 questions on a 90-question form).
> **What it covers:** The attacker side of the exam — who attacks (threat actors and their motivations), how they get in (vectors and attack surfaces), the weaknesses they exploit (vulnerabilities), the fingerprints they leave (indicators of malicious activity), and what you do about it (mitigation techniques).
> **Strategic note:** Second-heaviest domain and the home of every "identify-the-attack" and "identify-the-indicator" scenario. The points here are won by owning four taxonomies cold — threat actors, vectors, vulnerabilities, and indicators — and by distinguishing near-identical attacks (worm vs. virus, vishing vs. smishing, DDoS reflection vs. amplification, spraying vs. brute force). This is vocabulary-and-pattern recognition under time pressure, so drill the keyword triggers in this module until the answer is reflexive.

## Objective map

- **2.1** Threat actors & motivations — actor types, attributes (internal/external, resources, sophistication), and motivations.
- **2.2** Threat vectors & attack surfaces — message/image/file/voice/removable-media/software/network/port/credential/supply-chain vectors, plus human (social-engineering) vectors.
- **2.3** Vulnerabilities — application, OS, web, hardware, virtualization, cloud, supply chain, cryptographic, misconfiguration, mobile, zero-day.
- **2.4** Indicators of malicious activity — malware types, physical/network/application/cryptographic/password attacks, and the indicators that reveal them.
- **2.5** Mitigation techniques — segmentation, access control, allow listing, isolation, patching, encryption, monitoring, least privilege, configuration enforcement, decommissioning, and hardening.

---

## 2.1 Threat actors & motivations

A **threat actor** is any entity — person, group, or organization — that intends to cause a security incident. The exam wants you to fingerprint an actor from a scenario by reading three things: *who they are* (type), *what they bring to the fight* (attributes), and *why they're doing it* (motivation). Match those three and the answer falls out.

### Threat actor types

| Actor | Definition | Sophistication | Resources | Typical motivation |
|---|---|---|---|---|
| **Nation-state** | Government-sponsored group conducting cyber operations against other states, infrastructure, or industry. | Very high | Very high (state budgets) | Espionage, war, service disruption |
| **Unskilled attacker** | Low-skill individual running tools/scripts they didn't write (the term that replaced "script kiddie"). | Low | Low | Chaos, notoriety, philosophical |
| **Hacktivist** | Ideologically driven actor attacking to make a political/social statement. | Low to high | Low to moderate | Philosophical/political beliefs |
| **Insider threat** | A current/former employee, contractor, or partner who misuses authorized access. | Varies (knows the environment) | Internal access (their edge) | Revenge, financial gain, espionage |
| **Organized crime** | Professional criminal enterprise running cybercrime as a business. | High | High (profit-funded) | Financial gain |
| **Shadow IT** | Systems, software, or services deployed by employees *without* IT/security approval. | N/A (not malicious by intent) | Internal | Convenience/productivity, not malice |

🎯 **Exam trap** — **Shadow IT is a threat actor on SY0-701, but it isn't an attacker.** It's employees standing up unsanctioned tools (a rogue SaaS app, a personal cloud share, an unapproved Raspberry Pi) that create unmanaged risk. If a scenario says "a department spun up a service without telling security," that's shadow IT — not an insider attack, not organized crime.

🎯 **Exam trap** — **Insider threat ≠ insider attribute.** The *actor type* "insider threat" is a person misusing access. The *attribute* "internal" describes where any actor sits relative to the org. A nation-state can recruit an insider, blurring the two — read whether the question is asking *who* (type) or *where* (attribute).

⚡ **Keyword association** — "state-sponsored / APT / critical infrastructure / years-long stealth" → nation-state. "script / downloaded tool / no real skill" → unskilled attacker. "protest / ideology / deface to send a message" → hacktivist. "disgruntled employee / former admin still has access" → insider threat. "ransomware-as-a-business / professional / profit" → organized crime. "unapproved app / no IT sign-off" → shadow IT.

📌 **Scenario pattern** — "An attacker maintains covert access to a utility's SCADA network for 18 months, exfiltrating engineering data and showing no interest in money." → **nation-state** (long dwell time, espionage on critical infrastructure, no financial motive).

### Threat actor attributes

| Attribute | What it tells you |
|---|---|
| **Internal vs. external** | *Internal* actors already have some authorized access (employees, contractors). *External* actors must breach the perimeter first. Insiders skip a lot of the kill chain. |
| **Resources / funding** | How much money, infrastructure, and personnel back the actor. Nation-states and organized crime are well-resourced; unskilled attackers are not. |
| **Sophistication / capability** | Technical skill — from running off-the-shelf scripts to writing custom zero-day exploits and supply-chain implants. |

🎯 **Exam trap** — **High sophistication + high resources + stealth → nation-state, even when the question doesn't say "government."** CompTIA describes the actor by attributes and expects you to infer the type. Custom malware, zero-days, and long dwell time are the tells.

🧠 **Mnemonic** — Actor attributes: **"In Real Style"** → **In**ternal/external, **R**esources/funding, **S**ophistication/capability.

### Motivations

| Motivation | What the actor wants |
|---|---|
| **Data exfiltration** | Steal data and remove it from the organization. |
| **Espionage** | Covertly gather intelligence (state or corporate secrets) over time. |
| **Service disruption** | Take systems/services offline (often via DoS). |
| **Blackmail** | Coerce the victim with threats (e.g., leak stolen data unless paid). |
| **Financial gain** | Make money — ransomware, fraud, theft. The dominant criminal motive. |
| **Philosophical / political beliefs** | Advance an ideology or cause (the hacktivist driver). |
| **Ethical** | "White-hat" / authorized testing to *improve* security. |
| **Revenge** | Get back at an employer or target (classic insider driver). |
| **Disruption / chaos** | Cause mayhem for its own sake, no clear gain. |
| **War** | Cyber operations as an instrument of armed/geopolitical conflict. |

🎯 **Exam trap** — **Blackmail vs. financial gain.** Both can involve money, but *blackmail* is coercion-by-threat (pay or I leak); *financial gain* is the broad profit motive (theft, fraud, ransomware payout). Double-extortion ransomware blends them — encrypt for financial gain, threaten to leak for blackmail.

🎯 **Exam trap** — **Ethical is a listed motivation.** An authorized penetration tester or bug-bounty researcher is a "threat actor" by behavior but with an *ethical* motive. If the scenario stresses *authorization* and *improving security*, the motivation is ethical — don't mislabel them as a criminal.

⚡ **Keyword association** — "leak unless you pay" → blackmail. "steal and remove data" → exfiltration. "long-term covert intel gathering" → espionage. "knock it offline" → service disruption. "for the cause / ideology" → philosophical/political. "authorized / improve security" → ethical. "get back at the company" → revenge.

🔬 **Hands-on** — In your bank role, model this with a real insider scenario in your IGA tooling: a departing employee whose access wasn't deprovisioned (revenge/financial motive, internal attribute). Run a SailPoint access-review campaign and an Entra ID access-package expiration to see how identity governance closes the exact gap an insider threat exploits — orphaned and over-provisioned access.

---

## 2.2 Threat vectors & attack surfaces

A **threat vector** is the *path or method* an actor uses to gain access or deliver a payload. The **attack surface** is the sum of all those exposed paths. The exam tests these as "how did it get in?" — recognize the vector from the delivery mechanism.

### Technical & physical vectors

| Vector | What it is | Why it works / exam note |
|---|---|---|
| **Email** | Malicious messages, attachments, or links delivered over email. | The most common vector. Phishing lives here. |
| **SMS (text)** | Malicious texts (links, spoofed sender). | The delivery channel behind smishing. |
| **Instant messaging** | Malicious links/files over Teams, Slack, WhatsApp, etc. | Often trusted more than email → effective. |
| **Image-based** | Malware/exploit hidden in an image file (malformed image triggering a parser flaw, or steganographic payload). | The image *itself* is the vehicle. |
| **File-based** | A malicious file (document, PDF, executable, archive) carrying the payload. | Macro-laden Office docs are the classic. |
| **Voice call (vishing channel)** | Phone calls used to manipulate or deliver instructions. | Human-targeted; see vishing below. |
| **Removable device** | USB drives/external media introducing malware or exfiltrating data. | "Lost USB in the parking lot" drop attacks. |
| **Vulnerable software** | Exploiting flaws in installed software. *Client-based* needs an agent/app installed on the host; *agentless* is exploited remotely with nothing installed on the target. | Know the client-based vs. agentless split. |
| **Unsupported systems/applications** | End-of-life software no longer receiving patches. | No fix will ever come → permanent exposure. |
| **Unsecure networks** | Weak/open **wireless**, exposed **wired** access, or unguarded **Bluetooth**. | Open Wi-Fi, rogue ports, Bluetooth pairing abuse. |
| **Open service ports** | Network ports left listening/exposed that shouldn't be. | Each open port is a potential entry; close unused ones. |
| **Default credentials** | Devices/apps still using factory username/password. | Trivially guessed (admin/admin); change on deploy. |
| **Supply chain** | Compromise reaching you *through* a trusted **MSP, vendor, or supplier**. | You inherit their breach (see SolarWinds-style attacks). |

🎯 **Exam trap** — **Client-based vs. agentless vulnerable software.** *Client-based* = the vulnerable software (or its agent) is **installed on the endpoint** you're attacking. *Agentless* = exploited **remotely with nothing installed on the target** (e.g., a flaw in a web service reachable over the network). "Requires the app on the host" → client-based; "no agent, hit it over the network" → agentless.

🎯 **Exam trap** — **Default credentials vs. weak password.** Default creds are the *factory-set* values never changed (admin/admin, the router's printed password). A weak password is a poorly chosen *user* password. Scenario says "device shipped with and still uses the manufacturer's password" → default credentials.

🎯 **Exam trap** — **Supply chain is both a vector (2.2) and a vulnerability (2.3).** As a *vector* it's the path (the compromised vendor/MSP). As a *vulnerability* it's the weakness (trusting an insecure provider). Same theme, two objective slots — answer to the framing.

⚡ **Keyword association** — "USB dropped / external media" → removable device. "factory password unchanged" → default credentials. "breach came through our MSP/vendor" → supply chain. "no longer patched / end-of-life" → unsupported systems. "listening port that shouldn't be open" → open service ports.

### Human vectors / social engineering

| Technique | Definition | The tell |
|---|---|---|
| **Phishing** | Fraudulent **email** tricking the victim into clicking, opening, or divulging. | Broad email lure. |
| **Vishing** | **Voice** phishing — phone-call manipulation. | "V" = voice. |
| **Smishing** | **SMS** phishing — malicious text messages. | "Sm" = SMS. |
| **Misinformation / disinformation** | Spreading false information. *Mis*information = false but not necessarily deliberate; *dis*information = deliberately false to deceive. | Influence/confuse. |
| **Impersonation** | Pretending to be someone else (a known person, a help-desk tech, an exec). | "I'm from IT, I need your password." |
| **Business email compromise (BEC)** | Compromising/spoofing a legitimate (often executive) email account to authorize fraudulent payments or actions. | "CEO" emails finance to wire funds urgently. |
| **Pretexting** | Inventing a fabricated scenario/backstory to justify the request and build trust. | The *story* that sets up the ask. |
| **Watering hole** | Compromising a website the target group is known to visit, so victims infect themselves. | Attacker poisons a trusted third-party site. |
| **Brand impersonation** | Masquerading as a trusted brand (logos, look-alike emails/sites). | Fake "Microsoft" or "your bank" page. |
| **Typosquatting** | Registering misspelled domains (g00gle.com, micros0ft.com) to catch fat-finger traffic. | Look-alike URL by typo. |

🎯 **Exam trap** — **Phishing vs. vishing vs. smishing — match the channel.** Email → phishing. Voice/phone → vishing. SMS/text → smishing. CompTIA picks the answer purely by *delivery medium*; read for "called," "texted," or "emailed."

🎯 **Exam trap** — **Misinformation vs. disinformation.** Both are false. *Dis*information is **deliberately** false (intent to deceive); *mis*information may be spread unknowingly. Intent is the discriminator.

🎯 **Exam trap** — **Pretexting vs. impersonation.** Impersonation = pretending to *be a specific someone*. Pretexting = the *fabricated scenario/story* that gives a reason for the request. They often co-occur ("I'm Dave from the audit team [impersonation] and we have an urgent compliance review [pretext]"), but if the question stresses the *invented backstory*, it's pretexting.

🎯 **Exam trap** — **BEC vs. generic phishing.** BEC specifically targets/abuses a *legitimate business email account* (often executive) to authorize fraud — frequently no malware, no link, just a convincing request. "Spoofed/compromised CEO email asking finance to wire money" → BEC, not plain phishing.

🎯 **Exam trap** — **Watering hole vs. brand impersonation.** Watering hole compromises a *site the targets already trust and visit*. Brand impersonation *mimics a trusted brand* on a fake site/email. One poisons a real site; the other fakes a real brand.

🧠 **Mnemonic** — Phishing channels: **"Every Voice Should"** → **E**mail = phishing, **V**oice = vishing, **S**MS = smishing.

⚡ **Keyword association** — "phone call asking for credentials" → vishing. "text message link" → smishing. "fake login page mimicking your bank's brand" → brand impersonation. "misspelled domain" → typosquatting. "compromised industry website the targets all visit" → watering hole. "CEO email, urgent wire transfer" → BEC. "made-up backstory to justify the ask" → pretexting.

📌 **Scenario pattern** — "Several employees at an engineering firm are infected after visiting an industry forum they all read daily; the forum was compromised to serve malware." → **watering hole attack**.

---

## 2.3 Vulnerabilities

A **vulnerability** is a weakness that a threat actor can exploit. Where 2.2 is the *path in*, 2.3 is the *flaw that lets them through*. Group these by where they live.

### Application vulnerabilities

| Vulnerability | Definition | Exam note |
|---|---|---|
| **Memory injection** | Inserting/altering code or data in a process's memory space to hijack execution. | Manipulating a running process's memory. |
| **Buffer overflow** | Writing more data than a buffer can hold, spilling into adjacent memory and potentially overwriting return addresses to run attacker code. | Classic; tied to unchecked input. |
| **Race condition / TOCTOU** | A timing flaw where the state checked changes before it's used. **TOCTOU = Time-Of-Check to Time-Of-Use** — the gap between checking a condition and acting on it is exploited. | "Check, then use" gap. |
| **Malicious update** | A software update that is itself trojanized/poisoned (often via a compromised update channel). | Update mechanism abused as delivery. |

🎯 **Exam trap** — **TOCTOU is the race-condition you must name.** If a scenario describes a value being validated and then changing before it's acted on (a file swapped between permission check and open), the answer is **TOCTOU / race condition**.

🎯 **Exam trap** — **Buffer overflow vs. memory injection.** Buffer overflow is *overrunning a buffer's bounds*; memory injection is *placing/altering code in process memory*. A buffer overflow is often the *means* of achieving memory injection, but if the question stresses "wrote past the buffer," pick buffer overflow.

### Operating-system-based vulnerabilities

Weaknesses in the OS kernel, drivers, or bundled services — privilege-escalation flaws, unpatched kernel CVEs, vulnerable system services. They're high-impact because OS-level compromise often means full control of the host. Mitigated by patching, hardening, and least privilege.

### Web-based vulnerabilities

| Vulnerability | Definition | Exam note |
|---|---|---|
| **SQL injection (SQLi)** | Injecting malicious SQL into an input so the database executes attacker commands. | Targets the **database/back end**. |
| **Cross-site scripting (XSS)** | Injecting malicious script that runs in *other users'* browsers in the context of a trusted site. | Targets **other users / the client**. |

🎯 **Exam trap** — **SQLi vs. XSS — who's the victim?** SQLi attacks the **application's database** (data theft, auth bypass). XSS attacks **other users' browsers** (session hijacking, defacement). "manipulates the database query" → SQLi; "script runs in another user's browser" → XSS. Both stem from unsanitized input — the fix for both is **input validation/output encoding**.

### Hardware vulnerabilities

| Vulnerability | Definition |
|---|---|
| **Firmware** | Flaws in the low-level code on hardware (BIOS/UEFI, NIC, router firmware). Hard to detect, persistent below the OS. |
| **End-of-life (EOL)** | Hardware no longer sold/supported by the vendor. |
| **Legacy** | Old hardware/systems still in use, often unpatchable and incompatible with modern controls. |

🎯 **Exam trap** — **End-of-life vs. legacy.** *EOL* = the vendor has stopped supporting it (no more patches/updates). *Legacy* = it's old and still in production (may or may not be EOL). They overlap, but EOL specifically means *support has ended*. Both push you toward **compensating controls** (isolation, segmentation) from [[Domain 1 — General Security Concepts]].

### Virtualization vulnerabilities

| Vulnerability | Definition |
|---|---|
| **VM escape** | Breaking out of a guest VM to reach the **hypervisor or host**, then potentially other VMs. The most serious virtualization threat. |
| **Resource reuse** | Sensitive data lingering in memory/storage that gets reassigned to another VM/tenant without being cleared — one tenant reads another's leftover data. |

🎯 **Exam trap** — **VM escape vs. resource reuse.** VM escape = *breaking out* of the guest to the host/hypervisor. Resource reuse = *leftover data* exposed when memory/disk is reassigned. "guest reaches the hypervisor" → escape; "previous tenant's data in reused memory" → resource reuse.

### Cloud-specific vulnerabilities

Weaknesses unique to cloud models — misconfigured storage buckets, overly permissive IAM roles, exposed management APIs, insecure shared-tenancy boundaries, and confusion over the **shared responsibility model** (treating the provider as responsible for something that's the customer's job). Most real cloud breaches trace to **customer-side misconfiguration**, not provider failure. Detailed cloud architecture lives in [[Domain 3 — Security Architecture]].

### Supply-chain vulnerabilities

Weaknesses introduced through **service providers, hardware providers, or software providers** you depend on. A compromised vendor library, a backdoored hardware component, or an MSP with weak security all become *your* vulnerability. The defense is vendor risk management, SBOMs, and least-privilege integration.

🎯 **Exam trap** — Distinguish the three supply-chain provider types: **service provider** (an MSP/SaaS you rely on), **hardware provider** (chips/devices that could be tampered in manufacture or transit), **software provider** (libraries, dependencies, update channels). The exam may ask which provider type a given compromise came through.

### Cryptographic vulnerabilities

Weaknesses in how crypto is chosen or implemented — deprecated algorithms (MD5, SHA-1, DES, RC4), weak keys, poor randomness, improper certificate validation, or supporting downgrade to weak protocols. These enable the cryptographic *attacks* in 2.4 (downgrade, collision, birthday). Cross-reference the crypto toolkit in [[Domain 1 — General Security Concepts]].

### Misconfiguration

The most common and most preventable vulnerability class: default settings left in place, unnecessary services enabled, open permissions, missing hardening, exposed admin interfaces. The fix is **configuration enforcement** and **hardening** (see 2.5).

⚡ **Keyword association** — "default settings / open S3 bucket / overly permissive role / unhardened" → misconfiguration.

### Mobile vulnerabilities

| Vulnerability | Definition |
|---|---|
| **Side loading** | Installing apps from outside the official app store, bypassing store vetting. |
| **Jailbreaking / rooting** | Removing manufacturer/OS restrictions to gain privileged control, disabling built-in security. |

🎯 **Exam trap** — **Side loading vs. jailbreaking.** Side loading = *installing apps from unofficial sources* (the OS restrictions are still intact, you're just going around the store). Jailbreaking/rooting = *removing the OS restrictions themselves* to get root/admin. Jailbreaking often enables side loading, but they're distinct: one bypasses the *store*, the other breaks the *OS sandbox*.

### Zero-day

A vulnerability **unknown to the vendor and without a patch**, exploited before defenders are even aware it exists. There's "zero days" of warning. The most dangerous class because no signature or fix exists yet; defended only by behavioral detection, defense-in-depth, and rapid response.

🎯 **Exam trap** — **Zero-day = no patch available yet** because the flaw is unknown to the vendor. If a scenario says "a patch existed but wasn't applied," that's an *unpatched/known* vulnerability, **not** a zero-day. Zero-day specifically means no fix exists.

⚡ **Keyword association** — "no patch exists / unknown to vendor / exploited before disclosure" → zero-day. "break out of the VM to the host" → VM escape. "install from outside the app store" → side loading. "check-then-use timing gap" → TOCTOU.

🔬 **Hands-on** — Run a free vulnerability scan against a deliberately weak target in your VirtualBox lab: spin up an old/EOL OS image and scan it with OpenVAS/Greenbone or Nessus Essentials. Watching the scanner flag missing patches, default creds, weak ciphers, and EOL software maps every 2.3 category to a real finding — and the report format is exactly what [[Domain 4 — Security Operations]] expects you to interpret.

---

## 2.4 Indicators of malicious activity

This is the heaviest single objective in the domain: name the malware, name the attack, and read the indicators. Build the recognition reflexes.

### Malware types

| Malware | Definition | The distinguishing trait |
|---|---|---|
| **Ransomware** | Encrypts (or threatens to leak) data and demands payment for restoration. | Extortion via encryption. |
| **Trojan** | Malware disguised as legitimate software; the user installs it willingly. | Disguise — no self-spread. |
| **Worm** | Self-replicating malware that **spreads across networks on its own**, no user action needed. | Self-propagation. |
| **Spyware** | Covertly gathers information about the user/activity. | Surveillance/data collection. |
| **Bloatware** | Unwanted pre-installed software; not inherently malicious but wastes resources and widens attack surface. | Unwanted, not (usually) malicious. |
| **Virus** | Malicious code that attaches to a file/program and **requires user action/execution to spread**. | Needs a host + user execution. |
| **Keylogger** | Records keystrokes to capture credentials and sensitive input. | Captures typing. |
| **Logic bomb** | Dormant malicious code that triggers on a **condition** (a date, an event, a user's deletion from payroll). | Conditional trigger. |
| **Rootkit** | Malware that hides itself and grants persistent privileged access, often subverting the OS/kernel. | Stealth + persistence at deep privilege. |

🎯 **Exam trap** — **Worm vs. virus.** A **worm spreads by itself** across networks (no user needed). A **virus needs a host file and user execution** to propagate. "Spread automatically with no user interaction" → worm; "attached to a file and ran when the user opened it" → virus. This is one of the most tested distinctions in the domain.

🎯 **Exam trap** — **Trojan vs. virus.** A trojan is *disguised* as legitimate software and doesn't self-replicate; a virus *attaches to and infects* other files. "Looked like a real program the user installed" → trojan.

🎯 **Exam trap** — **Logic bomb is condition-triggered.** "Malicious code that activated on a specific date / after the admin was terminated" → logic bomb. Classic insider weapon.

🎯 **Exam trap** — **Rootkit = stealth + deep persistence.** If the scenario stresses that malware *hides from detection* and survives at kernel/admin level, it's a rootkit (often requiring offline/boot-time detection).

🧠 **Mnemonic** — "Worm Wanders, Virus Visits-with-a-host." Worm **W**anders the network on its own; Virus needs a host file to **V**isit (attach to) and a user to run it.

⚡ **Keyword association** — "encrypts files, demands payment" → ransomware. "disguised as legit software" → trojan. "spreads on its own across the network" → worm. "records keystrokes" → keylogger. "triggers on a date/condition" → logic bomb. "hides itself, kernel-level, persistent admin" → rootkit. "pre-installed junk" → bloatware.

### Physical attacks

| Attack | Definition |
|---|---|
| **Brute force (physical)** | Forcing physical entry/access — smashing, prying, defeating a lock by force. |
| **RFID cloning** | Copying an RFID/proximity badge to duplicate access credentials. |
| **Environmental** | Attacking via the physical environment — power, cooling, fire suppression, HVAC — to damage or disrupt. |

🎯 **Exam trap** — **"Brute force" is overloaded.** In *physical* attacks it means forcing entry. In *password* attacks (below) it means trying many password combinations. Read whether the context is a door/lock or a login.

### Network attacks

| Attack | Definition | Exam note |
|---|---|---|
| **DDoS — amplified** | Attacker sends small queries to services (DNS, NTP, memcached) that return **much larger responses**, multiplying volume against the victim. | Small request → big response (amplification factor). |
| **DDoS — reflected** | Attacker spoofs the victim's source IP so responses from third parties are **reflected to the victim**. | Spoofed source → responses bounce to victim. |
| **DNS attacks** | Poisoning, spoofing, or hijacking DNS to redirect victims or disrupt resolution. | Manipulating name resolution. |
| **Wireless** | Attacks on Wi-Fi/Bluetooth — rogue APs, evil twins, deauth, jamming. | Targets the radio layer. |
| **On-path (MITM)** | Attacker positions between two parties to intercept/alter traffic. (Formerly "man-in-the-middle.") | Intercept/modify in transit. |
| **Credential replay** | Capturing valid credentials/session tokens and **reusing** them to authenticate. | Replay of captured auth. |
| **Malicious code** | Injecting/executing hostile code over the network as part of the attack. | Code execution as the payload. |

🎯 **Exam trap** — **Amplified vs. reflected DDoS.** *Reflection* = spoof the victim's IP so third parties send responses **to** the victim. *Amplification* = the responses are **much larger** than the requests. They're usually combined (reflected + amplified), but if asked to pick: "responses bounce off third parties to the victim" → reflected; "small query yields a huge response" → amplified.

🎯 **Exam trap** — **On-path is the SY0-701 term for man-in-the-middle.** If you see "man-in-the-middle," it's on-path; the exam prefers "on-path attack."

🎯 **Exam trap** — **Credential replay vs. password attack.** Replay reuses *already-captured valid* credentials/tokens — no guessing. A password attack (spraying/brute force) *guesses*. "Captured and reused a valid session" → replay; "tried many guesses" → password attack. Defended by nonces, timestamps, and one-time tokens.

### Application attacks

| Attack | Definition |
|---|---|
| **Injection** | Inserting malicious input that the app interprets as a command (SQLi, command injection, LDAP injection). |
| **Buffer overflow** | Overrunning a buffer to corrupt memory/execute code (see 2.3). |
| **Replay** | Reusing captured legitimate requests/tokens to repeat an action. |
| **Privilege escalation** | Gaining higher rights than granted — *vertical* (user → admin) or *horizontal* (one user → another's access). |
| **Forgery (CSRF/SSRF)** | **CSRF** tricks an authenticated user's browser into sending an unwanted request; **SSRF** tricks the *server* into making requests to attacker-chosen destinations. |
| **Directory traversal** | Using path sequences (`../`) to access files outside the web root the app shouldn't expose. |

🎯 **Exam trap** — **Vertical vs. horizontal privilege escalation.** Vertical = gaining *higher* privilege (standard user → admin/root). Horizontal = accessing *another user's same-level* resources. "Standard account got admin rights" → vertical; "User A read User B's account data" → horizontal.

🎯 **Exam trap** — **CSRF vs. SSRF.** CSRF abuses the *client/user's* authenticated session (the browser is the unwitting actor). SSRF abuses the *server* into making requests on the attacker's behalf (reaching internal systems). "victim's browser sends the request" → CSRF; "tricked the server into fetching an internal URL" → SSRF.

⚡ **Keyword association** — "`../../etc/passwd` / escape the web root" → directory traversal. "user became admin" → vertical privilege escalation. "tricked the server into requesting an internal address" → SSRF.

### Cryptographic attacks

| Attack | Definition |
|---|---|
| **Downgrade** | Forcing a connection to use a weaker/older protocol or cipher (e.g., TLS → SSL) to exploit it. |
| **Collision** | Finding two different inputs that produce the **same hash** — breaks integrity (MD5/SHA-1 are vulnerable). |
| **Birthday** | A statistical attack leveraging the birthday paradox to find hash collisions faster than brute force. |

🎯 **Exam trap** — **Collision vs. birthday.** A *collision* is the **goal** (two inputs, same hash). The *birthday attack* is a **method/math** that makes finding a collision probabilistically easier. "two different files, identical hash" → collision; "exploits the birthday paradox to find that collision" → birthday attack.

🎯 **Exam trap** — **Downgrade** → think "forced to use the weaker option." A scenario where an attacker strips TLS down to an exploitable older version is a downgrade attack (a common partner to on-path positioning).

### Password attacks

| Attack | Definition |
|---|---|
| **Spraying** | Trying **one (or a few) common passwords against many accounts** to avoid lockout. | 
| **Brute force** | Trying **many/all password combinations against one account** until it cracks. |

🎯 **Exam trap** — **Spraying vs. brute force — invert the targets.** *Spraying* = **one password, many accounts** (low-and-slow, evades lockout). *Brute force* = **many passwords, one account**. The tell: spraying spreads attempts wide to dodge account-lockout thresholds; brute force hammers a single login. This is a guaranteed exam question.

🧠 **Mnemonic** — "**Spray** the crowd, **Brute** the one." Spraying sprays one guess across the *crowd* of accounts; brute force focuses brute strength on *one* account.

### Indicators

These are the *symptoms* in logs/telemetry that reveal an attack happened. Match indicator → likely attack.

| Indicator | What it signals |
|---|---|
| **Account lockout** | Repeated failed logins — often a **brute-force** attempt (or a locked-out spraying victim). |
| **Concurrent session usage** | The same account active in two places at once — possible **credential theft/replay**. |
| **Blocked content** | Security controls blocking access to something — DLP/web filter catching exfiltration or malicious sites. |
| **Impossible travel** | Logins from geographically distant locations too close in time to be one human — **compromised credentials**. |
| **Resource consumption** | Unexplained spikes in CPU/bandwidth/storage — crypto-mining, exfiltration, or DoS. |
| **Resource inaccessibility** | Systems/files suddenly unreachable — **ransomware** encryption or a DoS. |
| **Out-of-cycle logging** | Log entries/activity at unusual times (3 a.m. admin actions) — attacker operating off-hours. |
| **Published / documented** | The vulnerability or breach indicator is publicly known/disclosed (CVE published, data appearing on a leak site). |
| **Missing logs** | Gaps or deleted log entries — an attacker **covering tracks** (anti-forensics). |

🎯 **Exam trap** — **Impossible travel = compromised credentials.** Logins from New York and Singapore eight minutes apart can't be one person traveling → the account is compromised (or credentials shared). This is the single most recognizable indicator scenario.

🎯 **Exam trap** — **Missing logs vs. out-of-cycle logging.** *Missing logs* = entries are **gone/deleted** (track-covering). *Out-of-cycle* = entries **exist but at abnormal times**. "log gaps / deleted entries" → missing logs; "activity at 3 a.m." → out-of-cycle.

🎯 **Exam trap** — **Resource consumption vs. resource inaccessibility.** Consumption = abnormal **usage** (CPU/bandwidth spike → mining/exfil/DoS). Inaccessibility = you **can't reach** the resource (→ ransomware/DoS). One is "it's working too hard," the other is "it's unreachable."

⚡ **Keyword association** — "logins from two far-apart places minutes apart" → impossible travel. "same account in two sessions at once" → concurrent session usage. "logs deleted / gaps" → missing logs (track covering). "files suddenly encrypted/unreachable" → resource inaccessibility (ransomware). "CPU pegged with no explanation" → resource consumption.

📌 **Scenario pattern** — "A user account logs in from Ohio, then from Eastern Europe four minutes later; minutes after, finance reports unreachable shared files and the server's CPU spikes." → **impossible travel (credential compromise)** leading to **resource inaccessibility + consumption (ransomware)**.

🔬 **Hands-on** — These indicators are literally the alerts you tune in production. In Entra ID, the "atypical travel" and "anonymous IP" risk detections *are* impossible-travel indicators; in Microsoft Defender/Sentinel you can build a KQL query for concurrent sign-ins or after-hours admin activity (out-of-cycle logging). Write one Sentinel analytics rule for impossible travel and you've operationalized a 2.4 indicator — and prepped for [[Domain 4 — Security Operations]].

---

## 2.5 Mitigation techniques

Mitigations are the defensive countermeasures that reduce the likelihood or impact of the threats above. Several reappear as controls in [[Domain 1 — General Security Concepts]] and architecture in [[Domain 3 — Security Architecture]] — here the exam tests *which mitigation answers which problem*.

| Technique | What it does | When it's the answer |
|---|---|---|
| **Segmentation** | Dividing the network into isolated zones (VLANs, subnets) so a compromise in one can't freely reach others. | Limit **lateral movement** / contain blast radius. |
| **Access control (ACL, permissions)** | Restricting who/what can reach a resource via access control lists and assigned permissions. | Enforce who can touch what. |
| **Application allow list** | Permitting **only explicitly approved** applications to run; everything else is denied by default. | Stop unauthorized/unknown software (default-deny). |
| **Isolation** | Cutting a system/process off entirely (sandbox, air-gap, quarantine) so it can't interact with others. | Contain an infected/untrusted host. |
| **Patching** | Applying vendor fixes to remove known vulnerabilities. | Close **known** CVEs; the #1 routine mitigation. |
| **Encryption** | Rendering data unreadable without the key. | Protect confidentiality at rest/in transit. |
| **Monitoring** | Continuously collecting and reviewing telemetry/logs to detect activity. | Detect the 2.4 indicators. |
| **Least privilege** | Granting the minimum access needed to do the job, nothing more. | Limit what a compromised account/insider can do. |
| **Configuration enforcement** | Ensuring systems stay in their approved, hardened, compliant state (and drift is corrected). | Prevent/fix **misconfiguration**. |
| **Decommissioning** | Securely retiring and removing systems/data no longer needed (wipe, deprovision, remove from inventory). | Eliminate EOL/orphaned attack surface. |
| **Hardening** | Reducing a system's attack surface by removing/disabling unnecessary functionality and tightening settings (detailed below). | Make each host minimal and resilient. |

🎯 **Exam trap** — **Application allow list vs. deny list.** An **allow list is default-deny** (only approved apps run — stronger, used against unknown malware). A deny list is default-allow (only listed bad items blocked — weaker). If a scenario wants to stop *any unapproved/unknown* software from running, it's an **allow list**. (Same logic as Domain 1's allow/deny posture.)

🎯 **Exam trap** — **Segmentation vs. isolation.** Segmentation *divides* the network into controlled zones that can still talk under rules. Isolation *fully cuts off* a system (quarantine/air-gap). "Contain lateral movement across zones" → segmentation; "completely sever an infected host" → isolation.

🎯 **Exam trap** — **Least privilege vs. access control.** Access control is the *mechanism* (ACLs/permissions decide who can access what). Least privilege is the *principle* (grant the **minimum** necessary). The exam may ask for the principle that limits an insider's reach → least privilege.

### Hardening

Hardening is the deliberate reduction of attack surface on a host. The listed measures:

| Hardening measure | What it does |
|---|---|
| **Encryption** | Protect data at rest (e.g., full-disk encryption) so a stolen device yields nothing. |
| **Endpoint protection** | EDR/antivirus on the host to detect and block malware. |
| **Host-based firewall** | A firewall *on the host itself* controlling inbound/outbound traffic per machine. |
| **HIPS (Host Intrusion Prevention System)** | Host agent that detects **and blocks** malicious activity on that endpoint. |
| **Disabling ports/protocols** | Turning off unused network ports and insecure protocols (Telnet, SMBv1) to shrink exposure. |
| **Default password changes** | Replacing factory credentials immediately on deployment. |
| **Removal of unnecessary software** | Uninstalling unneeded apps/services (and bloatware) to eliminate their vulnerabilities. |

🎯 **Exam trap** — **Host-based firewall vs. HIPS.** A host-based firewall filters **traffic by rules** (ports/IPs/protocols). HIPS inspects **behavior/signatures and blocks malicious activity** (a host IPS). One is a traffic gate; the other is an active threat-blocker. Both live on the endpoint.

🎯 **Exam trap** — **Hardening vs. configuration enforcement.** Hardening is the *act* of locking a system down (disable services, change defaults). Configuration enforcement is *keeping it that way* (detecting and correcting drift from the hardened baseline). "Initially secure the box" → hardening; "ensure it stays compliant over time" → configuration enforcement.

⚡ **Keyword association** — "only approved apps can run" → application allow list. "minimum access necessary" → least privilege. "contain lateral movement / VLANs" → segmentation. "completely cut off the infected host" → isolation. "turn off Telnet/SMBv1/unused ports" → disabling ports/protocols. "securely retire the old server" → decommissioning. "keep it in its approved state / fix drift" → configuration enforcement.

🔬 **Hands-on** — Build a hardening baseline in your lab and *enforce* it. In Intune, deploy a security baseline + an attack-surface-reduction policy and watch noncompliant devices flag — that's configuration enforcement and hardening in one workflow. On a Windows Server VM, apply a CIS Benchmark with a PowerShell DSC or Group Policy, then disable SMBv1 (`Disable-WindowsOptionalFeature -Online -FeatureName SMB1Protocol`) and turn on the host firewall. You've now touched hardening, configuration enforcement, and disabling protocols against a real machine.

---

## 🎯 Top exam traps

1. **High sophistication + high resources + long stealth → nation-state**, even when "government" isn't stated. Infer the actor type from attributes.
2. **Shadow IT is a listed actor but not an attacker** — it's employees' unsanctioned tools creating unmanaged risk.
3. **Blackmail (coerce by threat) vs. financial gain (broad profit)**; **ethical is a real motivation** (authorized testing). Read for intent and authorization.
4. **Phishing/vishing/smishing split purely by channel** — email / voice / SMS. **Misinformation vs. disinformation** = intent (disinformation is deliberate). **BEC** = abused legitimate (often exec) email for fraud.
5. **Pretexting (the fabricated story) vs. impersonation (pretending to be someone)**; **watering hole (poison a trusted site) vs. brand impersonation (fake a trusted brand)**.
6. **Client-based vs. agentless vulnerable software** — agent installed on the host vs. exploited remotely with nothing on the target.
7. **Worm spreads itself; virus needs a host file + user execution.** Trojan is disguised legit software. Logic bomb is condition-triggered. Rootkit = stealth + deep persistence.
8. **Spraying (one password, many accounts) vs. brute force (many passwords, one account)** — invert the targets. **Brute force is also a physical attack** (forcing entry) — read context.
9. **Reflected DDoS (spoofed source bounces responses to victim) vs. amplified (small request → huge response).** **On-path = man-in-the-middle.** **Credential replay reuses captured valid creds — no guessing.**
10. **SQLi attacks the database; XSS attacks other users' browsers.** **Vertical (→ admin) vs. horizontal (→ peer) privilege escalation.** **CSRF abuses the user's browser; SSRF abuses the server.**
11. **VM escape (break out to host) vs. resource reuse (leftover tenant data).** **Side loading (unofficial app source) vs. jailbreaking (remove OS restrictions).**
12. **Zero-day = no patch exists / unknown to vendor.** If a patch existed but wasn't applied, it's an unpatched known vuln, not a zero-day.
13. **Collision (the goal: two inputs, same hash) vs. birthday attack (the method to find it).** **Downgrade = forced to a weaker protocol.**
14. **Impossible travel = compromised credentials.** **Missing logs (deleted/gaps, track-covering) vs. out-of-cycle logging (exists but abnormal time).** **Resource consumption (overworked) vs. inaccessibility (unreachable → ransomware/DoS).**
15. **Application allow list = default-deny (stronger).** **Segmentation (zoned, still communicates) vs. isolation (fully severed).** **Host-based firewall (traffic rules) vs. HIPS (blocks malicious behavior).** **Hardening (lock it down) vs. configuration enforcement (keep it locked down).**

## 🧠 Mnemonic pack

- **Threat actor attributes — "In Real Style"**
  - **In** → **In**ternal/external
  - **Real** → **R**esources/funding
  - **Style** → **S**ophistication/capability
  - ✅ Maps to Internal/external, Resources/funding, Sophistication/capability — the three attributes, correct.

- **Phishing channels — "Every Voice Should"**
  - **Every** → **E**mail = phishing
  - **Voice** → **V**oice = vishing
  - **Should** → **S**MS = smishing
  - ✅ Maps email→phishing, voice→vishing, SMS→smishing — the three channels, correct.

- **Worm vs. virus — "Worm Wanders, Virus Visits-with-a-host"**
  - **Worm Wanders** → a worm **W**anders the network on its own (self-propagating, no user)
  - **Virus Visits** → a virus needs a host file to **V**isit (attach to) and a user to execute it
  - ✅ Correctly separates self-spreading worm from host-dependent, user-executed virus.

- **Password attacks — "Spray the crowd, Brute the one"**
  - **Spray the crowd** → spraying = one password across **many** accounts (the crowd), evading lockout
  - **Brute the one** → brute force = **many** passwords against **one** account
  - ✅ Correctly inverts the targets for the two password attacks.

## ✅ Self-check

1. An attacker maintains undetected access to a power utility's control network for over a year, exfiltrating engineering documents with no ransom demand. What actor type and primary motivation best fit?
2. A marketing team signs up for an unapproved cloud file-sharing service without telling IT. Which Domain 2 category does this represent, and is it an "attack"?
3. An employee receives a phone call from someone claiming to be the help desk, asking them to read back an MFA code. Email, voice, or SMS — and what is the technique called?
4. Finance receives an urgent email from the CFO's actual (compromised) mailbox instructing a wire transfer to a new vendor. What attack is this, specifically?
5. A vulnerability is exploited in a service over the network with nothing installed on the target host. Is this client-based or agentless vulnerable software?
6. Malware spreads automatically from machine to machine across the LAN with no user interaction. Worm or virus? What if it had instead attached to a document and run only when opened?
7. A standard user exploits a flaw to obtain root privileges. Vertical or horizontal privilege escalation?
8. An attacker tries the single password "Spring2026!" against 4,000 user accounts. What password attack is this, and why does it evade account lockout?
9. Two different files are found to produce an identical SHA-1 hash. What is this result called, and what statistical attack method is used to find it efficiently?
10. A user account signs in from Columbus and then from Manila six minutes later. What indicator is this, and what does it imply?
11. An analyst finds that security event logs have gaps with entries clearly deleted around the time of suspected intrusion. What indicator is this?
12. A guest virtual machine is used to gain access to the underlying hypervisor and reach other VMs. What vulnerability is this?
13. A vulnerability is being actively exploited before the vendor is even aware of it and no fix exists. What is it called, and why is "they failed to patch" the wrong description?
14. The team wants to ensure that only an explicitly approved set of applications can execute on company endpoints, blocking everything else by default. Which mitigation is this?
15. After hardening a fleet of servers, the team wants to guarantee they don't drift out of the secure baseline over time. Which mitigation technique addresses that, as distinct from the initial hardening?

<details>
<summary>Answers</summary>

1. **Nation-state**, motivation **espionage** (long covert dwell time on critical infrastructure, intelligence-gathering, no financial demand).
2. **Shadow IT.** It is **not an attack** — it's employees deploying unsanctioned services, creating unmanaged risk rather than acting maliciously.
3. **Voice** → **vishing** (voice phishing).
4. **Business email compromise (BEC)** — abuse of a legitimate/compromised executive mailbox to authorize a fraudulent payment; distinct from generic phishing.
5. **Agentless** — exploited remotely with nothing installed on the target (client-based would require the vulnerable software/agent on the host).
6. **Worm** (self-propagating, no user action). If it attached to a document and ran only when opened, it would be a **virus** (host-dependent, user-executed).
7. **Vertical** privilege escalation (gaining *higher* privilege — user to root).
8. **Password spraying** — one common password tried against many accounts. It evades lockout because each individual account only sees one failed attempt, staying under the lockout threshold.
9. A **collision** (two inputs, same hash). The efficient method is the **birthday attack** (exploiting the birthday paradox).
10. **Impossible travel** — the two locations can't be reached by one person in that time, implying **compromised credentials**.
11. **Missing logs** — deleted entries/gaps indicate an attacker **covering their tracks** (anti-forensics).
12. **VM escape** — breaking out of the guest to the hypervisor/host (the most serious virtualization vulnerability).
13. A **zero-day** — unknown to the vendor with no patch available. "They failed to patch" is wrong because **no patch exists**; that phrasing would describe an unpatched *known* vulnerability instead.
14. **Application allow list** (default-deny: only approved apps run).
15. **Configuration enforcement** — detecting and correcting drift to keep systems in their approved, hardened state (hardening is the initial lockdown; enforcement maintains it).

</details>

⬅ [[Security+ SY0-701 — Course Index]] | ➡ Next: [[Domain 3 — Security Architecture]]
