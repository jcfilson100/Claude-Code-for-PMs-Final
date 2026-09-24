# Kill-switch KPIs for routing and availability changes

**Status:** draft for discussion with Marcus, Wen, Nadia, Ravi and Helen
**Scope:** any change to Dispatch routing, the Responder Availability Record, or Supply
scheduling that reads it. This covers the fixes for the 4.2 freeze-out and every change after.

A kill switch is a set of **thresholds agreed before a change ships**. If one is crossed, the
change is reverted first and discussed afterwards. The point is to take judgment out of the
moment. 4.2 showed why: the signs were visible from 17 Aug, and the team spent three weeks
debating whether it was seasonal.

---

## 0. Prerequisites: without these there is no kill switch

| # | Gap today | Why it matters | Owner |
|---|---|---|---|
| P1 | Routing values (`config.py`) ship only with the monthly release. There is no runtime override. | A "kill" would take up to a month. We need server-side flags for timeout, weights and penalties that can be reverted within hours. | Marcus / Wen |
| P2 | **Unconfirmed: where recent-acceptance scores are stored.** In the sample code (`history.py`) they sit in the program's memory, so a restart would reset everyone to neutral. But the freeze-out has lasted three weeks, which suggests production keeps them somewhere. Either way, scores have no decay, and reverting the config doesn't revert them. | If the scores are stored, rolling 4.2 back tomorrow would leave Farlight, Meteor Mite, The Undertow and Vesper frozen out, so every change needs a score snapshot and the kill must restore it. If they're in memory, deploys are silently resetting scores, which the kill design must account for. | Wen |
| P3 | `history.py` merges *decline* and *no answer* into one event. | We can't tell whether a fix changed responder behaviour or just timing. Both need logging separately. | Wen |
| P4 | Missing data: offers per responder per day, time-to-assign, unfilled callouts, and the score distribution. | The fairness guardrails in section 2 can't be computed without them. | Ravi |
| P5 | `callout-history.csv` contradicts handler tickets for about 7 responders. The CSV shows *more* offers where handlers report silence. | No kill decision should rest on unreconciled data (see the data trust gate in section 3). | Ravi / Nadia |

**Confidentiality:** per-responder metrics are computed on record IDs and reported as
distributions and counts. They never identify anyone, and no join may attempt to (Security
Policy 4.1).

---

## 1. The Rook-wide rule

Every qualifying change must ship with a one-page **kill sheet** stating:

1. The universal KPIs in section 2, plus any specific to the change (section 3).
2. A **baseline**: the 4 weeks before the change, from Ravi's data of record.
3. **Kill thresholds** and the window each one is measured over.
4. The **revert path**: which flags to flip and which score snapshot to restore (once P2 is
   confirmed).
5. **Who can pull it.** Any one of the PM, the EM (Marcus) or the Support Lead (Nadia) can call
   a kill without a meeting. Wen or the on-call engineer executes it, and Helen is informed the
   same day.

**Watch cadence:** daily for the first 7 days, then weekly for 4 weeks. After that, the change
graduates to normal reporting.

---

## 2. Universal guardrails (apply to every change)

### Tier 1: safety. Kill immediately.

| KPI | Definition | Kill if | Window |
|---|---|---|---|
| **Unfilled callouts** | Callouts where the ranked list ran out with no yes (`dispatch()` returns `None`) | Rises more than 10% above baseline | 24 h |
| **Time-to-assign, p90** | Incident created → a responder accepts, across *all* offers in the chain | Rises more than 20% above baseline | 48 h |
| **Coverage gap** | Incidents with no available responder carrying the required tags | Any rise above baseline noise. A routing change shouldn't move this, so a rise means availability or tags broke. | 48 h |
| **Supply contract** | `current_record()` shape unchanged, and maintenance booked into windows where the responder was then called out | Shape changes without Supply's sign-off, **or** conflicts rise more than 25% | Per release / 1 wk |

*Time-to-assign* is the one to watch most closely. The obvious fix, restoring the 90s timeout,
directly lengthens every chain where the first responder doesn't answer.

### Tier 2: fairness. Kill if crossed for 2 consecutive weeks.

These metrics would have caught 4.2. Aggregate acceptance cannot catch this failure, because
starved responders stop receiving offers and drop out of the denominator.

Figures are from `callout-history.csv` (16 responders). They are indicative until reconciled
with Ravi's data.

| KPI | Definition | Pre-4.2 baseline | Now (w/c 31 Aug) | Kill if |
|---|---|---|---|---|
| **Starved responders** | Available responders getting less than 50% of their own 4-week-average offers | 0 of 16 | **4 of 16** | Count rises above the pre-change count |
| **Overloaded responders** | Getting more than 130% of their own 4-week average | 0 of 16 | **7 of 16** | Count rises above the pre-change count |
| **Offer concentration** | Share of all offers going to the busiest 25% of responders | 31–33% | **48%** | Above 38% |
| **Floored scores** | Responders whose recent-acceptance score is at 0.0 | needs P4 | needs P4 | Any rise above the pre-change count |

In the first full week after 4.2 (w/c 17 Aug), starved responders went from 0 to 4 and
concentration from 32% to 42%. Either guardrail would have fired 10+ days before the team
paused to regroup.

### Tier 3: outcomes. Decide at the weekly review, not automatically.

| KPI | Baseline | Now | Red line | Target after fix |
|---|---|---|---|---|
| **Acceptance rate** (aggregate) | 75–78% | 73%, recovering | Below 70% for 2 weeks | ≥ 75%, **and** Tier 2 green. A recovery with rising concentration is not success. |
| **No-answer share** of non-accepts | needs P3 | — | Rises vs pre-change | Falls |
| **Time-to-accept** (median) | Ravi | — | +25% | Flat |
| **Callout-related tickets** | 1× normal | ~3× | Rises above the current level | Below 1.5× within 4 weeks; the "never goes off" theme near zero |
| **Active responder seats** (lagging; revenue) | Ravi | — | Any net loss among starved responders | Flat |

---

## 3. Per-priority kill criteria

### Priority 1: fixing the freeze-out

Each candidate fix has its own failure mode. Watch the extra KPI in addition to section 2.

| Fix | What could go wrong | Extra kill KPI |
|---|---|---|
| **Restore the 90s timeout** | Slower fill whenever the first responder doesn't answer | Time-to-assign p90 (Tier 1) is the deciding metric |
| **Stop treating no-answer as a decline** (or penalise it less) | Unresponsive responders keep a high rank, so offers stall at the top of the list | No-answer rate among the top 3 ranked responders is 1.5× baseline |
| **Let scores decay back to neutral** (the 2019 TODO) | Genuinely disengaged responders keep getting first offers | Same as above, plus the acceptance rate of recently recovered responders is below 50% |
| **Reset the frozen-out responders' scores** | The fix doesn't hold and they sink again | More than half of the reset responders are back at the floor within 2 weeks. That means the cause isn't fixed; escalate rather than roll back. |

A fix **succeeds** when starved responders return to 0 and concentration falls below 35%, with
Tier 1 green, over 3 consecutive weeks.

### Priority 2: correcting the metrics story

What's at risk here is a decision rather than production, so the "kill" is a **data trust gate**:
- No kill or ship decision uses a dataset until Ravi's official series and the working data
  agree within ±10% per responder-week.
- A new reporting metric runs in parallel with the old one for 2 weeks before replacing it.
- Every weekly acceptance report must show Tier 2 alongside the aggregate. A report with the
  aggregate alone is not valid.

### Priority 3: commitments and handler communication

| Solution | Extra kill KPI |
|---|---|
| **Availability Confidence** (if it's reinstated) | This touches the record Supply reads, so the Tier 1 Supply-contract guardrail applies in full. Also kill if the handler override rate on availability rises more than 25%, which would mean handlers distrust the score. |
| **Message to affected handlers** | Not a kill; revise the message if repeat tickets ("is my account broken?") from the same handlers don't fall within 1 week of sending. |

---

## 4. Open decisions for the group

1. Who approves runtime flag changes to routing, now that the config would no longer ride the
   release train?
2. Are the thresholds above right? They're proposed from a 16-responder sample. Ravi should
   recalibrate them on the full fleet, and they must be agreed **before** the first fix ships.
3. Should the Supply team adopt the same kill-sheet rule for its own changes? Recommended.
