---
name: weekly-update
description: Draft the weekly update from Qale memory, what was decided, shipped, promised and changed this week, for the people who were not in the meetings. Use when the user asks for the weekly update, the Friday update, what to tell the company or the team about this week, or a test run over last week.
allowed-tools: mcp__plugin_qale_qale__qale_status, mcp__plugin_qale_qale__qale_search, mcp__plugin_qale_qale__qale_read, mcp__plugin_qale_qale__qale_about
---

<!-- Ported from vault-dev/skills/weekly-update/SKILL.md @ 1939b3bd (the Qale app). Rewritten for the plugin tools; voices, draft panels and Confluence drafts are cut. -->

# Write the weekly update

## List the week first

Write nothing until this list exists. The draft comes from it and from nothing else.

1. Call `qale_status`. It names the user and the memory folder.
2. The week is Monday to today. For "do last week as a test", it is the Monday to Sunday before this Monday.
3. Call `qale_search` with no query, `from` and `to` set to the week, once for each of these, in parallel:
   - `types: ["decision"]`
   - `types: ["record"]` with `kind: ["change", "told"]`
   - `types: ["record"]` with `kind: ["risk", "dependency"]`
   - `types: ["commitment"]`
4. Also call `qale_search` with `types: ["commitment"]` and `status: ["overdue"]`, with no dates. These are the promises that slipped.
5. Call `qale_read` on a note if its row does not say what changed. Leave out a note that changed nothing a reader needs.

## Write the draft

Write it in the chat, in two versions: **Full** and **Short**. Use the shape below.

- Write only from the list. Copy every name, date and ticket key from a note. Never work out a date.
- Never say something shipped, slipped or is blocked without a note on the list behind it.
- The "Watch" line can name an open or overdue commitment from step 4. Copy its date from the note.
- If a line has nothing behind it, write "nothing this week" and keep the line.
- Give each line the id of the note it comes from, in brackets, so that the user can check it.
- A judgment record is Qale's view, not a fact. Leave it out of the update.

```
Full
Decided: <what was decided this week, by whom, and the date> [decisions/...]
Changed: <a date that moved, a promise kept or dropped, and who was told> [records/...]
Promised: <new commitments, to whom, and by when> [commitments/...]
Watch: <the one thing that could go wrong next, with its date> [...]

Short
<the one line that matters, with its date or number> [...]
```

End with one line on what the week's list covered, from the coverage line the search returns: the number of sources this week, and their kinds. If the week has no source, say so and do not write a draft.

The user sends the update. Qale does not send anything.
