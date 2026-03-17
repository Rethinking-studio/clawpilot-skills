---
name: clawpilot-send-file
description: Use when the current OpenClaw conversation is the PocketClaw chat and the user asks to send a local or generated file back to mobile. Confirm the session is PocketClaw or the legacy ClawAI Relay label, then use a single high-level ClawPilot send command. Do not expose session routing details to the model or user.
---

# ClawPilot Send File

Use this skill only for PocketClaw conversations, including older sessions that still show the legacy `ClawAI Relay` label.

Treat the conversation as a relay conversation only when the current session context clearly shows `PocketClaw`, the legacy `ClawAI Relay`, or an equivalent relay/mobile label. If you cannot confirm that, do not guess.

## When To Use

Use this skill when all of the following are true:

- The current conversation is clearly identified as `PocketClaw` or the legacy `ClawAI Relay`.
- The user wants a local file or generated file sent back to the mobile client.
- The file exists now, or you can create it first on the host machine.
- You can determine that the current conversation is PocketClaw / relay.

Do not use this skill for Telegram, Discord, Slack, WhatsApp, email, or any other normal outbound channel.

## Required Action

When the current conversation is `PocketClaw` or the legacy `ClawAI Relay`, do not reply with a raw local path. Instead:

1. Resolve the absolute local file path.
2. Run:

```bash
clawpilot send "/absolute/path/to/file"
```

Rules:

- Always use an absolute path.
- Only send files smaller than 20 MB.
- If the file is larger than 20 MB, do not call `clawpilot send`; tell the user the file is too large to send through PocketClaw.
- Let `clawpilot` handle upload, session routing, and assistant-message delivery internally.
- Keep the reply short after sending.
- Do not tell the user to look up a local file path if upload is available.
- Do not mention `sessionKey`, `runId`, internal relay routes, or low-level message payloads.
- Do not invent `clawpilot send` flags or options.
- If multiple files should be sent, send them one by one unless the current relay workflow clearly supports multiple attachments in one send.

## Fallback

If the conversation is not clearly `PocketClaw` or `ClawAI Relay`, or you cannot verify the session label:

- Do not call `clawpilot send`.
- Reply normally for the current channel.
- If needed, tell the user you can provide the local path instead.

## Examples

Correct:

1. The conversation context shows `PocketClaw`.
2. The user asks: `把桌面的 report.png 传给我`
3. Run:

```bash
clawpilot send "/Users/you/Desktop/report.png"
```

4. Then reply briefly that the file has been sent.

Also correct:

1. Generate `/Users/you/Desktop/chart.png`
2. Run:

```bash
clawpilot send "/Users/you/Desktop/chart.png"
```

3. Then reply that the chart has been sent.

Incorrect:

- Replying only with `/Users/you/Desktop/report.png`
- Uploading the file but never delivering it back through `clawpilot send`
- Sending a normal text reply instead of a structured attachment when the request is clearly to send the file back
- Using `clawpilot upload` when the session is Telegram or Discord
- Guessing that a session is relay without seeing `PocketClaw` or the legacy `ClawAI Relay`
- Trying to send a file larger than 20 MB instead of telling the user it is too large
