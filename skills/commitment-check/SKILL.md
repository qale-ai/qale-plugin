---
name: commitment-check
description: Work out what to do about one promise in Qale memory, a commitment that is overdue or slipping, or a request that just came in. Use when the user asks what to do about an overdue promise, says they still owe someone something, asks if a commitment can be closed, or pastes a message that asks for something and wants to know how to answer.
allowed-tools: mcp__plugin_qale_qale__qale_status, mcp__plugin_qale_qale__qale_search, mcp__plugin_qale_qale__qale_read, mcp__plugin_qale_qale__qale_about, mcp__plugin_qale_qale__qale_correct, mcp__plugin_qale_qale__qale_ingest
---

<!-- Ported from vault-dev/skills/commitment-check/SKILL.md @ 0706e1ed (the Qale app). Rewritten for the plugin tools: no plan sections on the note, no findings written, drafts go in the chat. -->

# Handle a commitment

One promise, in one of two directions. Decide which first:

- **A commitment of the user's**: they point at one, usually because it slipped. Work on that one only.
- **A request that came in**: a pasted message, a forwarded email, an ask from above.

## Read, for both

1. Call `qale_status`. It names the user.
2. Call `qale_about` for each person and customer involved. It gives what they own and are owed, what they were told, their requests and their decisions. If a sender is not in memory, say so. Never guess their position.
3. Call `qale_search` on the subject, in English and in Swedish, with `kind: ["change", "told", "dependency"]`, and once with no kind.
4. Call `qale_read` on each note you use. Before you quote, read the source lines around the quote.

## A commitment of the user's

Also read the commitment (due date, owner, sources) and the meeting where it was made. Look for:

- a change record that fits it, which means it probably happened but is not linked;
- a later meeting with the person it was made to.

Then recommend one of these, with the notes it rests on:

- **Plan** (the default): two to four next steps, from memory.
- **Close**: only when memory shows that it happened or no longer matters. If the user confirms, call `qale_correct` with `{ action: "close", commitmentId, state: "done" or "dropped", what }`. If you worked it out and nobody said it, ask the user first.
- **Move the date**: only with a real reason for the new date. Never move a date just to clear the overdue status. Tell the user to change `due` in the note; `qale_status` gives the memory folder.
- **Raise it in the next meeting**: if the person is in a meeting soon, give the line to say.
- **Nudge**: if it waits on someone, draft a short message in the chat that names where the promise was made.

If the user says they told the person, call `qale_correct` with `{ action: "told", to, about, relatedTo }`.

## A request that came in

1. Read the message closely: what it asks for, by when, and what it only implies.
2. Work out the job behind the ask. "Can we add a CSV export button" is the request. "Finance rebuilds that report by hand every month" is the job. The job is a guess: say what would confirm it.
3. Check memory for the same ask from others (`qale_search` with `kind: ["feedback"]`), for a decision it runs into, and for an earlier promise on it.
4. Recommend yes, no or not yet, with the decision or the notes it rests on. If a reply has to go now, draft it in the chat.
5. Ask the user if they want the message in memory. If yes, call `qale_ingest` with `{ text, title, kind, date, origin }`, then process it as the `qale` skill says. Never put it in memory without asking.

Give every point its source, its date and who said it. A judgment record is Qale's view: put "Qale thinks" in front of it.
