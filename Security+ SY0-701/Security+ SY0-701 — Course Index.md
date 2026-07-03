# Security+ SY0-701 — Course Index

> The structured, weight-aware study system for CompTIA Security+ SY0-701 — five domain modules, drill tools, and exam-day tactics, sequenced so you spend hours where the exam spends points.

---

## Exam at a glance

| Attribute | Detail |
|---|---|
| **Exam code** | SY0-701 |
| **Questions** | Up to 90 (multiple-choice + performance-based/PBQ) |
| **Time** | 90 minutes |
| **Passing score** | 750 / 900 (scaled) |
| **Cost** | ~$400+ USD (single voucher; bundles/retakes extra) |
| **Validity** | 3 years (renewable via CE or higher cert) |
| **DoD mapping** | Meets DoD 8570/8140 **IAT Level II** and **IAM Level II** |

> 📌 **Version note:** SY0-701 is the **current and only active version** of Security+. CompTIA runs a roughly 3-year objectives cycle, and SY0-701 launched **November 2023** — so the next revision is plausibly on the horizon. Before you book, pull the **live objectives PDF from CompTIA.org** and confirm SY0-701 is still the testable version and that the domain weights below haven't shifted.

---

## Domains + weights

| Domain | Weight | ~Questions |
|---|---|---|
| [[Domain 1 — General Security Concepts]] | 12% | ~11 |
| [[Domain 2 — Threats, Vulnerabilities & Mitigations]] | 22% | ~20 |
| [[Domain 3 — Security Architecture]] | 18% | ~16 |
| [[Domain 4 — Security Operations]] | 28% | ~25 |
| [[Domain 5 — Security Program Management & Oversight]] | 20% | ~18 |

Per-question counts are approximate — the live exam draws from a pool and the unscored/pretest items shift the exact split. Treat the **weights** as the real signal, not the counts.

---

## Study by weight, learn in order

**Learn in order (1 → 2 → 3 → 4 → 5).** The domains build on each other: concepts and vocabulary (D1) feed the threat model (D2), which the architecture (D3) and operations (D4) defend against, all governed by the program layer (D5). Reading them out of order means relearning terms mid-stream.

**Spend hours by weight.** The exam is not balanced, so your study time shouldn't be either:

- **D4 (28%)** is the single biggest block — most of your hours go here.
- **D2 (22%)** is next — the threat/vuln/mitigation core.
- **D4 + D2 = 50%** of the exam. Half your score lives in two domains.
- **D4 + D2 + D5 = 70%** of the exam. Get these three solid and you're already at a comfortable pass margin before D1 and D3.

D1 (12%) is the lightest — much of it you likely already own from IAM/PAM/FFIEC work (CIA, AAA, least privilege, zero trust, change management). Skim to confirm CompTIA's vocabulary, don't grind it.

### 80-hour example allocation

Baseline split is straight proportional to exam weight; the **adjusted** column applies a **~1.2× bump** to any domain you're weak in (pull those extra hours from domains you already know cold — usually D1 and D3 for someone with your background).

| Domain | Weight | Baseline (proportional) | Weak-area adjusted (~1.2×) |
|---|---|---|---|
| D1 — General Security Concepts | 12% | ~10 hrs | ~8 hrs (strong → trim) |
| D2 — Threats, Vulns & Mitigations | 22% | ~18 hrs | ~21 hrs (if weak) |
| D3 — Security Architecture | 18% | ~14 hrs | ~12 hrs (strong → trim) |
| D4 — Security Operations | 28% | ~22 hrs | ~27 hrs (if weak) |
| D5 — Program Management & Oversight | 20% | ~16 hrs | ~16 hrs |
| **Total** | **100%** | **~80 hrs** | **~80 hrs (rebalanced)** |

The adjusted column is illustrative — re-weight it after your first simulator run reveals where you actually bleed points. Keep the total near 80; the bump is a reallocation, not an addition.

---

## Tested vs NOT tested

**Tested** — conceptual, scenario-driven, "what's the BEST control / first action / right tool" framing. You're being asked to *reason* like a practitioner, not recall a man page.

**NOT tested** — do not waste hours here:

- **Vendor config syntax** — no memorizing Cisco IOS lines, iptables flags, or specific GPO paths.
- **Writing real code** — you won't author scripts. You *may* be asked to **READ pseudocode or a snippet** in a PBQ and reason about what it does.
- **Deep cryptographic math** — know AES vs RSA vs ECC use cases, symmetric vs asymmetric, hashing vs encryption, key exchange purpose. You will **not** compute modular exponentiation or work a cipher by hand.
- **Specific CVE numbers** — understand vulnerability classes and CVSS severity bands; nobody asks you to recall "CVE-2021-44228."
- **Detailed legal case law** — know the *purpose and scope* of frameworks/regs (GDPR, PCI DSS, HIPAA, SOX) and concepts like due care/due diligence; not statutes, citations, or court rulings.

---

## Exam-day tactics

- **PBQs come first** — expect **~3–5** performance-based questions up front. Each can eat **3–5 minutes**. Do **not** sink 20 minutes here while 85 MCQs wait.
- **Flag and skip the PBQs**, clear every MCQ first. MCQs run **~1 minute each** and are worth the **same point value** as a PBQ. Bank the easy points, then circle back to PBQs with your remaining time.
- **Read the stem for negatives** — **NOT / EXCEPT / LEAST** flip the question. Misreading one is a free wrong answer.
- **Pick the BEST / most-complete answer.** Multiple options are often technically valid; CompTIA wants the strongest, most complete one. Eliminate the two obviously wrong, then choose between the two "right" answers on completeness.
- **"First step" ≠ permanent fix.** When a question asks the *first* action, it's almost always **contain / isolate / verify / document** — not the root-cause remediation. The permanent fix is a later step.
- **When stuck, lean on the frameworks** — **defense-in-depth** and **least privilege** are the safe defaults. The answer that adds a layer or reduces standing access is usually CompTIA's pick.
- **Never leave a question blank.** No penalty for guessing. Flag it, answer your best guess, move on, return if time allows.

---

## How to use this course

1. **Read the modules in order** — D1 → D5. Don't skip ahead; the vocabulary compounds.
2. **Drill your weak domains** — after the first pass, pour the ~1.2× bump hours into wherever you're shaky (weight-adjusted, per the table above).
3. **Take the simulator** — full-length, timed, PBQs-first, to surface gaps and build clock discipline. `practice-exam-simulator.html`
4. **Track in the dashboard** — log scores by domain, watch the weak-domain trend line. `study-dashboard.html`
5. **Cram the cheat sheet + one-pager in the final week** — high-density review of [[Last-Week Cheat Sheet]] and [[Exam-Day One-Pager]]. Run flashcards daily. `flashcards.csv`

---

## Module status

**Domain modules**
- [ ] [[Domain 1 — General Security Concepts]]
- [ ] [[Domain 2 — Threats, Vulnerabilities & Mitigations]]
- [ ] [[Domain 3 — Security Architecture]]
- [ ] [[Domain 4 — Security Operations]]
- [ ] [[Domain 5 — Security Program Management & Oversight]]

**Reference**
- [ ] [[Acronym Master List]]
- [ ] [[Last-Week Cheat Sheet]]
- [ ] [[Exam-Day One-Pager]]

**Drill tools** *(non-markdown — Obsidian won't wikilink these; open from the file system)*
- [ ] `practice-exam-simulator.html`
- [ ] `study-dashboard.html`
- [ ] `flashcards.csv`
