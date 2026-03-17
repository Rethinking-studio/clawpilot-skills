# ClawPilot Send File Skill

A small skill for relay conversations that need to send a local file back to PocketClaw.

## What It Does

When the current OpenClaw conversation is clearly the PocketClaw relay chat, this skill tells the model to use:

```bash
clawpilot send "/absolute/path/to/file"
```

The command handles:

- uploading the file to OSS
- resolving the latest relay conversation session
- sending the file back as an assistant attachment message

## When To Use

Use this skill only when all of the following are true:

- the current conversation is clearly PocketClaw
- the user wants a local or generated file sent back to mobile
- the file already exists, or can be created locally first

Do not use it for normal outbound channels like Telegram, Discord, Slack, WhatsApp, or email.

## Files

- `SKILL.md`: the skill instructions

## Example

```bash
clawpilot send "/Users/you/Desktop/report.png"
```

## Notes

- Always use an absolute path.
- Only send files smaller than 20 MB.
- If a file is larger than 20 MB, tell the user it is too large instead of calling `clawpilot send`.
- Do not expose internal routing details like `sessionKey` or `runId`.
- Send files one by one unless your relay workflow explicitly supports batching.
