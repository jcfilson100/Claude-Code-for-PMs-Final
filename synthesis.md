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

## Looking for the cause: two checks

*In plain words. These use the chart (the data file) and the rules written in the routing code.
They show what is **likely**, not what is proven. Wen can prove it with the real scores.*

### Check 1: was it the shorter answer time, or the new "closer heroes first" rule?

The update changed two things at the same time:
- **Shorter answer time:** heroes got 60 seconds to answer instead of 90.
- **"Closer heroes first":** being close to the emergency now counts for more.

We want to know which one pushed the four heroes out.

**The clue: what came first, the misses or the drop in offers?**

| Hero | Offers a week before the update | Update week (10 Aug) | Missed in update week | The week after (17 Aug) |
|---|---|---|---|---|
| Farlight | 12 | 10 (−17%) | 6 | **3 (−75%)** |
| Meteor Mite | 11 | 10 (−10%) | 6 | **4 (−64%)** |
| The Undertow | 12 | 11 (−8%) | 7 | **4 (−67%)** |
| Vesper | 14 | 12 (−13%) | 6 | **5 (−64%)** |
| All other 12 heroes | — | 0% to +15% | 3 to 7 | −20% to +31% |

**What this tells us:**
- **The misses came first.** In the update week, the four heroes still got almost their normal
  number of offers, but they missed about half of them. Their offers only crashed the *next*
  week. If the "closer heroes first" rule had pushed them out, their offers would have dropped
  straight away, not a week later.
- **There was a small early dip.** These four were the only heroes whose offers fell at all in
  the update week (−8% to −17%). The update went live on Wednesday 12 Aug, so that dip could be
  their misses starting to count mid-week. It could also be the distance rule nudging them down
  a little. Weekly numbers can't tell these apart; daily numbers could.
- **The "closer heroes first" change actually made a bad score matter *less*.** Before the
  update, a hero with the worst possible score needed to be about **40 minutes** closer than a
  hero with a perfect score to get the job. After the update, only about **19 minutes** closer.
  So the new distance rule didn't create the trap. The misses did.

**Answer (likely):** the **shorter answer time** was the trigger. It made these heroes miss lots
of offers, and the misses pushed them down the list. The distance rule may have added a small
push, and may be why Ashgrove and Halfmoon are slipping (see Check 2).

**Still unknown:** why these four missed so many more than everyone else. It wasn't a bad track
record; Vesper was one of the best at saying yes. It may be that they just take longer to reach
their phone. Dot's story of Vesper running down the stairs fits that. Answer-time data would show
it.

### Check 2: does the "60% rule" predict who got stuck?

**The rule, from the code:** every time a hero says yes, their score goes **up 0.08**. Every
time they miss or say no, it goes **down 0.12**. The score can't go above 1 or below 0, and it
never recovers on its own. So to stay level, a hero has to say yes to **at least 60%** of their
offers.

**Starting point:** before the update, every hero in the chart said yes to 60% or more every
single week. So by 10 Aug, every hero's score should have been at the top (1.00).

**What the scores would have done, week by week** (worked out from the chart):

| Hero | After 10 Aug | After 17 Aug | After 24 Aug | After 31 Aug |
|---|---|---|---|---|
| **The Undertow** | **0.48** | 0.20 | 0.08 | **0.00** |
| **Farlight** | **0.60** | 0.24 | 0.12 | **0.12** |
| **Meteor Mite** | **0.60** | 0.32 | 0.08 | **0.00** |
| **Vesper** | **0.76** | 0.36 | 0.12 | **0.00** |
| Sgt. Bulwark | 0.80 | 1.00 | 1.00 | 1.00 |
| Halfmoon | 0.88 | 0.80 | 0.84 | 1.00 |
| Nightwell | 0.88 | 1.00 | 1.00 | 1.00 |
| Ironvale, Stormwrack | 0.92 | 1.00 | 1.00 | 1.00 |
| The Drift | 0.96 | 1.00 | 1.00 | 1.00 |
| Everyone else | 1.00 | 1.00 | 1.00 (Longcast 0.92) | 1.00 |

**What this tells us:**
- **The rule picks out the right four, but only in one version of the maths.** If each week's
  yeses and misses are added up together, the four lowest scores belong to exactly the four
  heroes who got cut off.
  - **Correction:** this depends on an assumption. If a hero's misses happened *after* their
    yeses within the week, other heroes such as Nightwell would score lower than Vesper, and
    Nightwell didn't get cut off.
  - The rule shows how heroes got *stuck*. It doesn't prove *which* heroes would get stuck.
- **After that, they could never climb back.** They got so few offers that every miss dragged
  them lower, and they had no chance to earn points back. By 31 Aug three of them are at 0.
- **Everyone else bounced back.** Heroes who dipped a little, like Nightwell and Halfmoon, still
  got enough offers to recover to 1.00 within a week or three.
- **It was close.** Sgt. Bulwark scored 0.80, just above Vesper's 0.76, and he recovered. The
  difference is that he kept getting about 10 offers, while Vesper dropped to 5. So the score
  isn't the whole story. Where a hero sits compared with nearby heroes matters too.

**What the rule does *not* explain:** Corporal Ashgrove and Halfmoon. By this maths their scores
are back at 1.00, but they still get about 30% fewer offers than before. Something else is
pushing them down, most likely the **"closer heroes first" rule**, if they live further from the
emergencies than other heroes. We'd need to know where heroes are to check.

### What the two checks add up to

**Likely cause:** there are two causes working together.
1. **The shorter answer time made heroes miss more offers.** For four heroes, the misses were
   bad enough to push them below the 60% line.
2. **The score rule has no way back up.** Once a hero is low on the list, they rarely get
   offers, so they can't earn their score back. They stay stuck.

A third, smaller cause may be hurting Ashgrove and Halfmoon: the "closer heroes first" rule.

**What this means for the fix:** giving the stuck heroes a fresh score won't be enough on its
own, because the same trap would catch them again. The fix needs to:
- give heroes a way to climb back up
- treat "didn't answer in time" more gently than "said no", or give back the 90 seconds
- reset the heroes who are already stuck

**How sure are we?**
- **All four stuck heroes are backed up outside the chart.** The Undertow and Farlight appear in
  the tickets. Meteor Mite and Vesper appear in Kip's and Dot's interviews.
  - **Correction:** an earlier version said we were "less sure" about Meteor Mite and Vesper.
- **Caveats:** these checks use weekly totals, not every single offer, and assume all scores
  were at the top on 10 Aug. The chart's source is also still unknown.
- **To prove it:** Wen can confirm with the real scores, and by replaying August with the old
  and new settings.

## What five investigators concluded (28 Sep)

*In plain words.* Five investigators each tested one explanation, then read each other's findings,
argued about them and voted. **All five agreed on the basic answer**, with changes.
**Confidence: medium.** It's likely, but not proven.

### Who tested what

| Investigator | Explanation tested | Where they ended up |
|---|---|---|
| A | Shorter answer time (90 → 60 seconds) | The **trigger**, but not the whole story |
| B | The score rule (no way back up) | The **trap** that kept heroes stuck; a weakness that sat unnoticed until 4.2 |
| C | "Closer heroes first" | **Not the cause** (about 85% sure). It made the trap weaker. |
| D | Can we trust the evidence? | The chart is wrong or different for 7 heroes, but right for the four stuck ones. Late delivery is now about 35% likely. |
| E | The sceptic: is it even 4.2? | Not a quiet August. It *is* the update, and how it was released made it worse. |

### What they agreed on

**Likely root cause:**
1. **The update made heroes miss far more job offers.** Misses rose from about 1 in 5 offers to
   nearly 1 in 2 in the update week. The shorter answer time is the main suspect.
2. **An old rule in the code turned those misses into a trap.** The rule has three problems:
   - "Didn't answer in time" counts the same as "said no."
   - A miss costs more points than a yes earns.
   - There's no way to climb back up.

   It never mattered before, because nobody ever missed that many. After the update, the heroes
   who said yes least often sank to the bottom and stayed there.
3. **How it was released made it worse.** Two big changes went out at once, with no gradual
   rollout, no fairness checks and no kill switch.

**Ruled out:**
- **A quiet August.** Everything was steady for six weeks, then dropped the exact week of the
  update. The only comparison with last year points the other way.
- **Heroes' availability settings.** Handlers checked them, and nothing in the code changes them.
- **"Closer heroes first" as the trigger.** It made the trap weaker, not stronger. **Undoing it on
  its own would probably make things worse.**

### What they still disagree on

**1. Why *these* four heroes?**
- Other heroes, such as Nightwell and The Gale, missed just as many offers but didn't get stuck.
  The four said yes much less often: 4–6 times in the update week, against about 9.
- Sgt. Bulwark and Vesper both said yes to exactly half their offers. One got stuck and one
  didn't.
- Three ideas are still open:
  - **Location** (A and C): a stuck hero only gets skipped if other heroes are close enough to be
    asked first. So where a hero lives may decide who gets trapped.
  - **Late delivery** (D): some offers may reach phones late, leaving only seconds to answer. That
    fits tickets saying offers vanished "in a few seconds" (T-019, T-020, T-023, T-025), which a
    60-second window can't explain. The update also changed how offer notifications are sent
    (the "duplicate notification" fix).
  - **Can't tell from this data** (B and E).

**2. Can we trust the chart?**
- It's right for the four stuck heroes, because tickets or interviews back all four.
- It contradicts the tickets for 7 other heroes.
- It may be counting something different from what handlers count. For example, The Drift's
  handler says "two or three a week"; the chart says 6–7.

### Information needed to settle it

All five investigators asked for item 1 first.

| # | What to get | From | What it settles |
|---|---|---|---|
| 1 | **A log of every offer since 3 Aug**: when it was sent, when it reached the phone, when the hero answered, "no" vs "too late", and the hero's score and list position at that moment | Wen / Ravi | Slow answers (timeout) vs late delivery, and whether the four's scores really hit zero |
| 2 | **Replay August four ways**: old or new answer time × old or new "closer heroes first", plus one run with a "climb back up" fix | Wen | Which change caused it, and whether the fix works |
| 3 | **Official per-hero numbers**, and what the chart actually counts | Ravi | Whether the chart or the tickets are right for the 7 heroes |
| 4 | **What 4.2 changed about offer notifications** | Wen | Whether late delivery is part of the cause |
| 5 | **Travel times** for the stuck heroes vs the busy ones (numbers only, never identities) | Wen | Whether location decides who gets trapped |
| 6 | **Where scores are stored**, and restart dates | Wen | Whether restarts reset scores, and how to build the fix and kill switch |
| 7 | **Emergencies per week, unfilled jobs, June–July tickets, last year's August** | Ravi / Nadia | Whether less work or earlier problems play any part |

### What this means for the fix

- **Can go ahead in outline now:** let heroes climb back up, stop treating "too late" as harshly
  as "no", and reset the four stuck heroes.
- **Don't** undo "closer heroes first" on its own.
- **Wait for item 1** before choosing between 60 and 90 seconds. If late delivery is the real
  problem, the 90 seconds alone won't fix it.

## What the code tells us (2 Oct)

*In plain words.* We walked through `00-rook/code/dispatch-routing/` step by step, from an
emergency coming in to a hero's phone buzzing. Almost everything we saw in the data can be traced
to a specific step in the code.

### How a job gets to a phone

| Step | What happens | File |
|---|---|---|
| 1 | Emergency comes in | *Not in this folder* (the helpers' screen or another system) |
| 2 | Start finding someone | `offer.py` |
| 3 | Find who's free nearby; only the hero or helper can set this | `availability.py` |
| 4 | Score each hero: travel time 60%, says-yes-lately 25%, skills 15%. Over 45 minutes away gets zero travel points. | `routing.py` (with `availability.py`, `history.py`, `config.py`) |
| 5 | Put them in order. Nobody is removed, but the list stops at the first yes. | `routing.py` |
| 6 | Buzz the top person's phone | `offer.py` |
| 7 | Wait 60 seconds (was 90). Yes: score up 0.08. No or too late: score down 0.12, next person. | `offer.py`, `config.py`, `history.py` |

The update only changed three numbers in `config.py`: the answer time and two score weights.

### What we saw, and where it comes from

| What we saw in the data | Where it comes from in the code |
|---|---|
| Everyone missed more in the update week | **Step 7:** 60 seconds instead of 90 |
| A miss hurt more than a yes helped (the 60% line) | **Step 7:** "too late" counts as "no"; −0.12 per miss vs +0.08 per yes |
| Offers dried up a week later | **Steps 4–5:** the score fell first, then the hero slid down a list that stops at the first yes |
| The four never recovered | `history.py` has no way back up. The "should this ease back?" note has been open since 2019. |
| They missed almost every rare offer (3 of 25) | Unexpected buzz, then a miss, then the score drops again. Nothing breaks the loop. |
| Others got overloaded | **Step 5:** skipped heroes' work goes to whoever is next |
| The rate "recovered" but jobs taken are still down | Stuck heroes barely get buzzed, so their misses stop counting |
| "My phone never goes off" | Last on a list that stops at the first yes is the same as never being asked |
| Helpers can't explain it | Nothing shows a hero or helper their score |
| Ashgrove and Halfmoon slipping despite good scores | **Step 4:** travel time now counts for more, and over 45 minutes away gets zero |
| Why these four, not others | **Step 4:** a low score only matters if other free heroes are close enough to go first |

### New clue: the clock starts too early

In `offer.py`, the 60-second clock starts **when the offer is sent**, not when it reaches the
phone. If the offer arrives late, the hero gets less time. A 50-second delay leaves only 10
seconds.

That fits the help messages saying jobs vanished "in a few seconds" (T-019, T-020, T-023, T-025),
which a full 60 seconds can't explain. The update also changed how offer notifications are sent.

**Ask Wen:** does the real system start the clock at "sent" or at "received", and how late do
offers arrive? If the clock starts at "sent", the fix should start it when the phone receives
the offer, not just bring back 90 seconds.

### What the code can't explain

- **The 7 heroes where the chart and the help messages disagree.** A "yes" can only come from the
  phone, so those heroes really answered. Either the chart is wrong or memories are.
- **Complaints from before 12 Aug.** Nothing in the code changed then.
- **Whether offers arrive late.** The phone-sending part is only an outline in this folder.
- **Why scores never reset.** In this code, scores live only in memory, so a restart would wipe
  them. But nobody ever bounced back, so the real system probably saves them somewhere.

### What to do next

1. **Ask Wen this week:**
   - does the clock start at sent or at received?
   - a log of every offer (sent, arrived, answered)
   - where scores are saved
   - a replay of August
2. **Ask Ravi:** official numbers for Nightwell, Stormwrack and Ironvale, and what the chart
   counts.
3. **Tell the helpers now:** an honest message through Nadia.
4. **Plan the fix with Marcus and Wen** (a point release Helen approves):
   - let heroes climb back up
   - treat "too late" more gently than "no"
   - reset the four stuck heroes
   - start the clock when the phone gets the offer
5. **Don't:** undo "closer heroes first" on its own, or pick 60 vs 90 seconds before the offer log
   comes back.
6. **Before the fix ships:** agree the kill-switch measures. Then watch the four stuck heroes,
   Ashgrove and Halfmoon for a month.
7. **Still open:** flag the cracked vest plate to the Supply team.

## Hypotheses to test (2 Oct)

*In plain words.* Ten hypotheses from what we learned in the code, ranked by how much testing
each one would tell us about the **root cause**. Each is written the scientific way:
- what we saw
- the question
- a testable "If… then… because…"
- a "nothing's there" version (the null hypothesis)
- how to test it
- what would back it up or knock it down

### Ranking, and why #1 and #2 come first

| Rank | Hypothesis | How likely it's true | Why it's ranked here |
|---|---|---|---|
| **1** | **H2: Late delivery** | Medium | **Recommended:** it's the only one that can change the fix itself ("start the clock on arrival" vs "bring back 90 seconds"). |
| **2** | **H1: Shorter answer time** | High | **Recommended:** the miss surge is the first domino, and nothing else happens without it. |
| 3 | H3: Score trap | Very high | The trap only fires once misses surge, so it explains why heroes *stayed down*, not what *started* it. |
| 4 | H4: "No answer" = "no" | High | Tested by the same log as #1 and #2. It only matters once we know why answers came too late. |
| 5 | H6: Location | Medium | Explains *who* got hit, not *what* broke. |
| 6 | H9: What the chart counts | Medium–high | A trust check, not a cause. Ask Ravi at the same time. |
| 7 | H7: "Closer heroes first" | Medium | A side issue for two heroes. All five investigators said it isn't the main cause. |
| 8 | H5: Saved scores | High | Affects *how we fix it*, not *what caused it*. |
| 9 | H8: Manual overrides | Low | Nothing points to it yet. At most it adds to the story. |
| 10 | H10: Unfilled jobs | Unknown | Measures the damage, not the cause. Important for leadership. |

**Data that supports putting #1 and #2 first:**
- **The miss surge hit everyone at once, and offers didn't change.** In the update week, misses
  went from 38 a week to 81 (22% → 46%) while offers stayed level (172 → 177). All 16 heroes
  missed more.
- **Nothing else had moved before it.** The share of offers taken was flat at 75–78% for six
  weeks, then dropped to 54% in the update week. The four heroes' offers only fell the week
  *after* they missed.
- **The trap couldn't fire without the misses.** Before the update, no hero ever fell below the
  60% line. The trap rule has existed since at least 2019 and never caught anyone.
- **Some misses are too fast for 60 seconds.** T-019 ("gone by the time he'd even finished
  reading"), T-020 ("almost instantly"), T-023 ("before i could even swipe") and T-025 ("a few
  seconds") all fit late delivery.
- **The code and the update notes back #2.** The clock starts at "sent" (`offer.py`), and 4.2
  changed how notifications are sent ("duplicate push notification on re-offer").
- **Everyone points to the misses.**
  - interviews: 3 of 4
  - help messages: 9 of 25
  - investigators: all 5 named the miss surge, and 4 of them flagged late delivery
- **The answer changes the fix.** If #1 is true and #2 is false, bring back 90 seconds. If #2 is
  true, start the clock when the phone gets the offer.

### Table 1: what we saw and what we think

| Rank | Name | What we saw | Question | Hypothesis (If… then… because…) |
|---|---|---|---|---|
| **1** | **H2: Late delivery** | 4 messages say offers vanished in "a few seconds"; the clock starts when an offer is *sent*; 4.2 changed notifications | Are offers reaching some phones late? | **If** offers arrive late, **then** those heroes miss far more, **because** the clock is already running before the phone buzzes. |
| **2** | **H1: Shorter answer time** | Misses jumped from 22% to 46% in the update week, for all 16 heroes; offers stayed level (172 → 177) | Did cutting 90s to 60s cause most of the extra misses? | **If** heroes often answered in 60–90 seconds, **then** the cut turned those answers into misses, **because** they now came after the offer was pulled. |
| 3 | H3: Score trap | Four heroes fell to 0–1 offers a week and never recovered; no recovery rule in `history.py` | Does the score rule keep them at the bottom? | **If** a score falls low enough, **then** the hero stays stuck, **because** they're rarely asked, can't earn points back, and each rare miss lowers it again. |
| 4 | H4: "No answer" = "no" | The code records both separately but penalises both −0.12 | Did the four run out of time, or say no on purpose? | **If** most of their misses were "no answer", **then** a gentler penalty would have kept them out of the trap, **because** they'd never have fallen below the 60% line. |
| 5 | H6: Location | Vesper (50% yes) got stuck; Bulwark (50% yes) didn't | Does closeness to other free heroes decide who gets trapped? | **If** a low-scoring hero has nearby competitors, **then** they get skipped, **because** those heroes rank above them and the asking stops at the first yes. |
| 6 | H9: What the chart counts | For 7 heroes, the chart shows more offers but the messages say "quiet" | Does the chart count something different from what helpers count? | **If** the chart counts bulk sends or repeat offers, **then** it shows more offers than heroes notice, **because** one job sent to many counts once per hero. |
| 7 | H7: "Closer heroes first" | Ashgrove and Halfmoon get about 30% fewer offers despite good scores | Did the weighting change push them down? Would undoing it help the four? | **If** they're further from most jobs, **then** the higher travel weight lowered their rank, **because** travel now counts 60% instead of 45%. |
| 8 | H5: Saved scores | Outline code keeps scores in memory, yet no stuck hero bounced back | Are scores saved through restarts? | **If** scores are saved, **then** restarts won't reset them, **because** they're stored outside memory. |
| 9 | H8: Manual overrides | Helpers can override by hand, and overrides are logged (since 4.0) | Did helpers pass over the four by hand? | **If** helpers often picked others over the four, **then** the four lost extra offers, **because** overrides skipped them on purpose. |
| 10 | H10: Unfilled jobs | Jobs taken fell 9% (132 → 120 a week) | Were the missing jobs unfilled, or was there less work? | **If** emergencies stayed level, **then** the missing jobs went unfilled or late, **because** fewer heroes were able to take them. |

### Table 2: how we test it

| Rank | Null hypothesis (nothing's there) | Experiment | Supported if | Rejected if |
|---|---|---|---|---|
| **1** | Delivery time is the same for everyone and makes no difference | Offer log for 3–31 Aug. *Test:* delay from sent to arrived. *Measure:* answered or timed out. *Compare:* before/after 12 Aug, and the four vs the rest. Ask Wen whether the clock starts at "sent" or "arrived". | Misses cluster on long delays, and delays grew after 12 Aug or are worse for the four | Delivery takes a second or two for everyone, or the real clock starts on arrival |
| **2** | Almost no answers ever took over 60 seconds | (A) Share of pre-12 Aug yeses that took 60–90 seconds. (B) Wen replays August at 90 seconds with everything else the same. *Measure:* share missed. | A real share took 60–90 seconds, **and** the 90-second replay brings misses back near 22% | Almost all yeses were under 60 seconds, or the replay still shows the surge |
| 3 | The four's real scores are normal | Real weekly scores for the four, with Bulwark, Ashgrove and Halfmoon for comparison. Replay with a "drift back to the middle" rule. *Measure:* offers per week. | Scores sit near 0 from mid-August, **and** the recovery replay frees them | Scores are fine, or the four stay stuck even with recovery |
| 4 | The four mostly said "no" on purpose | Split their misses into "no" and "no answer". Replay with a smaller "no answer" penalty. *Measure:* do they get stuck? | Mostly "no answer", **and** the replay keeps them out of the trap | Mostly deliberate "no"s |
| 5 | Nearby competitors make no difference | Travel minutes for each offer vs other free heroes (minutes only, never addresses). *Measure:* offers per week. *Compare:* the four vs Bulwark. | The four usually had 2 or more nearby competitors; Bulwark didn't | The four were often closest and still skipped |
| 6 | The chart counts personal offers correctly | Ravi explains what "pings_sent" counts and pulls official numbers for Nightwell, Stormwrack and Ironvale | Official numbers are clearly lower than the chart | Official numbers match the chart |
| 7 | The weighting makes no difference | Travel times vs average. Replay with old weights at 60 seconds. *Measure:* offers for Ashgrove, Halfmoon and the four. | They're further than average; the old weights restore them but don't free the four | Average distance, or the old weights free the four |
| 8 | Scores reset to 0.5 on every restart | Restart dates and storage location from Wen. Check for jumps back to 0.5. | No jumps after restarts | Scores reset after restarts |
| 9 | Overrides were rare and didn't involve the four | Count overrides against the four in the override records, before vs after 12 Aug | Overrides against the four rose after 12 Aug | Few or none |
| 10 | Emergencies fell by about the same amount | Emergencies per week, unfilled jobs and time to assign, before vs after (Ravi) | Emergencies level; unfilled jobs or waits up | Emergencies fell about 9% |

### Which data tests which hypothesis

| Data source | From | Tests ranks |
|---|---|---|
| **Offer-by-offer log** (sent, arrived and answered times; "no" vs "no answer"; score; place in list; travel time) | Wen / Ravi | **1, 2, 3, 4, 5, 9** |
| **Replays of August**, changing one setting at a time | Wen | 2, 3, 4, 7 |
| **What the chart counts, plus official numbers** | Ravi | 6 |
| **Restart dates and where scores are saved** | Wen | 8 |
| **Override records** | Wen / Marcus | 9 |
| **Emergencies, unfilled jobs, wait times** | Ravi | 10 |

## Marcus's question: did the 4.2 change apply to responders who were already turning jobs down? (2 Oct)

*Marcus first asked this on 14 Aug, and it was never answered.*

**Short answer:** yes, it applied to everyone, including responders who'd already been turning
jobs down. The code has no "new vs existing" split anywhere. One caveat needs Wen to confirm.

**What the code shows:**
- **One setting for everybody.** The 4.2 change was two numbers in `config.py`: travel time went
  from 45% to 60%, and "says yes lately" from 40% to 25%. They're single values, with no
  exceptions, no list of who they apply to, and no start date.
- **The score is worked out fresh for every job.** Each time a job comes in, `routing.py` scores
  every free hero with those numbers. From the moment 4.2 went live, every hero was ranked under
  the new weights.
- **History carried over.** The "says yes lately" score in `history.py` wasn't touched by 4.2.
  Heroes who'd been turning jobs down kept their low score and were ranked with it under the new
  weights. Only brand-new heroes start fresh, at 0.5.

**The surprise:** for heroes who'd been turning jobs down, the change actually **helped**. Their
low score now counts for 25% instead of 40%, so it drags them down less. The weight change didn't
push decliners further down. What hurt them after 12 Aug was the shorter answer time adding new
misses on top.

**Caveats (don't guess on these):**
- **Did the 4.2 install reset scores?** In this version of the code, scores live only in memory.
  If the real system works the same way, restarting it for 4.2 would have reset every hero to
  0.5, wiping all history, good and bad. The data hints scores are saved (nobody ever bounced
  back), but only Wen can confirm.
- **Was it a deliberate choice?** There's no 4.2 decision record, so nothing says whether
  applying it to everyone was intended.

**Reply drafted for Marcus** (not sent):
> Checked the routing code: the 4.2 weight change applies to everyone, not just new responders.
> The weights are single global values in config.py, and every responder is re-scored on every
> callout, so anyone with a history of turning jobs down was ranked with their existing low score
> under the new weights from 12 Aug. For them it actually softened the penalty (acceptance history
> went from 40% to 25% of the score).
>
> Two things I can't confirm from this folder, so worth asking Wen: (1) whether the 4.2 deploy
> reset scores. In the sample code they live in memory and would go back to 0.5 on restart,
> though the data suggests the real system saves them. (2) Whether applying it to everyone was a
> deliberate decision. I can't find any record either way.

## Open questions (for Ravi and Wen)

*See the table "Information needed to settle it" above for the fuller list from the
investigation.*

1. Does the official per-responder data match this CSV, especially for the ~7 ticketed
   responders who show *more* pings here?
2. Incidents per week before and after 12 Aug, and unfilled callouts. Where did the missing
   ~12 taken pings a week go?
3. Actual recent-acceptance scores for the four cut-off responders, plus Ashgrove and Halfmoon.
   Do they confirm the 60% threshold?
4. How are the scores stored? If they're held in memory, a restart resets them, which changes
   the fix and the kill switch.
