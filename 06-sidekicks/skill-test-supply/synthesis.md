# Skill test: Rook Supply's slow approvals

*A test run of the `investigate-and-prototype` skill. Fictional course scenario.*

## Stage 0: Confidence gate (passed, 8 Oct)

| Dimension | Score | What I understand | Evidence |
|---|---|---|---|
| The ask | 95 | A full 7-stage run on Supply's slow requisition approvals, pausing after each stage | User: "Full run: all 7 stages" |
| Who you are | 95 | Acting Supply PM, reporting to Helen. Main user: Halloran (gear cage) | User: "Acting Supply PM"; CLAUDE.md People |
| Your goal | 95 | Prove the skill guides a clean, gated run. The Supply finding is the test case | User: "Prove the skill" |
| My role | 98 | Analyse and draft only, files in this folder, no agents, no posting | User: "files only"; CLAUDE.md |
| **Gate** | **95: passed** | | |

**First attempt:** the gate scored 60 and didn't pass. The goal and the depth of the run were
unclear, so I asked 3 questions before starting.

**Assumption:** the repo has almost no Supply evidence (no Supply tickets, no Supply data, no
Supply code). Thin stages will say so.

## Stage 1: Orient

### What Rook Supply is
- **Job:** keep each responder's gear working and accounted for, so a handler never has to guess
  whether it will hold.
- **Flow:**
  1. A handler raises a **requisition**.
  2. A **quartermaster** approves it.
  3. It's fulfilled and issued.
  4. Each item gets a **maintenance schedule** from its **service interval**.
  5. **Field failure reports** feed back into the item's history and can bring maintenance
     forward.
- **Users:** handlers (raise requisitions, file failure reports) and quartermasters (approve,
  fulfil, own the catalog and stock).
- **Link to Dispatch:** Supply *reads* the Responder Availability Record, which Dispatch writes,
  to book maintenance into quiet windows.

*Sources: `00-rook/company/supply-one-pager.pdf`, `glossary.docx`.*

### People

| Person | Role in this | Source |
|---|---|---|
| You (acting Supply PM) | Owns the investigation and the brief | User |
| Helen Achebe | Director of Product for both Dispatch and Supply; owns the roadmap | `who-does-what.xlsx`, roadmap |
| Halloran | Handler for Sgt. Bulwark; also runs the gear cage. Main voice of the problem | Interview `halloran.txt` |
| Quartermasters | Approve requisitions. **Not interviewed; no voice in the repo** | One-pager |
| Nadia Hoffmann | Support lead for both products | `who-does-what.xlsx` |
| Ravi Menon | Data analyst for both products | `who-does-what.xlsx` |
| Supply PM | **Nobody listed** | `who-does-what.xlsx` |

### Vocabulary
- **Requisition:** a handler's request for gear.
- **Quartermaster approval:** the one sign-off today.
- **Field failure report:** a handler's account of gear failing in use.
- **Service interval:** how often an item is due for maintenance.
- **Priority field:** a field on a requisition. Halloran says it changes nothing.

### Where things stand
- **The complaint (Halloran):**
  - A replacement vest plate, cracked and not cosmetic, waited **11 days** for a quartermaster
    signature.
  - The priority field "doesn't seem to change anything". There's one queue, "doesn't matter if
    it's laces or plate".
  - Field failure reports go "into a void".
  - Catalog search is poor.
- **On the roadmap:** "Requisition approval chains" is **committed for 4.3**, with an internal
  driver.
- **Its brief** (`06-sidekicks/briefs/requisition-approval-chains.txt`) proposes a **second**
  approval step for anything over $2,000. Its success measure is "zero big-ticket items shipped
  without a second sign-off". The scope then grows to:
  - auto-routing around slow approvers after 48 hours
  - a cross-site dashboard
  - handler visibility
  - replacing Halloran's spreadsheet
- **Also on the roadmap:** a handler phone app (Supply, Q4, exploring).

### Contradictions and gaps
1. **The committed fix could make the complaint worse.** Halloran's problem is that approvals are
   *too slow* and urgent items aren't prioritised. The 4.3 item adds a *second* approval step for
   expensive items. A vest plate could easily cost more than $2,000, so it could wait even
   longer. The brief's success measure counts sign-offs, not speed or safety.
2. **The two sources ask for different things.** The brief says Halloran asked for "a second set
   of eyes on the big-ticket items". In the interview, Halloran asks for *speed* and a working
   priority. Both may be true, but the brief has only one of them.
3. **Scope creep.** The brief grows from one approval step to five things. The overnight
   checklist run in `06-sidekicks/scheduled-run-output.txt` flagged this.
4. **The owner is unclear.** The brief names "Halloran's team, Supply" as owner, but Halloran is a
   handler who runs the gear cage, not a product owner, and there's no Supply PM.
5. **Failure reports vs the one-pager.** The one-pager says reports feed history and can trigger
   early maintenance. Halloran says he never hears what happens to them.
6. **The quartermasters' side is missing.** The people who approve have no voice in any file.
   We don't know *why* approvals take 11 days.
7. **No numbers.** There are no Supply tickets, no approval times and no Supply data or code in
   the repo.

### One-line summary
Supply's urgent gear is stuck in a single slow queue, and the fix already committed for 4.3 adds
another approval step. That could make the most important requests (safety gear) even slower.

## Stage 2: Listen

**Gate re-check: still passing (95).**

### Sources

| Source | Type | Whose voice |
|---|---|---|
| `00-rook/feedback/interviews/halloran.txt` | Interview, 5 Sep. Sofia came to ask about the *console*; Halloran raised Supply **without being asked** | Halloran (handler, runs the gear cage) |
| `06-sidekicks/briefs/requisition-approval-chains.txt` | Internal product brief, 14 Jul | Product, speaking *for* Halloran |
| `06-sidekicks/briefs/bulk-callout.txt` | Internal brief, 12 Jan, one line | Mentions Halloran |
| Support tickets | **None about Supply** | — |

**Caution:** this is essentially **one user**. Treat everything below as a signal to check, not
a pattern.

### Themes

| Theme | Raised in | How they said it |
|---|---|---|
| **Approvals are slow, with no reason given** | Interview | "takes too long and nobody can tell me why" (Halloran) |
| **The priority field does nothing: one queue for everything** | Interview | "doesn't matter if it's laces or plate" (Halloran) |
| **Safety risk while waiting** | Interview | A cracked vest plate waited **11 days** for one signature (Halloran) |
| **Can't see where a request is stuck** | Interview + brief | Interview: "nobody can tell me why"; the brief lists "let a handler see where their own requisition is stuck" |
| **Failure reports go nowhere** | Interview | "it goes into a void… I'd like to know it did something" (Halloran) |
| **Catalog search is poor** | Interview | "If I don't know the exact item name I'm not finding it" (Halloran) |
| **Wants a second check on big purchases** | Brief only | "a second set of eyes on the big-ticket items" (the brief, about Halloran) |
| **Kitting out several people at once** | Bulk-callout brief | Mentioned in passing (Halloran) |
| **Doing well: maintenance timing** | Interview | "It's gotten smarter about not booking maintenance into a week he's likely to be out" (Halloran) |

### Where the sources disagree

| Topic | Halloran's interview | The 4.3 brief | What it means |
|---|---|---|---|
| **The main problem** | Too slow; urgent items aren't put first | Too few checks on expensive items | The brief's core fix (a second approval) solves a different problem from the one in the interview, and adds delay |
| **What success looks like** | Urgent gear arrives fast, and he knows where it's stuck | Zero big purchases without a second sign-off | The brief's measure ignores speed and safety |
| **The brief's "extras"** | Matches his pain: see where it's stuck, route around a slow approver after 48 hours | Treated as scope creep | **The brief's extras fit Halloran's real need better than its main ask** |

They agree on one thing: **handlers can't see where their request is stuck.**

### Who never speaks
- **Quartermasters**, who approve. We don't know why approvals take 11 days: workload, unclear
  rules, or no alert.
- **Responders,** who wear the gear. Sgt. Bulwark was in the field with a degraded plate.
- **Other handlers.** Kip, Dot and Ambrose were never asked about Supply.
- **Quartermasters' managers,** the proposed second approvers. Would they add days?
- **Support.** No Supply tickets at all. Do Supply problems even reach Nadia?

### Summary (Stage 2)
One handler, unprompted, says urgent safety gear sits in a slow single queue with no
explanation. The committed 4.3 fix answers a different request (more checks) and could slow
urgent gear further. Its "extras" (visibility, routing around slow approvers) fit the real pain
better. The approvers themselves are silent.

## Stage 3: Count (blocked: no Supply data)

**Gate re-check: still passing (95).**

**Status: blocked.** The repo has no Supply data: no requisition records, no approval times, no
queue sizes, no Supply tickets. Following the skill's rule, **no numbers are invented.** This
stage records what we *do* have and exactly what data would unblock it.

### Every number the repo has about Supply

| Number | What it is | Source | How solid |
|---|---|---|---|
| **11 days** | Vest plate waiting for one quartermaster signature | Halloran interview | One person's memory of one request |
| **About 3 weeks** | Time since the vest plate was requested (as of 5 Sep) | Halloran interview | Same |
| **40 results** | Catalog search for "plate", "half of them unrelated" | Halloran interview | One search |
| **$2,000** | Proposed limit for a second approval | 4.3 brief | A design choice, not data |
| **48 hours** | Proposed time before routing around a slow approver | 4.3 brief | A design choice |
| **3 sites** | Number of sites the brief's dashboard would cover | 4.3 brief | A fact about Rook |
| **3 fields + a photo** | What a failure report takes to file | Halloran interview | A fact about the form |

**What these can't tell us:**
- whether 11 days is normal or unusual
- how many requests are urgent
- whether urgent ones wait longer than routine ones
- how many requests would hit the $2,000 second approval

### Data needed to unblock Stage 3

| # | Data | From | Answers |
|---|---|---|---|
| 1 | **Every requisition for the last 3–6 months:** created, approved, fulfilled and issued dates; priority set (yes/no); item type; cost; site; which quartermaster | Ravi (Supply data) | How long approvals take, and whether **priority** makes any difference |
| 2 | **Wait time split by priority and by item type** (safety gear vs routine), as a count of requests and a median and slowest 10% of wait days | Ravi | Do urgent safety items wait as long as boot laces? |
| 3 | **Wait time split by quartermaster and by site** | Ravi | Is it one bottleneck person or site, or everyone? (The fairness view) |
| 4 | **Share of requisitions over $2,000**, and how many of those are safety items | Ravi | How many requests the 4.3 second approval would slow down |
| 5 | **Time managers take to sign other things today** (if any approvals exist) | Ravi / Helen | A rough guess at how many days a second approval adds |
| 6 | **Field failure reports:** count filed, how many led to early maintenance, how long until anything happened | Ravi | Do reports really "go into a void"? |
| 7 | **Supply tickets or contacts to support,** even zero | Nadia | Do Supply problems reach support at all? |
| 8 | **Catalog searches:** top terms and searches with no click | Engineering | How bad search is, beyond one example |

### What the first table would look like (empty, to show the shape)

| Month | Requests | With priority set | Median days to approve: priority | Median days to approve: no priority | Safety items waiting more than 3 days |
|---|---|---|---|---|---|
| Jun | ? | ? | ? | ? | ? |
| Jul | ? | ? | ? | ? | ? |
| Aug | ? | ? | ? | ? | ? |

**The one number to ask for first:** median days to approve for **priority** vs **no-priority**
requests. If they're about the same, Halloran's "priority does nothing" is confirmed in one line.

### Summary (Stage 3)
Nothing to count yet. There's one strong anecdote (11 days for a cracked vest plate) and a clear,
small data request that would show whether it's typical. The most important split is priority
vs no priority.

## Stage 4: Trace (the process, not code)

**Gate re-check: still passing (95).**

There's no Supply code in the repo, so this traces the **process** from
`supply-one-pager.pdf`, `glossary.docx`, Halloran's interview and the 4.3 brief. Steps marked
*(unknown)* aren't described anywhere.

### How a piece of gear gets to a responder today

| Step | What happens | Who | What we know | Source |
|---|---|---|---|---|
| 1 | **Find the item** in the equipment catalog | Handler | Search needs the exact name. "plate" gave 40 results, half unrelated | Interview |
| 2 | **Raise a requisition** and set **priority** | Handler | The priority field exists and Halloran sets it | Interview, one-pager |
| 3 | **Wait in the queue** | System | One queue for everything. Whether priority changes the order: *(unknown)*. Whether the quartermaster is alerted: *(unknown)* | Interview |
| 4 | **Quartermaster approves** | Quartermaster | The vest plate waited **11 days** here. Why: *(unknown)* | Interview |
| 5 | **Fulfil and deliver** | Quartermaster / stock | Tracked "to delivery and issue". Times: *(unknown)* | One-pager |
| 6 | **Issue to the responder** | Handler | — | One-pager |
| 7 | **Maintenance is scheduled** from the item's service interval, in quiet weeks, using Dispatch's availability record | System (reads Dispatch) | Working: "gotten smarter". How it knows about callout load: *(unknown)*, because the record only holds stated availability | One-pager, interview |
| 8 | **Field failure report** when gear fails in use | Handler | Quick to file (3 fields + a photo). Goes into the item's history and *can* pull maintenance forward. The handler hears nothing back | One-pager, interview |
| 9 | **Replacement**: back to step 1 | Handler | Whether the failure report links to the replacement requisition: *(unknown)* | — |

### What we saw, and where it comes from

| What we saw | Step |
|---|---|
| Catalog search wastes time | **1:** search depends on exact names; no category filters |
| "Priority does nothing" | **3:** the queue probably ignores the priority field, or treats every request the same |
| "Nobody can tell me why" | **3–4:** no visible status, owner or reason while waiting |
| 11 days for a cracked vest plate | **4:** one approver, no urgency rule, no escalation |
| "Goes into a void" | **8:** reports are stored, but nothing tells the handler what happened |
| A cracked plate was reported but still waited | **8 → 9 → 3:** failure reports and replacement requests aren't connected, so a safety failure doesn't make its replacement urgent *(if they're linked, we don't know about it)* |
| Maintenance timing "got smarter" | **7:** works, but we can't explain why from the documents |

### Where the 4.3 change fits

| 4.3 item | Step | Effect on Halloran's problem |
|---|---|---|
| **Second approval over $2,000** (the core ask) | New step **4b**, after the quartermaster | **Adds a wait** to expensive items. Safety gear like plate may be over $2,000, so the slowest step gets a second slow step |
| Route around a slow approver after 48 hours | Step **4** | **Helps:** caps the wait at about 2 days per approver |
| Handler can see where a request is stuck | Steps **3–4** | **Helps:** answers "nobody can tell me why" |
| Dashboard of pending approvals across 3 sites | Steps **3–4** | **Helps quartermasters** see their queue |
| Replace Halloran's hand-kept spreadsheet | All steps | Probably a sign the system is missing things he tracks by hand |

### A new idea from the trace
**Link step 8 to step 2.** When a handler files a failure report on safety gear, the replacement
request could be **marked urgent automatically** and jump the queue. The vest plate was cracked,
which is exactly the case this would catch.

### What the trace can't explain
- **Why approvals take 11 days:** too much work, no alerts, unclear rules, or one person away.
  Only the quartermasters can tell us.
- Whether the priority field is stored but ignored, or never stored at all.
- Whether 11 days is typical or one bad case. That needs the Stage 3 data.
- How maintenance became "smarter" without callout data in the availability record.

### Summary (Stage 4)
The pain sits at steps 3–4: one queue, no urgency, no visibility, one approver. The 4.3 core adds
a second slow step at the same spot. Its extras, plus a new link from safety failure reports to
urgent replacement, address the real problem.

## Stage 5: Hypothesize

**Gate re-check: still passing (95).**

These are ranked by **how much testing each would reveal about the root cause** of slow
approvals. A separate column flags hypotheses that matter for a **decision**, even if they don't
explain the cause.

### Ranking

| Rank | Hypothesis | How likely it's true | What testing it reveals | Decision-critical? |
|---|---|---|---|---|
| **1** | **S1: The queue ignores priority** | High | Whether the system itself causes urgent items to wait | — |
| **2** | **S2: Nobody is alerted or escalated** when a request waits | Medium–high | Whether requests sit unseen rather than refused | — |
| 3 | S3: One approver or site is a bottleneck (workload) | Medium | Whether it's capacity, not the system | — |
| 4 | S8: 11 days is typical, not a one-off | Unknown | How big the problem is (the trust check) | Yes: sizes everything |
| 5 | S4: Approval rules are unclear, so quartermasters hold requests | Medium | Whether it's a policy problem | — |
| 6 | S5: The wait is really stock, not the signature | Low–medium | Whether we're looking at the wrong step | — |
| 7 | S6: Safety failure reports don't make the replacement urgent | Medium | A missing link that a cheap fix would close | — |
| 8 | **S7: The 4.3 second approval adds days to safety gear** | Medium–high | The impact of a planned change, not today's cause | **Yes: 4.3 is committed** |
| 9 | S9: Supply problems never reach support | Medium | Why there are no tickets | — |
| 10 | S10: Poor catalog search delays requests | High, but small | A step-1 delay, not the 11 days | — |

**Test first:** S1 and S2. One data pull answers S1. S2 needs a short talk with 2–3
quartermasters. **Test S7 before 4.3 ships,** even though it isn't the root cause.

### Table 1: what we saw and what we think

| Rank | Name | What we saw | Question | If… then… because… |
|---|---|---|---|---|
| 1 | S1: Priority ignored | "Priority… doesn't seem to change anything" | Does the queue order use priority? | **If** the queue ignores priority, **then** urgent and routine requests wait about the same, **because** they're handled first-in-first-out |
| 2 | S2: No alert or escalation | "nobody can tell me why"; nothing in the documents about alerts | Do quartermasters know a request is waiting, and for how long? | **If** there's no alert or ageing warning, **then** requests sit unseen for days, **because** approvers only see them when they happen to look |
| 3 | S3: Bottleneck | 11 days for one signature | Is it one person or one site? | **If** approvals depend on one busy approver, **then** waits cluster around them, **because** there's no backup |
| 4 | S8: Typical, not one-off | One example only | Is 11 days normal? | **If** most requests wait over a week, **then** this is systemic, **because** one bad case wouldn't move the median |
| 5 | S4: Unclear rules | Not described anywhere | Do quartermasters know what they're allowed to approve? | **If** the rules are unclear, **then** requests are held for checks, **because** approvers play safe |
| 6 | S5: Stock, not signature | "Fulfilment is tracked" | Is the wait for approval, or for stock? | **If** approval waits on a stock check, **then** the 11 days is a stock problem, **because** approvers won't sign for what isn't there |
| 7 | S6: Failure report not linked | The plate was cracked, yet the replacement waited | Does a safety failure report change the replacement's urgency? | **If** reports and requests aren't linked, **then** safety replacements get no priority, **because** the system doesn't know they're related |
| 8 | S7: 4.3 adds delay | Second approval over $2,000 | How many safety items would get the extra step, and for how long? | **If** many safety items cost over $2,000, **then** 4.3 slows the most urgent gear, **because** it adds a second sign-off with no urgency rule |
| 9 | S9: No route to support | Zero Supply tickets | Where do Supply complaints go? | **If** handlers go straight to quartermasters or keep spreadsheets, **then** problems stay invisible, **because** support never sees them |
| 10 | S10: Search delays | "40 results… half unrelated" | Does search slow down raising requests? | **If** search needs exact names, **then** requests start late, **because** handlers can't find items |

### Table 2: how we test it

| Rank | Null hypothesis (nothing's there) | Experiment | Supported if | Rejected if |
|---|---|---|---|---|
| 1 | Priority requests are approved faster | Median days to approve, priority vs no priority, 3–6 months | About the same | Priority clearly faster |
| 2 | Quartermasters are alerted and see waiting requests | Talk to 2–3 quartermasters; check for alerts or ageing views; compare "first viewed" vs "created" if logged | No alerts, and requests first viewed days later | Alerts exist and requests are seen quickly |
| 3 | Waits are similar across approvers and sites | Wait time by quartermaster and site | One approver or site much slower | Similar everywhere |
| 4 | Most requests are approved within a few days | Distribution of approval days | Median over a week | Median of 1–3 days; the vest plate was an outlier |
| 5 | Quartermasters know the rules | Quartermaster talks; look for written rules | Different answers about what needs checking | Clear, shared rules |
| 6 | Approval doesn't wait on stock | Timestamps per step; stock level on the request date | Approval delays line up with low stock | No link |
| 7 | Safety failure reports already speed up replacements | Time to approve replacements that had a failure report vs those without | No difference | Faster with a report |
| 8 | Few safety items cost over $2,000 | Share of requests over $2,000 that are safety gear; manager sign-off times elsewhere | Many, and sign-offs take days | Few, or sign-offs are fast |
| 9 | Supply issues reach support | Nadia's contacts about Supply; ask handlers where they go | Few or no contacts; handlers go elsewhere | Supply contacts exist |
| 10 | Search doesn't delay requests | Search logs: failed searches, time to request | Many failed searches | Rarely fails |

### Data and conversations needed

| Source | From | Tests |
|---|---|---|
| **Requisition records** (dates, priority, type, cost, site, approver) | Ravi | **S1**, S3, S4 (partly), S6, S7, S8 |
| **Talks with 2–3 quartermasters** (alerts, rules, workload, stock) | You (acting Supply PM) | **S2**, S4, S5, S3 |
| Stock levels by date | Engineering / quartermasters | S5 |
| Manager sign-off times | Helen / Ravi | S7 |
| Support contacts about Supply | Nadia | S9 |
| Catalog search logs | Engineering | S10 |

### Summary (Stage 5)
The most revealing tests are cheap: one data split (does priority change wait times?) and 2–3
quartermaster conversations (are requests seen and escalated?). Separately, check before 4.3
ships whether its second approval will slow safety gear.

## Stage 6: Brief

**Gate:** 90 at first. There's no request file for Supply, so the audience and the decision were
unclear. I asked the user, who chose "Helen: rethink 4.3". The gate then passed at 95.

**Output:** `06-sidekicks/skill-test-supply/brief.md`, one page for Helen.
- **Problem:** the 4.3 second approval adds a wait at the slowest step, with no fast track for
  safety gear.
- **Proposal:** keep the second sign-off, and add:
  1. an urgent lane for safety gear
  2. an alert to the next approver after 48 hours
  3. "where's my request?" for handlers
  4. a link from failure report to urgent replacement
- **Defer:** the dashboard and the spreadsheet replacement.
- **Before deciding:** two cheap checks in about a week, a priority-vs-no-priority data split
  (Ravi) and 2–3 quartermaster talks.
- **Asks of Helen:** hold 4.3's scope for one week, and put the urgent lane first.

## Stage 7: Prototype and test

**Gate re-check: still passing (95).** Built as a local file only. Not published, as the user
chose "files only".

**Prototype:** `06-sidekicks/skill-test-supply/urgent-lane.html`. You can flip between Today and
Reshaped 4.3 and move a day slider (0–12) to see:
- Halloran's failure report, search and "where's my request?"
- the quartermaster's queue, with an urgent lane and 48-hour backup
- Sgt. Bulwark's phone

All numbers are illustrative.

**Caught by checking version 1, before testing:**
- a "2nd sign-off" tag showed in the Today view, before that step exists
- the normal queue never moved

### Round 1: Halloran, a quartermaster (example), Sgt. Bulwark

*Simulated, not research.*

| User | Clear | Useful | Trust | Ship it | Their words |
|---|---|---|---|---|---|
| Halloran | 4 | 5 | 3 | 4 | "Who decides what counts as safety gear? Can I mark something urgent myself?" |
| Quartermaster | 3 | 3 | 2 | 3 | "If everything's 'urgent', nothing is… I can't approve what's not in stock." |
| Sgt. Bulwark | 3 | 2 | 3 | 3 | "When do I get the plate, and do I keep taking callouts with a cracked one?" |

**Fixed in version 2:**
- the wrong tag
- the queue now moves, and "With backup" shows after 48 hours
- the urgent rule is shown (safety list plus failure report; handlers can ask with a reason;
  quartermasters can move it back with a reason)
- stock level shown on urgent requests
- a responder view
- a "manager questions the spend after issue" note

### Round 2: Kip, a quartermasters' manager (example), Vesper

**Critical questions answered: 33%** (one yes, two partly, three no).

**Gaps found:**
- no order rule inside the urgent lane
- no list of all of a handler's requests
- no report of how often "urgent" is used
- no Supply volumes
- no Dispatch link for broken safety gear

### Priority chart

| Priority | Item | Fixes | Size | Needs data from |
|---|---|---|---|---|
| P1 | Order rule for the urgent lane (most critical first, then oldest) | Kip | S | — |
| P1 | "Urgent use" report for managers: how often, by whom, reasons, moved back | Manager | S | — |
| P1 | Run the two cheap checks before deciding 4.3 | Everyone | — | Ravi; quartermaster talks |
| P2 | "My requests" list for handlers | Kip | M | — |
| P2 | Responder updates for all gear, not just urgent | Vesper, Bulwark | S | — |
| P2 | Cross-product: damaged safety gear could limit callouts until replaced | Vesper, Bulwark | M | Dispatch team + Helen |
| P3 | Catalog categories and search | Halloran | S | Search logs |

## How the skill performed (test-run review)

| Stage | Worked? | Note |
|---|---|---|
| 0 Gate | ✅ | Caught 2 real gaps: 60 at the start, 90 before Stage 6. The questions fixed both |
| 1 Orient | ✅ | Found the main insight: the 4.3 fix may make the problem worse |
| 2 Listen | ✅ | "Where they disagree" and "who never speaks" did the heavy lifting |
| 3 Count | ✅ (blocked) | No data, so it produced a precise data request instead of invented numbers |
| 4 Trace | ✅ | Worked on a *process* with no code. Produced a new idea (failure report → urgent) |
| 5 Hypothesize | ✅ | Needed a "decision-critical?" column |
| 6 Brief | ✅ | The gate stopped a guess about the audience |
| 7 Prototype and test | ✅ | Checking version 1 caught 2 bugs; each round found new gaps |

**Improvements made to the skill after this run:**
1. a "Blocked" outcome for any stage
2. Stage 4 may trace a process
3. a "decision-critical?" column in the ranking
4. the gate asks "who is this for, and what decision?" before Stage 6 when there's no request file
5. Stage 7 tests every role in the flow, including the person at the end of it
