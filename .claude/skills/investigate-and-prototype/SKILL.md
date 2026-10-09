---
name: investigate-and-prototype
description: Take a product problem from "something went wrong" to a tested, clickable fix, in gated stages. Use when the user wants to investigate a release, incident, complaint spike or broken process end to end - build context, analyse feedback and data, trace the cause, form hypotheses, brief a decision-maker, and prototype and test the fix. Starts with a 95% confidence gate on the user's ask, who they are, their goal, and Claude's role.
---

# Investigate and prototype

A gated, 7-stage playbook. It works for Rook Dispatch or any other product or process.

**How to run it**
- Before any stage: pass the **confidence gate** (Stage 0).
- After each stage:
  1. Give a short plain-language summary.
  2. Save the stage output.
  3. Add it to the synthesis file.
  4. Ask the user before moving on.
- Never run several stages in one go.

Templates for every table and document are in `templates.md` next to this file.

**If a stage can't be done properly** (no data, no code, no source), mark it **Blocked**. Still
produce:
- what we *do* have, with how solid each piece is
- exactly what would unblock it: the data, its owner, and what it would answer
- the empty table it would fill, so whoever pulls the data knows the shape

Never fill a gap with invented numbers.

---

## Stage 0: Confidence gate (required before Stage 1, re-checked at every stage)

Score your understanding of four things from 0 to 100. Show the scores in a table, with the evidence for each.

| # | Dimension | Question to answer |
|---|---|---|
| 1 | **The ask** | What exactly does the user want delivered or decided, in what form, and how deep? |
| 2 | **Who they are** | Their role, who they report to, who the audience is, and what they can decide |
| 3 | **Their goal** | What success looks like, for whom, by when, and what's out of scope |
| 4 | **My role** | What I do, what I don't, and what I may do without asking |

For "My role", be explicit about:
- analyse, draft, build or decide
- whether I may post, publish or send anything outside the folder
- whether I may spawn agents

**Scoring guide (be strict, never round up):**

| Score | Meaning |
|---|---|
| **95–100** | I could restate it in one sentence and the user would say "exactly", and I can point to their words or a file |
| **80–94** | Right direction, but a detail that would change the output is missing |
| **50–79** | Partly inferred. I'm filling gaps with assumptions |
| **Below 50** | Guessing |

**The gate rule**
- **Pass only if every dimension is 95 or higher.** The overall score is the *lowest* of the four, not the average.
- **If it doesn't pass:**
  - Don't start the stage.
  - Ask up to 3 short, targeted questions about the lowest dimensions.
  - Use multiple-choice options when the likely answers are clear (AskUserQuestion), and say which option you'd recommend.
  - Re-score after the answers.
- **If the user says "skip the gate":**
  - Say which assumptions you're making, and label them in the output.
  - Record the skip in the synthesis file.

**Where evidence can come from:**
- the user's own words in this conversation
- the project's CLAUDE.md
- request or brief files in the project (for example a note from a director)

Never count your own guesses as evidence.

**When to re-run the gate:**
- at the start of every stage
- when the user redirects ("too much", "show me the rows")
- when a new stakeholder or audience appears
- before anything leaves the project (a post, publish or send)

Keep re-checks short: show the table and say "Still passing" or ask.

**Show the gate like this:**

| Dimension | Score | What I understand | Evidence |
|---|---|---|---|
| The ask | 95 | … | "…" (user, today) |
| Who you are | 98 | … | CLAUDE.md, People |
| Your goal | 80 | … | inferred. **Question below** |
| My role | 95 | … | … |
| **Gate** | **80: not passed** | | |

---

## Stage 1: Orient
- Read the company and product documents.
- Write a working-context section:
  - products
  - people and their roles
  - vocabulary
  - where things stand
  - contradictions and gaps between documents
- Record the user's role and who decides.

**Output:** working context in CLAUDE.md, or the project's context file.

## Stage 2: Listen
- For each qualitative source (interviews, tickets, reviews, surveys), group the material into themes. For each theme, count how many raise it and quote one line.
- Then compare the sources:
  - what's loud in one but rare in another
  - what appears in every source
  - who never speaks up (silent users)

**Output:** themes table, "where they disagree" table, a one-sentence summary.

## Stage 3: Count
- Compare before vs after the change.
- **Always show the underlying rows**, plus:
  - raw counts, not just rates
  - fairness: who gained, who lost, and how concentrated it is
- Cross-check the data against Stage 2 and flag every disagreement.
- Don't present a headline number as fact if the data's source is unverified.

**Output:** weekly table, a per-person view, a "1 in N" style headline (labelled if unverified).

## Stage 4: Trace
- If there's no code, trace the **process or workflow** from the documents instead. It works the
  same way. Mark steps nobody describes as *(unknown)*.
- Walk through the code or process in plain English, step by step, naming which file or team owns each step.
- Look for missing links between steps. They often suggest the cheapest fix.
- Map each finding from Stages 2–3 to the step that causes it.
- State what the evidence **can't** explain.

**Output:** a step table, and a "what we saw → where it comes from" table.

## Stage 5: Hypothesize
For each hypothesis, write:
- what we saw
- the question
- If… then… because…
- the null hypothesis
- the experiment
- supported if
- rejected if

Then:
- Rank them by how much testing each would reveal about the **root cause**, not just how likely they are.
- Add a **"Decision-critical?"** column. Some hypotheses rank low on root cause but must be tested
  before an upcoming decision, such as a release that's already committed. Call those out.
- List the data needed and who owns it.

**Output:** ranking table, two hypothesis tables, a data-request list.

## Stage 6: Brief
- **First read the decision-maker's actual request,** if it exists as a file or message.
- **If there's no request,** re-run the gate and ask: "Who is this for, and what decision should it
  help them make?" Offer 2–3 options. Don't guess the audience.
- Write one page that answers only that ask, from the affected user's point of view.
- Move the analysis to the synthesis file.

**Output:** `brief.md`.

## Stage 7: Prototype and test
1. Build clickable prototypes of the top-scoring options. Use the project's design system if one exists; otherwise label the styles as prototype-only.
2. Check version 1 yourself before testing: click every control and step through every state.
   This catches bugs testers shouldn't have to find.
3. Test with 3 simulated users who are in different situations. Label it "simulated, not
   research". Include **every role in the flow**, including the person at the end of it (for
   example, the responder who wears the gear), not only the person using the screen.
4. Fix the P1 and P2 issues.
5. Re-test with 3 *different* simulated users, each asking critical questions.

**Output:** prototypes, feedback summary, a priority chart (template in `templates.md`).

---

## Rules (from experience)
1. **Label every guess:** illustrations, assumptions and unconfirmed data, on the page itself.
2. **Show the rows, not just the number.** Averages hide the people who are hurt.
3. **Look for disagreement between sources.** That's where the real finding usually is.
4. **Answer the actual ask.** If the user says "too much", cut. Don't defend.
5. **Different testers each round.** The same personas stop finding new gaps.
6. **One living synthesis file** is the single source of truth.
7. **Privacy:** never infer anyone's real identity. Use only the data's own identifiers.
8. **Ask before anything leaves the project:** posting, publishing, sending, or writing outside the folder.
9. **Use the user's reading level and plain words.**
10. **If this skill conflicts with the project's CLAUDE.md, follow the CLAUDE.md.**
