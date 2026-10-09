# review-checklist: a Claude Code skill

Point it at a brief and it checks four things:

1. **Owner named:** a real person or team is responsible.
2. **Success measure:** it says how we'll know it worked.
3. **Scope stays bounded:** it doesn't grow partway through.
4. **Problem before fix:** it explains the problem before the solution.

It reports **yes** or **flagged** for each check, quotes the brief as evidence, and ends with a
short "What to fix" list. **It never edits your brief.**

## Install (about 1 minute)

Put the `review-checklist` folder (the one containing `SKILL.md`) in **one** of these places.

| Use it… | Put the folder here |
|---|---|
| **In one project only** (recommended to start) | `<your project>/.claude/skills/review-checklist/` |
| **In all your projects** | Windows: `C:\Users\<you>\.claude\skills\review-checklist\` · Mac/Linux: `~/.claude/skills/review-checklist/` |

Then start a new Claude Code session in that project, or restart the one you're in.

## Use it

Type any of these:

- `/review-checklist` (always works)
- "Review `docs/brief.md`"
- "Check the briefs in `docs/briefs/`"
- "Vet this one-pager before I send it"
- "Is this proposal ready to share?"

It works on `.md`, `.txt`, `.docx` and `.pdf` files, one at a time or a whole folder.

## Example output

```
review-checklist: briefs/
Reviewed: 8 Oct 2026

phone-app.txt
  owner named ................... yes (Design team)
  success measure ............... yes (no baseline yet)
  scope stays bounded ........... yes
  problem stated before fix ..... NO: flagged
  -> 1 flag: opens with "We should build a phone app"; the problem only
     appears in the second section.

1 brief checked, 1 flagged, 1 flag total.

What to fix
- Move the problem ("handlers find out late") above the proposal.
```

## Change the rules

Open `SKILL.md` and edit it in plain English. For example, add a fifth check ("names the
deadline"), or change what counts as a pass. The skill follows whatever the file says.

## Good to know
- It only checks that an owner is **named**, not that they've agreed.
- It checks that a success measure **exists**, not whether it's a good one.
- It doesn't judge whether the idea is good. That's your call.
