---
name: synthesis
description: Read across many meetings and sources in Qale memory and say what they add up to. Use when the user asks what a set of interviews or customer calls shows, how many customers asked for the same thing, who else wants a feature, or what memory holds on a theme.
allowed-tools: mcp__plugin_qale_qale__qale_status, mcp__plugin_qale_qale__qale_search, mcp__plugin_qale_qale__qale_read, mcp__plugin_qale_qale__qale_about
---

<!-- Ported from vault-dev/skills/synthesis/SKILL.md @ 2501e401 (the Qale app). Rewritten for the plugin tools: the answer goes in the chat, and no findings, topic pages or tickets are written. -->

# Find the pattern

The user names a body of material and asks what it adds up to. The question can be pointed ("who wants scheduled exports?") or open ("what do the onboarding calls show?").

## Read

1. Call `qale_status`. It names the user and the memory folder.
2. Find the material in scope with `qale_search`: by words in English and in Swedish, with synonyms, by a party (`linkedTo`), by dates, and with `kind: ["feedback", "dependency", "question", "risk"]`. Run the searches in parallel.
3. If fewer than two meetings or sources are in scope, say there is no pattern in one source, and stop.
4. Call `qale_read` on each record you use, then on the source lines it points to. A record is a summary. Quote only words you read in a source.
5. Read the decisions that touch the subject.

## Group

Group what you read by what it claims, not by meeting. For each group, count the parties, not the records: one customer who says it three times is one account.

- Feedback says who said it and how strong it is. Weigh "said by the customer" above "reported", and "strong" above "weak". Never count "our idea" as a request.
- A record of kind `article` or `video` is outside material. Never count it as a customer.
- A judgment record is Qale's view, not a fact. Never count it as a request.

## Answer

Answer the user's question in the chat:

1. Lead with the answer in two or three sentences.
2. One short section per claim: the claim in plain words, how many accounts say it (and which), and one or two exact quotes with their source and date.
3. Say when a claim rests on one account, and what second account would confirm it.
4. Say where the material disagrees, and name a decision that the evidence runs against.
5. End with what you looked for and did not find, and one coverage line from the facts the search returns: the dates, the number of sources and their kinds.

Qale writes nothing here. If the user wants the result kept, they save it where they like.
