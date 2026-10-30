---
name: qale-setup
description: Set up Qale. Chooses the memory and inbox folders, stores the licence key and the user's name, writes the who-we-are note, and can add demo data. Use when the user runs /qale-setup or when a Qale tool returns licence_required.
allowed-tools: mcp__plugin_qale_qale__qale_setup, mcp__plugin_qale_qale__qale_status
---

# Qale setup

It is safe to run this more than once. It keeps what is already set up.

1. Call `qale_status`. If Qale is already set up, show the current folders, the licence state, who Qale thinks the user is and whether the who-we-are note exists, and use them as the defaults below.
2. Ask the user for four values in one question. Show the default for each:
   - the memory folder (default `~/Qale/Memory`)
   - the inbox folder (default `~/Qale/Inbox`)
   - the licence key (no default)
   - their full name, as people say it in meetings (no default). Qale uses it to know that "I" in a transcript is them. If a new person takes over from the current user, give the new person's name. Qale keeps the old user, and `qale_status` names them on a `Before you:` line.
3. Call `qale_setup` once with the values the user gave. Pass the name as `yourName`. Leave out a value the user did not change.
4. If `qale_setup` returns an error, show it and stop.
5. Ask the user if they want demo data. Tell them first what it does:
   - It copies two sample transcripts into the inbox folder.
   - It writes sample notes (people, decisions and commitments of a made-up company) into the memory folder. Qale adds these notes only if the memory folder is empty. If it already has notes, only the transcripts are copied.
   - It writes a who-we-are note for the made-up company, if the memory folder has none.
   - To keep the demo apart from real work, use a separate memory folder for it.

   If the user says yes, call `qale_setup` with `{ demo: true }`.
6. If the user said no to the demo, and `qale_status` in step 1 said there is no who-we-are note, ask for two values in one question:
   - the name of their company
   - what the company makes, in a few words

   Call `qale_setup` with them as `company` and `product`. Qale writes `who-we-are.md` in the memory folder, once. It never writes over the file, so a second run keeps the user's edits.
7. If a who-we-are note was written or exists, tell the user where it is. Ask them to open it and add the people on their team, with their roles, and what they call the people they build for. The analyst reads this note in every job to tell the user's side from the customer's side. It is plain text in their own words, under 2,000 characters.
8. Call `qale_status` and show the result.
9. Tell the user that Claude Code asks before each Qale tool call that the skills do not pre-approve. To stop these questions, the user can add `mcp__plugin_qale_qale` to `permissions.allow` in their Claude Code settings. A scheduled `/qale --unattended` run needs this, because nobody is there to answer.
10. Tell the user that the next step is to put a transcript (`.vtt`, `.srt`, `.txt`, `.md` or `.docx`) in the inbox folder and run `/qale`.
