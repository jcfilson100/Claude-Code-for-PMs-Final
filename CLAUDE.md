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
  Longcast. One hypothesis: offers are logged as sent but never reach phones, then time out and
  are scored as misses. Treat the CSV as unverified.
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
