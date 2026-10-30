# Qale

Qale is a product memory for product managers. It reads your meeting
transcripts and keeps what was decided, promised, asked for and changed, so
that you can ask about it later: what did we promise Nordkap, who needs to
know about this change, what do the customer calls add up to.

Qale runs on your computer. Your memory is a folder of markdown files on
your disk.

## What you need

- Claude Code, or the Claude desktop app with Cowork.
- Node.js 22.13 or later on your `PATH`. Check with `node --version`.
- A Qale licence key.

## Install

In Claude Code:

```
/plugin marketplace add qale-ai/qale-plugin
/plugin install qale@qale
/qale-setup
```

`/qale-setup` asks for your memory folder, your inbox folder, your licence
key and your name. It can also add demo data, so that you can try Qale
before you give it real meetings.

## Use

- Put transcripts (`.vtt`, `.srt`, `.txt`, `.md`, `.docx`) in the inbox
  folder, then run `/qale`. Or run `/qale add <path>`.
- Ask questions in plain words: "what did we promise Kranelund?", "who
  asked for CSV export?", "what changed since my last call with Sara?"
- Ask for the weekly update, prep for a meeting, help with an overdue
  promise, or what a set of interviews adds up to.
- `/qale review` shows what changed in memory. `/qale undo <batchId>` takes
  a batch back.

## Update

```sh
claude plugin marketplace update qale
claude plugin update qale@qale
```

Then start a new session.

## Privacy

See [PRIVACY.md](PRIVACY.md). In short: Qale keeps your memory on your
disk and sends nothing to us.

## Licence

See [LICENSE](LICENSE). Qale is free to use during the beta. It is not open source.
