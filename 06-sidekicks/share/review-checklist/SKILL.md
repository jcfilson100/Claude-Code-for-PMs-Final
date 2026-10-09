---
name: review-checklist
description: Review a product brief, one-pager, proposal or spec against four checks - names who owns it, says how we'll know it worked, keeps the same scope start to finish, and explains the problem before the fix. Use when the user points at a brief (a file or a folder of briefs) and asks to review, check, vet or audit it, or asks whether it's ready to share.
---

# Review checklist

Check a brief against **four** rules and report each one as **yes** or **NO: flagged**, with
short evidence from the brief itself.

**This skill is read-only.** Never edit the brief. Report only.

## What to review
- **One file:** review that file.
- **A folder:** review every brief in it (`.txt`, `.md`, `.docx`, `.pdf`), one after another.
- **Nothing given:** ask which brief or folder to review. Don't guess.

Read the **whole** brief before judging. Scope problems often appear halfway through.

## The four checks

### 1. Owner named
**Passes if** a specific person or team is named as responsible for delivering it.
For example: "The payments team builds it", or "Owner: Jordan".

**Fails if:**
- there's no owner at all
- it's left for the future ("for whoever picks it up next quarter", "nobody's picked it up yet")
- the owner is too vague to act on ("Product", "the business", "TBD")

Flagging it for a future owner isn't the same as naming one.

### 2. Success measure
**Passes if** it says how we'll know it worked: something observable that changes, so someone
could check it. For example: "fewer support tickets about X", or "checkout time drops below 30
seconds". A measure without a baseline still passes, but note "no baseline yet".

**Fails if:**
- there's no measure
- it only describes what gets built or logged, not how anyone would know it's working
- the "measure" is the feature existing ("the dashboard is live")

### 3. Scope stays bounded
**Passes if** what's in at the start is still what's in at the end. Saying clearly what's *out*
is a plus.

**Fails if** the scope grows partway through. Look for:
- "while we're building", "and also", "and honestly", "if we're doing all that"
- "this could eventually…"
- new features stacked onto the original ask
- the stated ask ("that's the whole ask") contradicted later

Quote the point where it grows, and list what got added.

### 4. Problem before fix
**Passes if** the problem (who is affected, what goes wrong, why it matters) comes **before** the
proposal, in reading order.

**Fails if:**
- it opens with the solution and explains the problem later
- it never states the problem, only the solution's benefits

Name the section where the problem first appears.

## How to judge
- **Use the brief's own words as evidence.** A short quote or a precise description.
- **Be strict but fair.** A heading alone ("Owner") with nothing useful under it fails. An owner
  named in a sentence without a heading passes.
- **One brief can fail several checks.** Report every one.
- **Don't judge whether the idea is good.** Only these four checks. If you notice something
  serious outside them, add one line under "Also noticed" and keep it separate from the result.

## Output format (always this shape)

    review-checklist: <file or folder>
    Reviewed: <date>

    <file name>
      owner named ................... yes (<who>) | NO: flagged
      success measure ............... yes | NO: flagged
      scope stays bounded ........... yes | NO: flagged
      problem stated before fix ..... yes | NO: flagged
      -> <N> flag(s): <one or two plain sentences of evidence for each flag>

    ...one block per brief...

    <N> briefs checked, <N> flagged, <N> flags total.

After the block, add a short **"What to fix"** list: one plain line per flag, saying what the
author should add or move. For example: "Name an owner for the build, not 'whoever picks it up
next quarter'."

Use plain, short sentences. Keep each flag's evidence to one or two lines.
