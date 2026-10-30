# Qale

Qale is a product memory for product managers. It reads your meeting
transcripts and keeps what was decided, promised, asked for and changed, so
that you can ask about it later: what did we promise this customer, who
needs to know about this change, what do the customer calls add up to.

Qale runs on your computer. Your memory is a folder of markdown files on
your disk.

## What you need

- One of these apps on a Mac or a Windows PC:
  - the Claude desktop app (Cowork) or Claude Code;
  - the ChatGPT desktop app (Codex) or the Codex CLI.
- Node.js 22.13 or later. Check with `node --version` in a terminal. If you
  do not have it, install the LTS version from https://nodejs.org.
- A Qale licence key.

Qale runs on your computer, so it does not work in Claude or ChatGPT on the
web or on a phone, or in Codex cloud tasks.

## Install in the Claude desktop app (Cowork)

1. Open the Claude desktop app and select the **Cowork** tab at the top.
2. Select **Customize** in the left sidebar, then **Browse plugins**.
3. Select **Personal**, then **+**, then **Add marketplace from GitHub**.
4. Enter `qale-ai/qale-plugin` and confirm.
5. Find **Qale** in the list and select **Install**.
6. Start a new Cowork session and type `/qale-setup`.

## Install in the ChatGPT desktop app (Codex)

The ChatGPT desktop app reads its plugin marketplaces from the Codex
settings on your computer. You add the Qale marketplace once, from a
terminal:

1. Open a terminal and run:

   ```sh
   npx -y @openai/codex plugin marketplace add qale-ai/qale-plugin
   ```

   If you have the Codex CLI, `codex plugin marketplace add
   qale-ai/qale-plugin` does the same.
2. Quit the ChatGPT desktop app and open it again.
3. Open Codex in the app, then the **Plugins** directory.
4. Choose the **Qale** marketplace, find **Qale** and install it.
5. Start a new Codex chat and write "Set up Qale".

## Install in Claude Code

```
/plugin marketplace add qale-ai/qale-plugin
/plugin install qale@qale
/qale-setup
```

## Install in the Codex CLI

```sh
codex plugin marketplace add qale-ai/qale-plugin
codex plugin add qale@qale
```

Then start Codex and write "Set up Qale".

## Set up

Setup asks for your memory folder, your inbox folder, your licence key and
your name. The memory folder is where Qale keeps what it learns. The inbox
folder is where you put transcripts.

## If the Qale tools do not appear

The app could not start Qale. The usual reason is that it cannot find
Node.js. A desktop app does not always see the same programs as your
terminal, for example a Node.js from Homebrew. Install Node.js from
https://nodejs.org, quit the app and open it again.

## Your licence key

Qale needs a licence key. It is free during the beta. You get it with your
invitation. If you have none, write to hello@qale.ai.

1. Run `/qale-setup` (in Codex or ChatGPT, write "Set up Qale").
2. Give the key when it asks.
3. The reply says `Licence: valid`. Run `/qale status` to see the last day
   the key works.

Qale checks the key on your computer and sends it nowhere. It saves the key
in `~/.qale/config.json`.

To change the key, run setup again and give the new one. If Qale tells you
to run setup, your key is missing or has expired.

## Use

- Put transcripts (`.vtt`, `.srt`, `.txt`, `.md`, `.docx`) in the inbox
  folder, then run `/qale`. Or run `/qale add <path>`.
- Ask questions in plain words: "what did we promise this customer?", "who
  asked for CSV export?", "what changed since my last call with them?"
- Ask for the weekly update, prep for a meeting, help with an overdue
  promise, or what a set of interviews adds up to.
- `/qale review` shows what changed in memory. `/qale undo <batchId>` takes
  a batch back.

## Update

Claude Code, in a terminal:

```sh
claude plugin marketplace update qale
claude plugin update qale@qale
```

Codex CLI and the ChatGPT desktop app, in a terminal:

```sh
npx -y @openai/codex plugin marketplace upgrade qale
```

Then quit the app and open it again, and start a new session or chat. In
the Claude desktop app, if the new version does not show, uninstall Qale on
the Plugins page and install it again. Your memory stays where it is.

## Privacy

See [PRIVACY.md](PRIVACY.md). In short: Qale keeps your memory on your
disk and sends nothing to us.

## Licence

See [LICENSE](LICENSE). Qale is free to use during the beta. It is not open source.
