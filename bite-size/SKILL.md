---
name: bite-size
description: Walk the user through a list of changes or decisions one item at a time instead of dumping them all at once. Use this automatically whenever your response would otherwise present roughly 4 or more items that each need the user's decision, approval, or edit — code review findings, checklist fixes, README/doc updates, refactor steps, migration steps, lint/audit results to act on, proposed edits across files. Also use when the user types /bite-size or says the output is overwhelming, "one at a time", "walk me through", or "step by step". Do NOT use for purely informational lists (explanations, summaries, search results, options the user only needs to read) or for lists of 3 or fewer items.
---

# Bite-size

Present actionable items one at a time so the user is never overwhelmed. Short beats thorough here.

## Start

1. Gather and order all items silently (dependencies first, then by importance or file order).
2. Group intertwined items (see Grouping). Count the resulting steps.
3. First message: one line with the count, then item 1. No overview, no full list up front.
   - e.g. "I found 10 changes. Here's the first."

## Grouping

Present intertwined items together as one step when:
- one decision changes or invalidates another,
- they touch the same lines or the same block, or
- they are parts of the same logical change (e.g. rename + its call sites).

Say why in one short line: "Grouped: both edit the install section."

## Each item

Use this shape, kept tight:

- **Header:** `Change 3 of 10: <title>` (group: `Changes 3–4 of 10: <title>`)
- **Where:** clickable `path:line` references.
- **Current / Proposed:** small code or text blocks. Show only the relevant lines.
- **Why:** one or two lines.
- **Recommendation:** only when there are real options; name your pick and why in one line.
- **Up next:** one-line titles of the next 2–3 remaining items.
- **Ask:** end with "Apply, modify, or skip?"

## Acting on the decision

- **Apply:** make the edit right away, confirm in one line, then present the next item in the same message.
- **Modify:** apply the user's version (ask a single clarifying question only if truly ambiguous), then move on.
- **Skip:** note it and move on.
- **Packets:** if several consecutive items together form one commit-sized logical change, collect the decisions and apply them together once the packet is complete. Tell the user: "Holding this until 5–6 are decided, they're one change."

## Commits

- NEVER `git add`, commit, or push.
- At each logical-packet boundary, suggest a conventional, concise commit message plus the files involved, and say the user can commit when ready:

  > Packet done. Suggested commit (files: `README.md`, `docs/setup.md`):
  > `docs: fix install steps and Node version`
  > Commit when ready.

## Tangents and resuming

- Track position as "item N of M" with the list of remaining items.
- If the user digresses, handle the tangent fully, then offer: "We're paused at change 4 of 10 (<title>). Resume?"
- If items change because of the tangent (new ones found, some made moot), update the count and say so in one line.

## Finish

One line: "Done, all 10 items handled (7 applied, 2 skipped, 1 modified)." Include a final commit suggestion if a packet is still uncommitted. No long recap.

## Example item

````markdown
**Change 3 of 10: Fix outdated Node version in setup**

[README.md:42](README.md:42)

Current:
```
Requires Node 16+
```
Proposed:
```
Requires Node 20+
```

Why: `package.json` engines field requires `>=20`; Node 16 fails at install.

Up next: Remove dead link to wiki · Add `.env` example · Update test command

Apply, modify, or skip?
````
