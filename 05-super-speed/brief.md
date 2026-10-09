# A way back for quiet responders

**For:** Helen · **From and owner:** Dispatch PM · **5 Oct 2026, updated 8 Oct**

Today, one bad week can silence a responder for good, and nobody can see it happening. Here's what
we'd build instead, so Wen's 2019 question finally gets a real answer.

## What a responder who's gone quiet would feel

**Today:** the phone goes silent. When an offer finally comes, it's gone before they can swipe.
They wonder if their account is broken.

**With this:**
- **One bad week fades.** Running out of time costs less than saying no, and a low standing
  slowly recovers on its own.
- **You can see where you stand.** "3 offers this week · 1 taken · you're back in the rotation."
  No more guessing.
- **You can say "I'm coming."** One tap gives you a little more time while you get to your
  phone.

## What a handler like Kip would notice

**Today:** two cards side by side. Meteor Mite is dead quiet and The Gale is exhausted, and Kip
has nothing to tell either of them.

**With this:**
- **A plain note on the quiet card.** "Meteor Mite: no offers in 6 days. Missed 3 recent offers.
  Recovering." Kip can finally explain it.
- **A heads-up when an offer is live** for one of his responders.
- **The cards even out.** Mite gets offers again, and The Gale gets a breather.

## What we're starting with

| Order | What | Why first | Who builds it (proposed) |
|---|---|---|---|
| **1** | **A way back:** a gentler penalty for running out of time, recovery up to the middle, and a full fresh start for everyone stuck since 12 August | Frees the responders stuck today | Marcus's team, with Wen on the scoring |
| **2** | **"Why haven't I had offers?"** for responders, with a matching note for handlers | Ends "is my account broken?" (10 of 25 tickets), and works whatever the data shows | Sofia (design) and Marcus's team |
| **3** | **"I'm coming" button** | Tackles why responders miss offers, so this doesn't happen again | Sofia (design) and Marcus's team (mobile) |

**We'll know it worked when,** within 4 weeks of shipping:
- **no responder is stuck.** Today it's 4 of 16 in our sample.
- **"Is my account broken?" tickets are near zero.** Today it's 10 in 3 weeks.
- **jobs don't wait longer to get a responder** than before. This is the safety check.

**We tested it in a simulator.** A full fresh start frees everyone stuck today. But stopping it
happening again also needs item 3, or the answer-time fix, not just scoring changes. Recovery on
its own doesn't free anyone.

**Next, if you'd like it:** a fairness dashboard for you and Marcus, showing who's gone quiet,
who's swamped, and when to roll back.

## Options we weighed

| Option | Score (out of 30) |
|---|---|
| A way back | 24 |
| "Why haven't I had offers?" | 23 |
| Fairness dashboard | 19 |
| Support lookup tool | 19 |
| "I'm coming" button | 18 |
| Helper phone alert | 16 |
| "Test before you ship" replay | 15 |

## Click through

- **`prototype.html`:** Kip's console and Mite's phone, today vs the fix.
- **`recovery-simulator.html`:** try the scoring rules yourself.
