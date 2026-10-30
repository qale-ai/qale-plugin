---
name: qale
description: Answer questions from the user's product memory (meetings, customers, decisions, promises), process meeting transcripts from the Qale inbox into that memory, add articles, blog posts and videos to it, and link tickets for commitments. Use for any question about what was decided, promised or said about a person, customer, team or topic, for example who needs to know about a change, what we promised a customer and what is overdue, what shipped that affects a team or customer, who asked for a feature and what we told them, why a decision was made, or what a predecessor promised. Also use when the user runs /qale, asks to process, add, review, retry or undo transcripts, asks for Qale status, or corrects memory (a promise kept or dropped, who was told, two names for one party).
argument-hint: "[add <path> | review | status | retry <itemId> | undo <batchId> | --unattended]"
allowed-tools: mcp__plugin_qale_qale__qale_next, mcp__plugin_qale_qale__qale_review, mcp__plugin_qale_qale__qale_status, mcp__plugin_qale_qale__qale_about, mcp__plugin_qale_qale__qale_ingest, mcp__plugin_qale_qale__qale_search, mcp__plugin_qale_qale__qale_read, mcp__plugin_qale_qale__qale_link_ticket, Agent
---

# Qale

The user ran `/qale $ARGUMENTS`. Pick the form that matches the arguments.

If the user asked a question about what memory knows, go to "Answer a question from memory", even when there are no arguments. Process the inbox only when the user ran `/qale` with no arguments and no question, or asked for it.

If a tool returns `licence_required`, tell the user to run `/qale-setup`, and stop.

## No arguments: process the inbox

1. Call `qale_next`. It sweeps the inbox and claims up to 2 jobs. If it returns no jobs, go to step 4.
2. For each job id, start one `qale:qale-analyst` subagent. Start all of them in one message, so that they run in parallel. Give each one only its job id, for example `Job id: job-mh2k3x9a-4f1c2b7e`. Do not read the transcripts yourself.

   If you cannot start that subagent (some apps have none), do the jobs yourself, one at a time: call `qale_job` with the job id, follow its instructions exactly, and call `qale_submit` once with the extraction. The customer-meeting skill tells you how to read a customer meeting.
3. Wait for all subagents. Report one line per job: the meeting title, the counts by type, and any failure. Never fix what a job wrote by editing a memory file yourself. If a fact looks wrong (a size, a stage, a name), tell the user; the fix goes through `qale_correct` or an edit the user asked for (see "Correct memory").
4. Call `qale_review` and show the user what changed in memory.
5. If the review lists commitments that need a ticket, ask the user which ones they want as tickets. Qale does not write to Jira. For each one the user picks:
   1. Call `qale_read` with its id, for the title, the owner, the due date and the quotes.
   2. Create the ticket with the user's own Jira tool (the Atlassian connector in their Claude). That tool asks the user to approve the call. Use the title as the summary, and the text and quotes as the description.
   3. Call `qale_link_ticket` with the commitment id and the new ticket key.

   If no Jira tool is available, tell the user that Qale cannot make tickets itself and that they can connect Atlassian in Claude. List the commitments, so that they can make the tickets by hand and give you the keys to link.

Memory changes are already written when a job ends. They do not need approval. The user can undo a batch with `/qale undo <batchId>`, and take back a linked ticket with git, like any other edit.

## Other forms

- `add <path>`: call `qale_ingest` with `{ path }`. The server needs a full path. If the path does not start with `/` or `~`, join it to the current working folder first (for example `notes/call.vtt` in `/Users/anna/work` becomes `/Users/anna/work/notes/call.vtt`). Then process the inbox as above.
- `add` with pasted text: if the text is 20,000 characters or less, call `qale_ingest` with `{ text, title }`. If the text is longer, ask the user to save it as a file or to put it in the inbox folder.
- Material you pulled through a connector (an email, a Slack or Teams thread, a ticket, an article): call `qale_ingest` with `{ text, title, kind, date, origin }`. `kind` is what it is (email, thread, ticket, article, video, chat, notes or transcript). `date` is the day the material is from, not today. `origin` is where it came from, in a few words, for example `"Slack #payments"` or `"mail from Sofia Dahl"`. Leave out what you do not know: Qale reads the kind from the text, and dates text with no date today.
- An article, a blog post or a video (a YouTube transcript, a podcast):
  1. Search the title first: `qale_search` with the title and `kind: ["article", "video"]`. If it is there, ask the user before you ingest it again.
  2. Give the full text as it was published, never a summary. Qale checks every quote against the stored text, so a summary would let made-up words pass. If the tool that fetched the page gave you only a summary, ask the user to paste the page or save it as a file.
  3. If the text is over 20,000 characters, save it as a file and call `qale_ingest` with `{ path, kind, date, origin }`.
  4. `kind` is `article` or `video`. `date` is the day it was published, not today. `origin` is the publisher, the author and the URL, for example `"Example Blog, by Jane Doe, https://example.com/pricing-pages"`.
- `review`: do steps 4 and 5.
- `status`: call `qale_status` and show the result. If it lists failed or undone items, tell the user that `/qale retry <itemId>` runs one again. The item id is at the end of the item's line, after the word `item`: `- Prissättning (item item-mh2k3x9a-4f1c2b7e)`.
- `retry <itemId>` (one or more ids of failed or undone items): call `qale_next` with `{ retry: [<itemId>, ...] }`. Those items go back to the queue and are claimed first. Then do steps 2 to 5 above. Retry only the items the user named.
- `undo <batchId>`: call `qale_undo` with the batch id. Claude Code asks the user before it runs, because undo changes memory. Show which memory changes went back and the last line of the reply, which gives the command to run the transcript again (`/qale retry <itemId>`).

## Answer a question from memory

Use this when the user asks what memory knows, with or without `/qale`. Each step has one example.

### Find

1. Call `qale_status` first, every time. `You are parties/...` is the user ("what I owe"). `Before you: parties/...` is the predecessor. If the line is missing, ask the user. Never guess the predecessor from who owns the most commitments.
2. Call `qale_about` for every party the question names. If a name matches several parties, take the newest, never the user's own party, and say so in the first line: "Ida Lind, Brightline: she is in the newest source, 2026-10-23." Name the others at the end: "Also in memory: Ida Wall, Wiklund & Co." Ask first only when two share the newest date.
3. Call `qale_search` by words, in English and in Swedish, with synonyms and aliases. For example, "bulk schedule editing" is also searched as "mass-edit", "massredigering", "ändra många scheman" and "bulk edit". Also search a feature's distinctive word alone, for example "export": "CSV export" puts the notes with both words first, and "schemalagd export" can fall below the limit. For who depends on or asked for a feature, add `kind: ["dependency"]` or `kind: ["feedback"]`. Run the searches in parallel.
4. Call `qale_read` on each note you use. Before you quote, read the source lines around the quote: `qale_read` with `{ id: "sources/...", lines: "L0408-L0416" }` for `L0412`.

### Check

- If a later change fits an open, overdue or unknown commitment, say "probably delivered, not linked", never closed. Search the commitment's own words with `kind: ["change"]` and no `linkedTo`, for example "SCIM": a change often links to the person who reported it, not the customer. Offer to close it with `qale_correct` if the user confirms.
- For every promise that looks delivered, closed ones too, say "no record they were told" when no told record names the party it was made to.
- Two parties are two people. Never write "Ida (Lind/Wall)". Name each with their company.

### Answer

- A record of kind `article` or `video` is outside material. Never count it as a customer, and never count what it says as something a customer said. If you use it, name the author or the speaker and the published date.
- Give every point its source, its date and who said it. A tentative commitment is "Nils said he would probably set up a test environment", never a promise.
- A judgment (the "Qale thinks" section of `qale_about`, or `kind: ["judgment"]` in `qale_search`) is Qale's view, not a fact. Put "Qale thinks" in front and give its check: "Qale thinks Lars takes the 15% prepay discount as settled. Check: ask Karin if the discount is approved before his CFO closes the budget." Never state it as a fact, and never count it as a request, a promise or a decision.
- If a decision has no reason, write "No reason recorded." and name the nearest related source as a lead: "Lead, not a reason: the product sync on 2026-10-07 starts on mobile research."
- After "who needs to know", offer to record who was told, with `qale_correct` `told`.
- End every answer with one coverage line from the facts the tools return: "Searched commitments and change records for Northwind and "Northwind Bank", 2026-03-01 to 2026-10-24, 14 sources (all transcripts); no source in 2026-08, and no release notes are captured." An answer without it is not finished.

## Correct memory

When the user corrects memory, change it on their word. Only on their word: never edit a memory file because you think a job got something wrong. Say what looks wrong and let the user decide.

- One field on a note: edit the memory file with the Edit tool. The file is `<memory folder>/<note id>.md`; `qale_status` gives the memory folder. The fields are `status: done` or `status: dropped` on a commitment, `firmness`, `due`, `owner`, and `aliases` on a party. If a note was never a claim, tell the user the file to delete (`<memory folder>/<id>.md`). If they prefer, set `needs_review: true` and add a line "Not a claim, per the user on <date>" with Edit; Qale commits the edit. Qale commits a hand edit on its own before its next write and reindexes it, so no tool call is needed.
- "That promise was kept" or "we dropped it": call `qale_correct` with `{ action: "close", commitmentId, state: "done" or "dropped", what }`. `what` is what the user said happened, if they said it. Qale writes a change record dated today that closes the commitment.
- "I told them": call `qale_correct` with `{ action: "told", to: [party ids], about, relatedTo }`. `relatedTo` is the commitment or decision it was about, if there is one.
- "These two are the same person" (or customer): call `qale_correct` with `{ action: "same_party", keep, remove }`. Keep the party with more links. Qale moves every link to the kept party and deletes the other note.

Claude Code asks the user before `qale_correct` runs. Each correction is one commit; the reply gives the `git revert` command that takes it back.

## `--unattended`

Nobody is watching. Do steps 1 to 3 only. Never make a ticket, never ask the user a question, and never edit a memory file. A hand edit is outside the batch, so `/qale undo` cannot take it back and the receipt does not show it. If a job wrote something wrong, say so in your final answer, with the note id and what is wrong, and leave the file as it is.
