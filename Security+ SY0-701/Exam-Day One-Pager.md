# Exam-Day One-Pager — Security+ SY0-701

A calm tactical playbook. You know the material. This page is about converting that knowledge into points under a clock. Read it once the morning of, then close it.

> [!tip] Callout legend
> 🎯 = scoring tactic · ⏱ = time discipline · 🧭 = decision rule · ⚠️ = trap to dodge

---

## The format, stated plainly

- Up to **90 questions in 90 minutes**. That's your hard ceiling: **1 minute per question on average.**
- A mix of **multiple-choice (MCQ)** and **performance-based questions (PBQs)** — drag-and-drop, matching, firewall/ACL rule ordering, log analysis, command selection.
- **Every question is worth the same** toward your scaled score (passing is **750/900**). A 4-minute PBQ and a 15-second MCQ pay out identically. Internalize that — it drives the entire strategy below.
- The exam **lets you flag and review** within the section. Use it relentlessly.

---

## PBQ strategy — the single biggest lever

🎯 PBQs almost always appear **first**, usually **3–5 of them**, and each one can eat **3–5 minutes** if you let it. If you grind them in order, you can burn 20 minutes before you've banked a single easy point — and then panic-rush 80 MCQs.

**The discipline:**

1. Open the exam. See PBQs up front. **Flag every PBQ and skip it.** Do not start solving.
2. Drive straight through the MCQ pool. Clear every **~1-minute MCQ** first — they carry the *same point value* as the PBQs at a fraction of the time cost. This is pure points-per-minute optimization.
3. Once the MCQs are answered (and your guaranteed points are banked), **return to the flagged PBQs** with whatever time remains, working biggest-confidence-first.
4. ⚠️ **Never leave a PBQ unfinished or blank.** Partial credit is real on PBQs — a half-correct firewall ruleset or three-of-five correct matches still scores. An empty PBQ scores zero. Even if you're guessing the last drag, fill every field.

⏱ Rough cadence: if you've got ~85 MCQs and ~5 PBQs, plan to be **back at the PBQs by the 60–65 minute mark** with 25–30 minutes for them. That's comfortable. If you started with the PBQs, you wouldn't have known whether you had that buffer.

---

## Time budgeting

⏱ Anchor on **1:00 per question**, but spend asymmetrically:

- **Easy MCQ:** answer in 15–40 seconds. Bank it.
- **Hard MCQ:** if you're not converging in ~60–75 seconds, lock your best current answer, **flag it, and move.** Do not stare. A flagged question you revisit with a fresh head is worth more than a third re-read in the moment.
- **PBQs:** budgeted as a block at the end, per the section above.

**Checkpoints** (glance at the clock, don't obsess):
- ~30 min in → roughly a third of the MCQ pool cleared.
- ~60 min in → MCQs essentially done, pivoting to flagged items + PBQs.
- ~85 min in → everything has an answer. Final 5 minutes is review of flags only.

⚠️ The failure mode isn't running out of knowledge — it's running out of *clock* because two PBQs and four stubborn MCQs swallowed it. Flag-and-move is what prevents that.

---

## Reading the stem — where points quietly leak

🧭 **Negative stems (NOT / EXCEPT / LEAST).** These invert the task. You're hunting the **one wrong/worst option among three right ones.** When you see NOT, EXCEPT, or LEAST:
- Mentally flip the question to "three of these are true — find the odd one."
- A useful move: evaluate each option as true/false against the *positive* version of the stem, then pick the one that breaks the pattern.
- ⚠️ The most common careless miss on the whole exam is reading a NOT stem as a normal stem and confidently picking a *correct* statement — which is the wrong answer here.

🧭 **Qualifier words change the answer:** *MOST likely*, *BEST*, *FIRST*, *primary*, *greatest*. Underline them in your head. Two options can both be valid; the qualifier is the tiebreaker the question is testing.

🎯 **Read every stem fully — including all four options — before answering.** Security+ loves a strong distractor in position A and the *more complete* correct answer in C. Committing on the first plausible option is how you lose points you knew.

---

## "First action" logic

🧭 When a question asks what to do **FIRST**, it's testing sequence, not just correctness. The permanent fix is usually one of the options and is usually **not** the first step.

**General security-action order:** **contain / isolate → verify → document → then remediate (the permanent fix).** You stop the bleeding and confirm the problem before you re-architect anything.

**For incident-response scenarios, follow the IR lifecycle order:**

**Preparation → Identification (Detection & Analysis) → Containment → Eradication → Recovery → Lessons Learned.**

- "We just detected X — what FIRST?" → you're at **Containment** (isolate the host, disable the account, segment the segment). Not eradicate, not rebuild.
- ⚠️ Don't jump to **Eradication/Recovery** (reimage, patch, restore) when the scenario hasn't been contained yet.
- ⚠️ Don't skip **Identification** — if the question is "you see an alert," sometimes FIRST is *verify it's a true positive / analyze*, not pull the plug.
- "What do we do at the END / to prevent recurrence?" → **Lessons Learned** (and updating the IR plan / playbook).

🧭 If the scenario is digital forensics, **preserve the evidence and maintain chain of custody first** — order of volatility, image before you analyze. Never alter the source.

---

## Tie-breakers when two answers both look right

This is the difference between 740 and 770. When you've eliminated two and the last two both look defensible:

🧭 **Defense in depth.** The answer that adds a *layer* without removing one, or that reflects *layered* controls over a single point solution, is usually the intended answer. CompTIA rewards "and" over "either/or."

🧭 **Least privilege / need-to-know.** When the question is about access, the correct answer grants the **minimum** required — the most restrictive option that still lets the work happen. If one choice is broader than necessary, it's the distractor.

🧭 **BEST / MOST COMPLETE wins.** When two answers are both *technically correct*, pick the one that is **most complete and most directly addresses the stem's actual ask.** A correct-but-partial answer loses to a correct-and-comprehensive one. Re-read the qualifier and ask: which option most fully satisfies *that specific word*?

🧭 Secondary tiebreakers when the above don't separate them:
- **Preventive > detective > corrective** when the stem asks to *stop* something before it happens.
- **Address the root cause**, not the symptom.
- The answer that's a **recognized control/framework term** over a vague description.

---

## Discipline rules (the non-negotiables)

🎯 **Never leave a question blank.** There is **no penalty for wrong answers** on Security+. Every unanswered question is a guaranteed zero; every guess is a free shot at a point. Before you submit, confirm **every** item — MCQ and PBQ — has *something* selected.

🎯 **Educated-guess protocol** when you don't know: eliminate the obviously wrong (usually 2 of 4 fall fast), then apply the tiebreakers above — least privilege, defense in depth, most complete. Even a coin flip between two survivors beats a blank.

⏱ **Flag-and-move is a skill, not a weakness.** A flagged question is a deliberate deferral, not a failure. You'll answer it better on the second pass, and you protect the clock for the rest of the pool.

⚠️ **Don't second-guess into a worse answer.** On review, only change an answer if you find a *concrete reason* — a qualifier you misread, a NOT you missed, an option you skimmed. "It feels wrong now" is not a reason. Your first instinct on a question you understood is usually correct.

---

## Final mental checklist

> [!tip] The morning of
> - **Sleep** the night before beats cramming the night before. A rested brain reads NOT stems correctly; a tired one doesn't.
> - **Bring acceptable ID** (and a second form if your test center requires it — check the requirements the day before, not the morning of).
> - **Arrive early.** Buffer for parking, check-in, and the locker/sign-in process. Walk in unhurried.
> - **Breathe.** One slow breath before you start and any time you feel the clock-panic rising. It resets your reading comprehension, which is what actually gets tested here.
> - **Read every stem fully** — all four options, every time. Catch the qualifier. Catch the NOT.
> - **Trust the prep.** You've done the work. Today is execution, not learning. Clear the easy points, flag the hard ones, finish the PBQs, leave nothing blank.

You've got this. Go bank the points.

---

⬅ [[Security+ SY0-701 — Course Index]]
