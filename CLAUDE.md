# Rook Industries — course working file

## Session scope — Product School lab

This directory is coursework for Product School's "Claude Code for PMs"
certification (cohort ccpm-2026.1). Everything in it is a fictional
teaching scenario.

- Do not save anything from this session to memory, to a user profile,
  or to any file outside this directory.
- Do not carry context from this directory into unrelated sessions.
- Rook Industries is not a real company. Nothing here is a fact about
  the world.
- Read and write only within this directory.

<!-- Keep the block above at the top of this file. Everything you add
     during the course goes below this line. -->

---

## Working context

The user is the new PM for **Rook Dispatch**. They started Monday 31 Aug 2026, taking over from
Priya Raghunathan, who left 21 Aug with no overlap. Sources: `00-rook/company/`.

### The company
- Rook sells software to independent masked responders and their handlers and quartermasters.
  It does not employ responders. It has 241 staff, mostly remote, and charges a subscription
  per active responder. Releases ship monthly, numbered 4.x; the current release is **4.2**.
- **Confidentiality is a hard rule.** Rook never stores or reconstructs a responder's legal
  identity. It holds only capability tags, availability windows and callout history. Never
  design features, analysis or data joins that assume or attempt identity mapping (see Security
  Policy 4.1).

### The products
- **Rook Dispatch** (the user's product): an incident comes in → Dispatch ranks available
  responders by routing priority → a callout offer goes to the top responder's phone →
  accept, or decline/timeout and move to the next → acceptance assigns the incident.
  Handlers use the web console; responders use the mobile app.
  - **Metrics:** *acceptance rate* is the headline metric (accepted ÷ offers, reported weekly,
    in aggregate; declines and timeouts both count as not accepted). Also *time-to-accept*
    (median seconds) and *coverage gap*.
  - Routing config ships with each release. Handlers cannot tune it at runtime.
- **Rook Supply**: requisitions → quartermaster approval → fulfillment → maintenance schedules
  → field failure reports. Used by handlers and quartermasters.
- **Coupling:** Dispatch *writes* the **Responder Availability Record**. Supply *reads* it to
  schedule gear maintenance into low-callout windows. Any change to how Dispatch calculates
  availability silently changes Supply's scheduling, so flag Supply impact on availability work.

### People
| Person | Role | Location | Notes |
|---|---|---|---|
| Helen Achebe | Director of Product (Dispatch & Supply) | Chicago | The user's manager. Owns the roadmap; changes to committed items go through her |
| Marcus Oyelaran | Engineering Manager, Dispatch | Chicago | Direct; first stop for anything unclear |
| Wen Li | Staff Engineer, routing | Berlin | Built the routing logic; the only real source on how ranking works. Was on PTO 14–24 Aug |
| Sofia Marino | Product Designer | Chicago | Console and mobile app |
| Nadia Hoffmann | Support Lead (both surfaces) | Berlin | Sees ticket trends first; tracking the 4.2 complaint split |
| Ravi Menon | Data Analyst (both surfaces) | Singapore | Owns the official weekly acceptance numbers; request via #data |
| Priya Raghunathan | Previous Dispatch PM | — | Left 21 Aug 2026 |

### Vocabulary
- **Responder:** independent operator with a *cover identity* (public persona). In our systems a
  responder is only a record. **Handler:** looks after one or a few responders and is the main
  user of the product. **Quartermaster:** owns equipment stock (Supply).
- **Callout:** a request to attend an incident. **Callout offer:** one callout offered to one
  responder. **Decline** and **timeout** are recorded separately in the data, but both move the
  offer on.
- **Callout timeout:** how long an offer stays live. It is the same for everyone and set per
  release (60s since 4.2, previously 90s).
- **Routing priority:** the ranking score. Inputs are proximity (travel-time estimate since
  4.1), availability, capability match and *recent acceptance history*. Declines and timeouts
  lower the recent-acceptance component, which lowers future priority until it recovers.
- **Coverage gap:** nobody *could* go (no matching tags). This is different from a low
  acceptance rate, where nobody *would* go.
- **Mutual aid:** cross-region cover. Not supported yet; Q4 exploration ("shared cover").
- "**Who gets pinged**" is the team's informal name for routing priority and weighting.

### Where things stand (as of early Sep 2026)
**4.2 shipped 12 Aug** with two routing changes at once:
1. Proximity was weighted up relative to recent acceptance history. This was a long-standing ask
   from wide-geography responders.
2. The offer timeout was cut from 90s to 60s.

**Since 4.2:**
- Callout tickets are running about 3× normal. Roughly ⅔ say "my phone never goes off" and ⅓ say
  "it was gone before I could answer". Nadia can explain the second theme (the shorter timeout)
  but not the first.
- A handler emailed support directly, which is unusual.
- Acceptance rate is reported down, but only "rough" numbers exist so far (from Marcus). Nobody
  has pulled Ravi's official weekly series.

**Open questions and cautions:**
- **Priya believed the drop is mostly seasonal** ("August is always soft") and argued against
  reverting 4.2. This is an unverified hypothesis from the person who made the change. Test it
  against prior Augusts before accepting it.
- Two variables changed in one release. A shorter timeout by itself turns more offers into
  timeouts, which lowers acceptance rate. Separate the two effects.
- **Marcus's unanswered question (14 Aug):** was the reweighting meant to apply to responders
  who have been declining? The config doesn't distinguish them. It isn't clear whether that was
  a decision or an accident.
- **Availability Confidence** (a confidence score shown next to stated availability) was a Q3
  roadmap item *committed to 4.2*, driven by support escalations. It is **not** in the 4.2
  release notes. Priya says items were "squeezed out" and asked for a conversation with Helen
  about what is still committed. That conversation hasn't happened. Note that this feature would
  touch the Responder Availability Record, which Supply reads.
- **There is no written spec of how routing works.** Priya asked the new PM to write one, with
  Wen as the source.
- The team deliberately paused (28 Aug) to let the new PM look at the 4.2 picture fresh, and
  will regroup about a week after the start date. Nadia will bring a ticket breakdown.

**Rest of the roadmap:** Supply requisition approval chains are committed for 4.3. The Supply
handler phone app and Dispatch shared cover between responders are Q4 explorations.

### Contradictions and gaps across 00-rook/ (all files reviewed)
**Contradictions**
- **The callout data and the tickets disagree.** `data/callout-history.csv` (16 responders,
  source unknown, not Ravi's) shows four responders frozen out since 4.2: Farlight, Meteor Mite,
  The Undertow and Vesper. But it shows *more* offers for about seven responders whose handlers
  report silence: Nightwell, Ironvale, Stormwrack, Cindermark, Falkirk, The Drift and The
  Longcast. *Update, session 3:* "offers sent but never reach phones" can't explain these rows,
  because they show high *accepted* counts, and an accept needs a reply from the phone. Treat
  the CSV as unverified for these 7.
- **The code doesn't match the glossary.** The glossary says declines and timeouts are distinct
  and that the score recovers. The code penalises both the same (−0.12 per miss vs +0.08 per
  accept), with a floor at 0 and no decay; the decay TODO has been open since 2019.
- **The routing code's claim is misleading.** The README says "nobody is removed". Offers stop
  at the first yes, so anyone at the bottom of the ranking is effectively never asked.
- **Who asked for the 4.2 routing change is unclear.** The roadmap says "Internal", the release
  notes say responders asked for it, and Priya says it came from wide-geography responders. The
  60s timeout has no stated rationale.
- **"Seasonal" isn't supported.** July and early August were flat (75–78%) and the drop came in
  release week. There is no prior-year data, and Stormwrack's handler sees the opposite.
- **The handover got several things wrong.** Filter persistence produced no tickets; Ambrose
  instead reports a real bug where it silently resets after updates. "Mobile stable since 4.1"
  is untested against "phone never goes off". Numbers come from Ravi, not Marcus.
- **Availability Confidence** is "locked" for 4.2 on the roadmap but didn't ship, with no record
  of it being dropped.
- **Supply's description doesn't fit the availability record.** Supply says it schedules
  maintenance around callout load, but the availability record holds only stated availability.
  Halloran says scheduling "got smarter" recently, with no explanation of what changed.
- **Names differ between files.** "Mr. Ambrose"/"Aunt Dot" in the CSV vs "Ambrose"/"Dorothy
  Pell" elsewhere, so joins on names break.
- **Unconfirmed:** whether scores are stored or held in memory. In the sample code they're in
  memory and a restart would reset them. Ask Wen.

**Missing**
- Security Policy 4.1, a routing spec, and a 4.2 decision record, including who approved the
  config changes.
- Data: declines vs timeouts, time-to-accept, coverage gap, unfilled callouts, prior-year
  numbers, responder location, and anything beyond 16 responders.
- A Supply PM (none in the directory; the new Dispatch PM isn't listed either). Sofia's console
  redesign isn't on the roadmap.
- **Asks from users:**
  - responders can't see their own callout history (T-008)
  - no handler alert when an offer is live on the responder's phone
  - per-responder alert sounds
  - dark mode
- **For Supply:** Halloran's cracked vest plate waited 11 days because the priority field has no
  effect.
- **Handling risk:** interview transcripts capture household details about responders. Check
  handling against Policy 4.1, and never use them to infer identity.

See `kill-switch-kpis.md` for the proposed guardrails on any fix.

### Conclusions so far (session 1)
- **The mechanism:** with the timeout at 60s, more offers go unanswered. Each unanswered offer
  scores as a decline, so the responder drops down the ranking, gets fewer offers, misses the
  rare one that arrives, and sinks further. Nothing lets them recover. In the CSV, four
  responders fell from about 12 offers a week to about 1, while the busiest quarter's share of
  offers went from 32% to 48%.
- **Aggregate acceptance hides it:** by week in the CSV it went 77% → 54% (release week) → 66% →
  67% → 73%. The rebound partly happens because starved responders drop out of the
  denominator, so "it's recovering, it was seasonal" is the trap to avoid. Always show fairness
  metrics next to the aggregate.
- **Top 3 priorities agreed:**
  1. Confirm and fix the freeze-out with Wen and Marcus, using Ravi's data, ideally in a point
     release.
  2. Correct the "seasonal" story before the regroup and reconcile the CSV with the tickets.
  3. Settle Availability Confidence and hotfix approval with Helen, and give Nadia a message for
     affected handlers.
- **A fix doesn't require reverting the proximity reweighting.** The levers are the timeout,
  how no-answers are scored, score recovery, and resetting the frozen-out responders. Any fix
  needs a runtime kill switch, which doesn't exist yet.
- **Working style:** keep deliverables as files in this folder rather than publishing them.
  Prompts worth keeping go in the `prompts.md` of each module folder.
- **How to explain things (user's standing request, 28 Sep):** use simple, 5th-grade language.
  Short sentences, everyday words ("heroes", "helpers", "job offers", "help messages", "the
  chart"), and plain sections like "Where they agree / Where they don't / What that means /
  What to do next". Keep exact numbers and names, but explain any jargon, or skip it.

### Conclusions so far (session 2: interviews and tickets)
- **Interviews** (4 handlers, from Sofia's console-redesign research):
  - offers vanish before the responder can answer: 3 of 4
  - handler alerts are too weak: 3 of 4
  - the console is hard to read: 3 of 4
  - uneven or quiet work: 2 of 4

  The plain-language summary page is `interview-signals.html`.
- **Tickets** (25, filed 13 Aug–5 Sep; 15 by handlers, 10 by responders):
  - quiet only: 16
  - vanished only: 4
  - both, i.e. first offer in weeks then lost: 5
  - "Is my account broken?": 10
  - handler has no answer to give: 4
- **Overlap:** only three themes appear in both sources: silence, vanishing offers, and nobody
  being able to explain either. No console complaint appears in any ticket.
- **Tickets undercount the problem.** Vesper and Meteor Mite are frozen out in the CSV but never
  filed a ticket, and overloaded responders (7 of 16 in the CSV) never file at all. Use Ravi's
  per-responder data to find everyone affected.
- **The tickets get worse over time, not better.** Reported quiet stretches grow from six days to
  almost a month, severity rises to Medium and High, and handlers file follow-up tickets.
- **The before/after comparison is incomplete.** There are no pre-12 Aug tickets in the folder, so
  ask Nadia for June–July tickets as a baseline. Six tickets say trouble began before the
  release; the CSV doesn't show it. Check with Ravi before calling 4.2 the only cause.
- **One-line summary:** 4.2 split responders into starved (quiet for weeks, then losing the rare
  offer) and overloaded, with nobody able to see why.
- **Next steps, ranked by ticket count:**
  1. Fix the silence (21 tickets).
  2. Give responders time to answer (9 tickets).
  3. Send handlers an honest message (10 "is it broken?" tickets).

### Conclusions so far (session 3: the callout data and root cause)
- **`synthesis.md` is now the main 4.2 document.** It holds the data before and after 12 Aug,
  the "1 in 4" finding, the ticket-vs-data check, the two root-cause checks, and the
  five-investigator result. Add new findings there, in plain language.
- **Raw counts matter more than the rate.** Pings taken are 132 a week before 4.2 and 120 in the
  week of 31 Aug (−9%), while the rate looks almost recovered (73%). Always show counts and
  fairness next to the rate.
- **"1 in 4 responders effectively cut off"** (Farlight, Meteor Mite, The Undertow, Vesper) is
  the headline for leadership, but it's flagged *not certain* until Ravi confirms the CSV. All
  four are backed by tickets or interviews.
- **Tickets vs CSV:** the CSV backs only 3 of 21 "phone went quiet" tickets. For Nightwell,
  Stormwrack and Ironvale they tell opposite stories; these three are the test cases for
  Ravi's data.
- **Likely root cause** (5 investigators agreed, medium confidence): 4.2's miss surge (60s
  timeout as main suspect; late push delivery is a live alternative for the "gone in seconds"
  tickets) plus the old no-recovery score rule (60% break-even), made worse by shipping two
  changes at once with no rollout, fairness checks or kill switch.
  - **Unresolved:** why *these* four (location vs late delivery).
  - **Don't** revert "closer heroes first" alone; it would make the trap worse.
  - **Wait for the per-offer log** before choosing 60s vs 90s.
- **Scores probably are stored**, not held in memory: no stuck responder ever rebounds. Still
  confirm with Wen.
- **Watch list for a month:** the four cut off first, then Ashgrove and Halfmoon (slipping), then
  Nightwell, Stormwrack and Ironvale (data test cases), then the overloaded The Gale, Falkirk and
  Vantage.
- **Repo:** all PRs (#1–#3) are merged into `main`. The old module branches can be deleted.
  Start each module on a fresh branch off `main`.

### Conclusions so far (session 4: the routing code)
- **The code walkthrough, hypotheses and points breakdown are in `synthesis.md`.**
- **Flow:** `offer.py` asks `routing.py` for an order. `routing.py` uses `availability.py` (who's
  free, travel time) and `history.py` (says-yes score). `offer.py` buzzes heroes one at a time
  until the first yes. All numbers live in `config.py`.
  - In this folder, **4.2 changed only three numbers in `config.py`**.
  - Push, polling and travel time are **empty outlines**, so this folder is a simplified
    snapshot. For example, bulk callout (4.1) isn't in it.
- **Points:**
  - Only a yes adds points (+0.08).
  - "No" and "no answer" both cost −0.12.
  - Nothing else restores a score: no decay, no reset, no credit.
  - A hero at 0 needs 7 straight yeses to reach 0.5 and 13 to reach 1.0, while getting about 1
    offer a week.
- **New clue:** the 60s clock starts when the offer is **sent**, not when the phone receives it
  (`offer.py`). The 4.2 release notes also include a "duplicate push on re-offer" fix that isn't
  in the routing changelog. So **"4.2 was just config" is unproven** beyond this folder.
- **No location or travel-time data exists anywhere in the repo.** Other possible data sources:
  - the routing override audit log (shipped 4.0)
  - whether the CSV counts bulk callouts (shipped 4.1)
- **Hypotheses H1–H12** are in `synthesis.md`, framed with the scientific method.
  - Test **H2 (late delivery)** and **H1 (shorter answer time)** first.
  - One offer-by-offer log from Wen tests most of them.
- **Marcus's 14 Aug question is answered:** the 4.2 weights applied to everyone, including
  existing decliners, and actually softened their penalty. Open checks for Wen:
  - whether the deploy reset scores
  - whether 4.2 had a gradual rollout
  - whether the decision was deliberate
  - the 4.1→4.2 diff
- **Slack:**
  - Posted to `product-school.slack.com`, channel `C0B8LTV13EJ`: the Marcus reply
    (p1790933401443459) and the "they'd need to say yes" summary (p1791243919710579).
  - Waiting on Marcus and Wen. Read the channel for replies next session.
  - Use the **Slack connector**, not Chrome (Chrome wasn't connected).
  - It's a real workspace, so **never @-tag** fictional people.
  - The session scope says to stay inside this folder, so posting to Slack needs the user's
    explicit OK each time.
- **Next module:** Helen's request (`05-super-speed/director-request.txt`) for a one-pager plus a
  clickable prototype, showing the fix from the handler's and responder's point of view.
- **PR #4** (`module-4-x-ray-vision`) is open, not merged. *(Update: merged in session 5.)*

### Conclusions so far (session 5: brief and prototypes for Helen)
- **Helen's ask** (`05-super-speed/director-request.txt`): what we'd build, from the point of view
  of the people it happens to, not a setting. She also wants something clickable. Keep briefs to
  her short. She already knows the cause, so cut the analysis, numbers and asks.
- **`05-super-speed/brief.md`** (posted to Slack, with a thread reply linking the prototypes).
  "What we're starting with":
  1. **A way back:**
     - "ran out of time" costs −0.04
     - recovery of +0.05 a week, up to the middle (0.5)
     - a full fresh start for anyone stuck since 12 Aug
  2. **"Why haven't I had offers?"** screen.
  3. **"I'm coming"** +30s button.
- **The recovery simulator** (`recovery-simulator.html`) reproduces the real four stuck under
  today's rules. Lessons:
  - recovery alone frees no one
  - the fresh start must restore a full standing
  - preventing the *next* freeze-out depends on the miss split (H4) and the clock fix
  - confidence: 85 for freeing the four now, 60 for preventing it happening again
- **Three shareable prototypes**, published as private claude.ai Artifacts (version 2). Republish
  to the same file path to keep each URL:

  | Prototype | File | Link |
  |---|---|---|
  | The Way Back | `way-back.html` | claude.ai/artifact/JMN1G6tKVZ3kj15biChSQe |
  | Why Haven't I Had Offers? | `why-no-offers.html` | claude.ai/artifact/WLcNbCdkq6rQSpyXfUMnm5 |
  | The I'm Coming Button | `im-coming.html` | claude.ai/artifact/FvC4cjhMW9BSwRYpffo3MW |

  The user made the exception to the folder-only rule for these. Sharing with Helen is done by
  the user through each page's Share menu.
- **Simulated hero tests** (role-play, not real research): round 1 and round 2 results and the
  current **priority chart** are in `synthesis.md`. The next step was to build the round 2 P1s:
  - an early warning before a hero gets stuck
  - "checking" instead of "that's on us"
  - an owner and a date for "we are checking"
  - slipping heroes (Halfmoon, Ashgrove) in The Way Back
- **Repo:** PR #5 is merged. **PR #6** (`module-5-prototypes`, the three prototypes plus session 5
  wrap-up) is open.

### Conclusions so far (session 6: skills)
- **Two project skills** live in `.claude/skills/`. They work in this folder only.
  - **`investigate-and-prototype`:** the 7-stage playbook (orient, listen, count, trace,
    hypothesize, brief, prototype and test), with a **Stage 0 confidence gate**. It scores the
    ask, who you are, your goal and my role. All four must be 95+; the gate is the lowest score.
    It re-checks at every stage and before anything leaves the folder. It has a "Blocked"
    outcome (never invent data), process tracing, a "decision-critical?" column, an audience
    check before the brief, and tests every role in the flow.
  - **`review-checklist`:** a read-only check of any brief for four things: owner named, success
    measure, scope stays bounded, problem before fix. Matches the course's expected result on
    `06-sidekicks/briefs/`. Trigger it with `/review-checklist` or "review/check/vet <brief>".
- **Skill test run:** `06-sidekicks/skill-test-supply/` holds a full 7-stage run on Rook Supply's
  slow approvals (synthesis, brief for Helen, `urgent-lane.html` local prototype).
  - **Finding:** the committed 4.3 second approval would add a wait at the slowest step.
  - **Proposal:**
    - an urgent lane for safety gear
    - 48-hour backup approvers
    - "where's my request?" for handlers
    - a failure report automatically marking the replacement urgent
  - **Two cheap checks first:** a priority-vs-no-priority data split (Ravi) and 2–3 quartermaster
    talks.
- **Both briefs fixed** to pass review-checklist: owner and proposed builders, plus "we'll know it
  worked when". The Slack copy of Helen's brief is the older version.
- **Shareable skill:** `06-sidekicks/share/review-checklist/` (generic examples, README) and
  `review-checklist.zip` (git ignores zips). Posted to Slack `C0B8LTV13EJ` as text, because the
  connector can't attach files (p1791506130915789, with the skill in the thread).
- **Scheduled-run lesson:** a pasted Monday run was wrong. It listed bulk-callout twice (by file
  name and by title), missed `handler-phone-app.txt`, and still printed "4 checked". Proposed fix,
  **not yet added**: a self-check before the summary (one block per file name, file count must
  match, no duplicates, full format).
- **Weekly schedule not created.** The scheduled-task tool stores tasks in
  `C:\Users\jcfil\.claude\scheduled-tasks\`, outside this folder, which the session scope forbids.
  It needs the user's explicit exception. (Tasks run only while the app is open; a missed run
  fires at next launch.)
- **Comic editions** of the three prototypes (`*-comic.html`, `comic-skin.css`) are published
  separately; the originals are unchanged. There is no Rook design system in the repo. Sofia
  would own the real one.
