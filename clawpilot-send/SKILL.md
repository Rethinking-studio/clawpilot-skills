---
name: clawpilot-send
description: Use when the current OpenClaw conversation is PocketClaw or the legacy ClawAI Relay and the user wants a local or generated file sent back to mobile. Use a single high-level ClawPilot send command and do not expose session routing details.
---

# ClawPilot Send

Use this skill only for PocketClaw conversations, including older sessions that still show the legacy `ClawAI Relay` label.

Treat the conversation as relay/mobile only when the current session context clearly shows `PocketClaw`, `ClawAI Relay`, or an equivalent relay label. If you cannot confirm that, do not guess.

## When To Use

Use this skill when all of the following are true:

- The current conversation is clearly PocketClaw / relay.
- The user wants a local or generated file sent back to mobile.
- The file already exists, or you can create it first.

Do not use this skill for Telegram, Discord, Slack, WhatsApp, email, or other normal outbound channels.

## Required Action

1. Resolve the absolute local file path.
2. Run:

```bash
clawpilot send "/absolute/path/to/file"
```

## Rules

- Always use an absolute path.
- Only send files smaller than 20 MB.
- If the file is larger than 20 MB, do not call `clawpilot send`; tell the user it is too large for PocketClaw delivery.
- Let `clawpilot` handle upload, routing, and assistant-message delivery internally.
- Do not mention `sessionKey`, `runId`, relay routes, or low-level payloads.
- Do not invent `clawpilot send` flags or options.
- If multiple files should be sent, send them one by one unless the current workflow clearly supports bundling.

## Fallback

If the conversation is not clearly PocketClaw / relay:

- Do not call `clawpilot send`.
- Reply normally for the current channel.

## Incorrect Behavior

- Replying only with a local file path
- Uploading a file without delivering it through `clawpilot send`
- Guessing that a session is PocketClaw without confirming the session label
