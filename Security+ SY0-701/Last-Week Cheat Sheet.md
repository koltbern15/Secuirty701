# Security+ SY0-701 — Last-Week Cheat Sheet

Rapid review. Highest-yield only. Read top to bottom the morning of.

> 💡 **Callout legend:** 💡 distinction worth memorizing · ⚠️ classic exam trap · 🎯 if the stem says X, answer Y

---

## Crypto direction rules

> 💡 You always operate on the *other party's* keys for confidentiality, and your *own* private key for proof-of-origin.

| Goal | Use this key | Reverse with |
|---|---|---|
| **Encrypt** (confidentiality) | recipient's **PUBLIC** | recipient's **PRIVATE** |
| **Sign** (integrity + authenticity + non-repudiation) | sender's **PRIVATE** | sender's **PUBLIC** |

- **Confidentiality** → recipient keys. **Authenticity/non-repudiation** → sender keys. Don't cross them.
- Real-world TLS/PGP: asymmetric only wraps a per-session **symmetric** key, then symmetric does the bulk. That's *hybrid* crypto.
- A **digital signature** = hash of the message encrypted with sender's private key. Gives integrity + authenticity + non-repudiation. Encryption alone gives confidentiality only — no integrity, no non-repudiation.

| | Symmetric | Asymmetric |
|---|---|---|
| Speed | fast | slow |
| Use | bulk data encryption | key exchange + signatures |
| Keys | one **shared secret** | public/private **pair** |
| Problem it has | secure key distribution | computationally expensive |
| Examples | AES, ChaCha20, 3DES | RSA, ECC, ECDSA, DH/ECDHE, ElGamal |

⚠️ **Diffie-Hellman / ECDHE = key *exchange/agreement*, not encryption and not a cipher.** ⚠️ **DSA/ECDSA = signatures only, can't encrypt.** ⚠️ Hashing (SHA-256, HMAC) ≠ encryption — no key recovers the plaintext; integrity only.

---

## Control CATEGORY × TYPE matrix

**Category = how it's implemented.** **Type = what it does.** Every control has exactly one of each — and the stem usually tests one axis at a time.

| Category ↓ \ Type → | Preventive | Deterrent | Detective | Corrective | Compensating | Directive |
|---|---|---|---|---|---|---|
| **Technical** (tech/logic) | firewall rule, ACL, MFA | login warning banner | IDS, SIEM alert | quarantine/auto-isolate host | hot site, app-layer encryption | DLP block-and-warn policy enforcement |
| **Managerial** (admin/policy) | hiring/onboarding policy | published sanctions policy | scheduled access review, audit | revised risk assessment after incident | exception approval w/ extra review | AUP, security policy, SDLC standard |
| **Operational** (people/process) | security-awareness training | "guard on duty" sign | log review, guard patrol | incident response execution | manual review when tool unavailable | runbook / posted procedures |
| **Physical** | bollard, locked door, mantrap | fence, lighting, "Beware of Dog" | CCTV, motion sensor, door alarm | fire suppression, backup generator | security guard covering broken lock | signage directing to exits |

> 💡 **Type discriminators:**
> - **Preventive** stops it *before* it happens (firewall, MFA, lock).
> - **Deterrent** discourages the attempt (banner, fence, sign) — psychological, doesn't physically stop.
> - **Detective** identifies *during/after* (IDS, CCTV, log).
> - **Corrective** fixes/restores *after* (backup restore, patch, IR).
> - **Compensating** is the *alternative* when the primary control isn't feasible.
> - **Directive** *instructs* people what to do (policy, AUP, procedure).

⚠️ CCTV is **detective**, not preventive (a sign saying "CCTV in use" is deterrent). ⚠️ A **backup** is corrective (recovery), the **encryption of the backup** is preventive. ⚠️ Compensating ≠ a weaker control — it's a *substitute* meeting the same intent.

---

## Incident response — phases (NIST order)

`Preparation → Detection → Analysis → Containment → Eradication → Recovery → Lessons Learned`

- **Preparation**: tooling, runbooks, training, comms plan, *before* anything happens.
- **Detection**: something's wrong (alert, report).
- **Analysis**: confirm it's real, scope it, triage.
- **Containment**: stop the spread (segment, isolate). ⚠️ Containment comes **before** eradication — isolate first, *then* clean.
- **Eradication**: remove the cause (malware, accounts, vuln).
- **Recovery**: restore to production, monitor.
- **Lessons Learned**: after-action, update controls/playbooks (feeds back into Preparation).

⚠️ The instinct to "pull the plug" is containment — but powering off destroys volatile evidence. Capture memory first if forensics matter.

## Order of volatility (collect most-volatile FIRST)

`CPU registers / cache → RAM + routing table / ARP cache / process table / kernel stats → temp files / swap (pagefile) → disk (HDD/SSD) → remote logging & monitoring data → physical config, network topology, archival media (backups)`

> 💡 Mnemonic logic: the faster it disappears when power/state changes, the sooner you grab it. Registers vanish in nanoseconds; backups sit for years.

---

## Risk formulas

| Term | Formula | Meaning |
|---|---|---|
| **SLE** | `AV × EF` | Single Loss Expectancy — $ lost per **one** event |
| **ALE** | `SLE × ARO` | Annualized Loss Expectancy — $ lost **per year** |
| AV | — | Asset Value ($) |
| EF | — | Exposure Factor (% of asset lost per event) |
| ARO | — | Annualized Rate of Occurrence (events/year) |

**Worked example:** Laptop AV = $2,000. A theft destroys 100% of it → EF = 1.0. Two laptops stolen per year → ARO = 2.
- SLE = $2,000 × 1.0 = **$2,000**
- ALE = $2,000 × 2 = **$4,000/yr**

🎯 If a control costs **less** than the ALE it reduces, it's cost-justified. ⚠️ ARO can be fractional (once every 5 years → ARO = 0.2). ⚠️ SLE is per-incident; ALE is annual — don't swap them.

---

## Access-control models

| Model | One-line discriminator |
|---|---|
| **MAC** | System enforces access via **labels/clearances** (Top Secret, etc.); users **can't** change perms. Non-discretionary, rule-of-the-OS. SELinux. |
| **DAC** | **Owner** decides who gets access (NTFS perms). Flexible, but owner discretion = weakest control. |
| **RBAC** | Access by **job role/group**, not individual. Scales for orgs; "Accountant" role gets accounting rights. |
| **Rule-based** | Access by **system-wide if/then rules** (firewall ACL, time-of-day restriction) applied to everyone. |
| **ABAC** | Access by **attributes/policy** (user dept + device + location + time). Most granular/dynamic; "context-aware." |

⚠️ **Rule-based ≠ Role-based.** Rule = conditions on the resource; Role = the user's job. ⚠️ MAC = labels + mandatory, not "the admin manually sets it." ⚠️ ABAC is the answer when the stem stacks multiple conditions (device + geo + time).

---

## Social-engineering quick-ID

| Keyword in the stem | Attack |
|---|---|
| voice call / phone / vishing | **Vishing** |
| SMS / text message | **Smishing** |
| targeted at a **specific** person/group | **Spear phishing** |
| targeting **executives/CEO/CFO** | **Whaling** |
| compromised **trusted website** victims already visit | **Watering hole** |
| **typo'd domain** / look-alike URL (gooogle.com) | **Typosquatting** |
| follows authorized person through door, **no consent** | **Tailgating** |
| follows through door **with** holder's awareness/courtesy | **Piggybacking** |
| shoulder-watching screen/PIN | **Shoulder surfing** |
| dumpster / discarded docs | **Dumpster diving** |
| offering something (USB, free thing) | **Baiting** |
| pretext story / fake identity to build trust | **Pretexting / impersonation** |
| **urgency / authority / scarcity / fear** language | social-engineering **principle** (not an attack — name the principle) |
| fake invoice / wire-transfer request from "exec" | **BEC** (business email compromise) |
| influence the masses / fake reviews / bandwagon | **Misinformation / influence campaign** |

> 💡 Principles of influence: **Authority, Urgency, Scarcity, Social proof (consensus), Familiarity/Liking, Trust, Intimidation.** If the question asks *why it worked*, pick a principle, not an attack name.

---

## Ports & protocols

| Port | Proto | | Port | Proto |
|---|---|---|---|---|
| 20/21 | FTP (data/control) | | 443 | HTTPS |
| 22 | SSH / SCP / SFTP | | 445 | SMB |
| 23 | Telnet *(insecure)* | | 465 | SMTPS |
| 25 | SMTP | | 514 | Syslog |
| 53 | DNS | | 587 | SMTP submission (STARTTLS) |
| 67/68 | DHCP (server/client) | | 636 | LDAPS |
| 69 | TFTP | | 993 | IMAPS |
| 80 | HTTP | | 995 | POP3S |
| 88 | Kerberos | | 1433 | MSSQL |
| 110 | POP3 | | 1812/1813 | RADIUS (auth/acct) |
| 123 | NTP | | 3306 | MySQL |
| 143 | IMAP | | 3389 | RDP |
| 161/162 | SNMP (poll/trap) | | | |
| 389 | LDAP | | | |

> 💡 Secure-pair swaps to memorize: HTTP **80→443**, FTP→FTPS **990/989**, SFTP rides **SSH 22** (≠ FTPS), LDAP **389→636**, SMTP **25→465/587**, IMAP **143→993**, POP3 **110→995**, Telnet **23→SSH 22**, RDP **3389**, SNMPv3 secures the same **161/162**.

⚠️ **SFTP (22, over SSH) ≠ FTPS (FTP over TLS, 990).** ⚠️ **Kerberos = 88**, **RADIUS = 1812/1813**, **TACACS+ = 49 (TCP)** — RADIUS uses UDP and only encrypts the password; TACACS+ encrypts the whole payload and separates AAA. ⚠️ Syslog **514**, RDP **3389**, SMB **445** (EternalBlue/lateral movement target).

---

## Top exam traps (the ones that bite)

1. **Confidentiality vs authenticity** keys — encrypt with *recipient* public, sign with *sender* private. (See crypto table.)
2. **Hashing is not encryption.** Integrity only. **HMAC** = hash + secret key (integrity + authenticity). **Salt** stops rainbow tables / pre-computation; **pepper** is a secret salt stored separately. **Key stretching** (PBKDF2, bcrypt, scrypt, Argon2) slows brute force.
3. **Tabletop = discussion** exercise (talk through it). **Simulation** = actually execute. Don't pick the live one for "least disruptive."
4. **Detective vs preventive** — CCTV/IDS/logs detect; firewall/lock/MFA prevent; banner/fence deter.
5. **Compensating control** = substitute when primary isn't feasible — not "extra" and not "weaker."
6. **Risk responses:** accept / avoid / **transfer** (insurance, outsource) / **mitigate** (reduce). Buying cyber-insurance = transfer, not mitigate.
7. **Qualitative** (high/med/low, subjective) vs **quantitative** (SLE/ALE, $). Heat map = qualitative.
8. **IOC vs IOA** — indicator of *compromise* (it happened) vs *attack* (in progress).
9. **DLP** stops data exfiltration; **CASB** is the cloud policy broker; **SWG** filters web; **NAC** gates devices onto the network. Match the tool to the boundary.
10. **TOTP vs HOTP** — Time-based vs counter/HMAC (event)-based. Authenticator apps = TOTP.
11. **Something you know/have/are/do/are-somewhere.** Two of the *same* factor (password + PIN) is **not** MFA.
12. **SAML vs OAuth vs OIDC** — SAML = enterprise SSO (XML, web apps). OAuth = **authorization** (delegated access). OIDC = **authentication** layer on OAuth. If stem says "verify identity," it's OIDC/SAML; "grant access to my data," OAuth.
13. **Federation** = trust across orgs/domains; **SSO** = one login within a trust boundary. Not synonyms.
14. **SOAR** = automation/orchestration/playbooks; **SIEM** = aggregation/correlation/alerting. SOAR *acts*, SIEM *sees*.
15. **Full / incremental / differential backups** — incremental (since last *any* backup, fast backup/slow restore) vs differential (since last *full*, slower backup/faster restore). Restore differential = full + last diff.
16. **RTO** (how fast back online) vs **RPO** (how much data loss tolerable) vs **MTTR** (mean repair time) vs **MTBF** (mean time between failures). RPO sets backup frequency.
17. **Zero-day** = no patch exists yet. Patching can't be the answer; use compensating controls (segmentation, IPS signatures, monitoring).
18. **On-path (MITM)** intercepts/relays; **replay** re-sends captured valid data; **session hijacking** steals the token. **Nonce/timestamp** defeats replay.
19. **SQLi vs XSS vs CSRF** — SQLi hits the *database*; XSS runs script in the *victim's browser*; CSRF *rides the victim's session* to force an action. Defenses: parameterized queries / output encoding+CSP / anti-CSRF tokens.
20. **Hot / warm / cold site** — hot = ready now (costly), cold = empty space (cheap, slowest), warm = in between. Match to RTO and budget.
21. **Order of volatility** before order of restore — capture RAM before powering off, always.
22. **Tokenization vs encryption vs masking** — tokenization swaps data for a non-mathematical token (vault maps back); encryption is reversible with a key; masking hides characters (often irreversible display-only).

---

⬅ [[Security+ SY0-701 — Course Index]]
