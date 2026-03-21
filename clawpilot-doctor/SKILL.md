---
name: clawpilot-doctor
description: Use when the user wants to diagnose or repair ClawPilot or OpenClaw host issues, including status checks, logs, restart, update, and self-repair. Focus on the concrete blocking issue and the next corrective action.
---

# ClawPilot Doctor

Use this skill for ClawPilot and OpenClaw host troubleshooting.

## When To Use

Use this skill when the user asks to:

- Check ClawPilot or Gateway status
- Read logs
- Restart the Gateway
- Update OpenClaw
- Run repair or diagnostics

Do not use this skill to generate a pairing code or send files back to PocketClaw.

## Preferred Actions

Use the smallest action that answers the question or fixes the issue:

- `clawpilot status`
- `clawpilot restart`
- Host commands already exposed through the current OpenClaw or relay setup
- Confirmed repair commands such as OpenClaw self-repair paths

## Output Rules

- Report what you checked.
- Report what failed or passed.
- If blocked, give the next command to run.
- Keep the result concrete and operational, not theoretical.

## Do Not

- Do not jump into pairing unless the user explicitly wants pairing.
- Do not modify unrelated config when a status or log check is enough.
- Do not claim a repair is complete without verifying the result.
