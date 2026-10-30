---
name: meeting-prep
description: Prepare for a meeting from Qale memory, what we know about the people and the customer, what changed since last time, open promises and open questions. Use when the user asks to prep for a call or a meeting with someone, or asks what they need to know before they meet a person or a customer.
allowed-tools: mcp__plugin_qale_qale__qale_status, mcp__plugin_qale_qale__qale_search, mcp__plugin_qale_qale__qale_read, mcp__plugin_qale_qale__qale_about
---

<!-- Based on apps/eval/bases/tavla-arrival@1/store/agents/meeting-prep/.shipped.md (the Qale app's meeting prep agent). Rewritten as a skill for the plugin tools: the prep goes in the chat, not on a meeting note. -->

# Prep for a meeting

## Read

1. Call `qale_status`. It names the user and the predecessor, if there is one.
2. Call `qale_about` for each person and customer in the meeting, in parallel. If a name matches several parties, take the newest and say so in the first line. If a person is not in memory, say so.
3. Find the last meeting with them: `qale_search` with `linkedTo` the party and `kind: ["meeting"]`. Call `qale_read` on it.
4. Call `qale_search` for decisions and change records since that meeting, by date, and read the ones that touch these people.

## Write

Write the prep in the chat. Keep it under a screen. Give every line its note id and date. Leave out a section that has nothing in it.

- **Since last time**: what changed that these people were not told. Compare their told records with the decisions and change records since the last meeting. Flag a decision they may know only in its old form.
- **Who owes what**: open and overdue commitments between us and them, with dates. Say "probably delivered, not linked" when a change record fits an open one.
- **Open questions**: their questions and our questions that no record answers.
- **What will come up**: their requests and dependencies, with who said them and how strong they are. Name a promise that a later decision runs against.
- **Qale thinks**: the judgments about them, each with its check. These are Qale's view, not facts.

If no told record names a person, write "Nothing recorded on what they were told." Never guess.

End with one coverage line from the facts the tools return.
