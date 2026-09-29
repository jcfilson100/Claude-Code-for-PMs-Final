# Release 4.2: synthesis

**Owner:** Dispatch PM · **Last updated:** 28 Sep 2026 · **Status:** working draft. The data
findings need confirming against Ravi's official series.

## Headline

**1 in 4 responders has effectively been cut off since 4.2.** In the 16-responder sample, four
responders fell from about 12 pings a week to 0–1. The work concentrated on the busiest
responders, and fewer callouts are being taken overall. The recovering acceptance rate hides
all of this.

> **⚠ Not certain yet.** The help messages and the data file disagree about who stopped getting
> jobs (see "Do the tickets and the data agree?" below). Don't share this headline as fact until
> Ravi's official numbers show which one is right.

## Sources

| Source | What it is | Caveat |
|---|---|---|
| `00-rook/feedback/interviews/` | 4 handler interviews from Sofia's console-redesign research, 2–5 Sep | Framed around the console; handlers relay what their responders said |
| `00-rook/feedback/tickets/` | 25 support tickets, 13 Aug–5 Sep (15 from handlers, 10 from responders) | No pre-release baseline tickets in the folder |
| `00-rook/data/callout-history.csv` | 16 responders × 10 weeks (29 Jun–31 Aug), pings sent and taken | Source unknown, not Ravi's; contradicts about 7 tickets |

## What each source says

- **Interviews:** the most common complaints were offers vanishing, weak alerts and a
  hard-to-read console (3 of 4 each). Silence came up in 2 of 4, told gently.
- **Tickets:**
  - silence: 21 of 25
  - offers vanishing: 9 of 25
  - "Is my account broken?": 10 of 25

  No console complaints. Tickets get worse over time: quiet stretches grow from six days to
  almost a month.
- **Both sources agree on three things:** silence, vanishing offers, and nobody being able to
  explain either.

## Callout data: before vs after 12 Aug

| | Before (6 weeks) | Release week (10 Aug) | After (17–31 Aug) | Week of 31 Aug |
|---|---|---|---|---|
| Pings sent per week | 172 | 177 | 162 | 165 (−4%) |
| Pings taken per week | 132 | 96 | 111 | **120 (−9%)** |
| Taken rate | 76.8% | 54.2% | 68.5% | 72.7% |
| Share of pings to busiest 4 | 32% | 33% | 42–48% | **48%** |

- **The rate recovers faster than the counts.** About 100 fewer pings were taken over the four
  weeks from release week on.
- **The missing work is unaccounted for.** The four cut-off responders lost about 37 taken
  pings a week. The other twelve gained about 25, which leaves roughly 12 a week that nobody
  in the sample took. Either there were fewer incidents, responders outside the sample took
  them, or callouts went unfilled. That's unknown.

## Finding: how 1 in 4 responders got cut off

**Who:** Farlight, Meteor Mite, The Undertow and Vesper.

| Responder | Pings a week before 4.2 | Taken rate before 4.2 | Taken rate in release week | Week of 31 Aug | Taken since 17 Aug |
|---|---|---|---|---|---|
| Vesper | 13.8 | 82% (2nd best of 16) | 50% | 1 | 1 of 8 |
| The Undertow | 12.0 | 78% | 36% | 1 | 1 of 6 |
| Farlight | 12.0 | 74% | 40% | 0 | 0 of 4 |
| Meteor Mite | 11.2 | 69% (lowest of 16) | 40% | 1 | 1 of 7 |

1. **They weren't weak responders.** Vesper had the second-best taken rate in the file, so past
   performance doesn't predict who got cut off.
2. **One bad week decided it.** In release week, with the new 60s window, all four took under
   half their pings. That's four of the five lowest rates; Sgt. Bulwark also hit 50% and
   survived, so the line is close. The other eleven took 55–64%.
3. **The threshold is 60% (my estimate from the code, to be confirmed by Wen).** A taken ping
   adds 0.08 to the score and a miss takes away 0.12, so a responder must take at least 60% of
   their pings to hold their score.
   - Before 4.2, everyone took about 77%.
   - In release week, most fell below 60%, and the four fell furthest. My estimated score
     change for them is −0.24 to −0.52, against 0 to −0.12 for most others.
4. **The drop came a week later, then got worse.** They were still pinged normally in release
   week (10–12 each), then got 3–5 on 17 Aug, then 0–2. Since 17 Aug they've taken 3 of 25
   pings (12%), so each rare ping becomes another miss. This matches the tickets: "first one in
   weeks and it vanished."
5. **It's not their skills, account or handler.**
   - Their tags are in demand: The Undertow is aquatic and Farlight crowd-management, and their
     handlers see matching incidents.
   - Kip handles both Meteor Mite (1 ping) and The Gale (21 pings, the busiest in the file).
6. **It's a big share of the work, and half of it is invisible.** Before 4.2 the four took about
   28% of all callouts in the file. Meteor Mite and Vesper never filed a ticket; they surfaced
   only in interviews.

**Who to watch next:** Corporal Ashgrove and Halfmoon are down about 30% and still slipping. The
score maths doesn't fully explain their drop, so proximity may also be working against them.

**What this means for the fix:** one bad week under the new window was enough to push reliable
responders below a threshold they can't climb back over. The point release must do three things:
- let scores recover
- reset responders who are already stuck
- stop scoring "no answer" as harshly as "no", or restore the longer window

Resetting scores alone won't hold if the 60% trap remains. See `kill-switch-kpis.md` for the
guardrails.

## Do the tickets and the data agree?

*In plain words.*

We have two ways of knowing what happened after the update on August 12:
- **The tickets**: help messages people sent us.
- **The data file**: a chart that counts how many job offers each hero got and how many they
  said yes to.

We checked whether the two tell the same story.

**Where they agree:**
- Right after the update, heroes started missing job offers because the offers disappeared too
  fast. The chart shows it that same week, and the first help message came the very next
  morning.
- Two heroes, The Undertow and Farlight, really did stop getting jobs. Both the messages and the
  chart say so.

**Where they don't agree:**
- 21 messages say "my phone went quiet." The chart only backs up 3 of them.
- For the other 18, the chart says the opposite. Nightwell wrote "nothing in like 10 days," but
  the chart says she said yes to 14 jobs that same week. Stormwrack's helper said it was his
  quietest time ever, but the chart says it was his busiest.
- Two heroes, Vesper and Meteor Mite, stopped getting jobs in the chart but never sent a message
  at all.

**Timing:** the missed offers started right away, the week of the update. The chart shows the
quiet only starting the week after, and only for four heroes. Four messages say the quiet
started *before* the update, but the chart shows nothing unusual then.

**What that means:** both can't be true. Either the chart has mistakes in it (we don't know where
it came from), or people are wrong about how many jobs they got. They can't check their own
history, so they might be.

**What we can trust either way:**
- the jobs vanishing too fast
- The Undertow and Farlight getting cut off

**What to do next:** ask Ravi, the numbers person, for the official counts for Nightwell,
Stormwrack and Ironvale, the three where the stories clash the most. That will show which one to
trust. Until then, don't tell leadership "1 in 4 heroes got cut off" as if it's certain.

## Open questions (for Ravi and Wen)

1. Does the official per-responder data match this CSV, especially for the ~7 ticketed
   responders who show *more* pings here?
2. Incidents per week before and after 12 Aug, and unfilled callouts. Where did the missing
   ~12 taken pings a week go?
3. Actual recent-acceptance scores for the four cut-off responders, plus Ashgrove and Halfmoon.
   Do they confirm the 60% threshold?
4. How are the scores stored? If they're held in memory, a restart resets them, which changes
   the fix and the kill switch.
