# 04 · X-Ray Vision — prompts

**Context:** You joined Rook two weeks ago as PM on Dispatch.
Release 4.2 shipped on 12 August, just before you arrived, and
landed badly. Last session the numbers showed you what happened: a
ping used to wait ninety seconds and now it waits sixty, people
missed pings they used to catch, and missing one counts the same as
turning one down — so four responders stopped hearing from us
altogether.

At the end of the session, ask Claude Code to save the prompts you
wrote yourself below — not the starter prompt. The closing slide has
the exact prompt to paste. By Module 6 this file is a prompt library
built from your own questions.

---

### 1.

How do you think this could infer what we saw from the data analysis conducted so far?

### 2.

add this to the synthesis file, but before you do so, give me the quick tl:dr on what to do next?

### 3.

Do we have real location and travel time data?

### 4.

Do we have any information in the repo that can help us

### 5.

Give me the tour of the .py files you mentioned

### 6.

based on what we know after looking at the code, help me create hypothesis statements that we should test?

### 7.

Rank the hypothesis based on how likely we are able to identify the root cause.

### 8.

Put this into a table. Add another column that will give me a brief 1 sentence explanation on why we recommend #1 and #2 over the others. give me some data that will support the recommendation

### 9.

Rewrite the hypothesis statements using scientific method framing

### 10.

Put it in the table format

### 11.

Fill in the following sentence:
Based on what I found, the reason some responders are getting no pings at all is ____, because _____.

### 12.

Marcus our engineering manager is asking: Quick one, did the change to who gets asked first apply to responders who'd already been turning jobs down, or just new ones? Can't tell from the code and don't want to guess on this. Anyone know?

### 13.

What hypothesis would otherwise confirm this?

### 14.

Yes, add to the synthesis and PR #4, redraft the slack message to marcus with this info

### 15.

Playing the role of Marcus, the engineering manager, what follow up questions will we potentially get based on our response and how can we prepare?

### 16.

Can we confirm nothing changed in the code pre 4.2? is it really just configs?

### 17.

From your point of view, what should we do next?

### 18.

Add the fourth question to the Marcus Slack draft

### 19.

Post the message to marcus in this slack channel: https://product-school.slack.com/archives/C0B8LTV13EJ

### 20.

Find me the part of this code that takes points off somebody when they miss a ping or turn one down. Show it to me and explain it in plain English. Then find me every single thing in this code that puts points back on.

### 21.

According to my findings in the code, for someone who's gone quiet, they would need to ___. Fill in the blank

### 22.

Paste this whole message to the slack channel
